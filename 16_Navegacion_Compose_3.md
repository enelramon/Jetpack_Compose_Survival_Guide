# Guía de Navegación en Jetpack Compose (Type-Safe)

Esta guía explica cómo implementar la navegación en Jetpack Compose utilizando el enfoque moderno "Type-Safe" con Navigation 3 (v1.1.3) y Kotlin Serialization, tal como se utiliza en este proyecto.

## 1. Dependencias Necesarias

Para utilizar la navegación segura por tipos en Navigation 3, sustituimos la librería monolítica anterior por los módulos oficiales `runtime` y `ui` fijados en la versión **1.1.3**, manteniendo el plugin de serialización de JSON.

### Configuración en gradle/libs.versions.toml

```toml
[versions]
navigation3 = "1.1.3"
kotlinSerialization = "2.0.21"

[libraries]
androidx-navigation3-runtime = { group = "androidx.navigation3", name = "navigation3-runtime", version.ref = "navigation3" }
androidx-navigation3-ui = { group = "androidx.navigation3", name = "navigation3-ui", version.ref = "navigation3" }
kotlinx-serialization-json = { group = "org.jetbrains.kotlinx", name = "kotlinx-serialization-json", version = "1.7.3" }

[plugins]
kotlin-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlinSerialization" }
```

### Configuración en build.gradle.kts (Module: app)

```kotlin
plugins {
    alias(libs.plugins.kotlin.serialization)
}

dependencies {
    implementation(libs.androidx.navigation3.runtime)
    implementation(libs.androidx.navigation3.ui)
    implementation(libs.kotlinx.serialization.json)
}
```

## 2. Definir las Rutas (Screen.kt)

En lugar de cadenas de texto hardcodeadas, definimos una clase sellada donde cada pantalla implementa obligatoriamente la interfaz nativa `NavKey` y lleva la anotación `@Serializable`. Pantallas sin argumentos son objetos (`data object`) y con argumentos son clases de datos (`data class`).

```kotlin
package edu.ucne.repaso.presentation.navigation

import androidx.navigation3.runtime.NavKey
import kotlinx.serialization.Serializable

@Serializable
sealed class Screen : NavKey {
    // Pantalla sin argumentos
    @Serializable
    data object BancoList : Screen()

    // Pantalla con argumentos (ej. un ID)
    @Serializable
    data class BancoEdit(val bancoId: Int = 0) : Screen()
}
```

## 3. Configurar el Contenedor (NavDisplay)

El contenedor donde se intercambian las pantallas pasa a ser `NavDisplay`. La pila de retroceso se declara con `rememberNavBackStack` para hacerla inmune a rotaciones, y se utiliza la función `entryProvider` para registrar las vistas.

```kotlin
package edu.ucne.repaso.presentation.navigation

import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.navigation3.runtime.entryProvider
import androidx.navigation3.runtime.rememberNavBackStack
import androidx.navigation3.ui.NavDisplay
import edu.ucne.repaso.presentation.registrosbancos.edit.EditBancoScreen
import edu.ucne.repaso.presentation.registrosbancos.list.BancoListScreen

@Composable
fun MainNavigationDisplay() {
    val backStack = rememberNavBackStack(Screen.BancoList)

    NavDisplay(
        backStack = backStack,
        modifier = Modifier.fillMaxSize(),
        entryProvider = entryProvider {
            
            // 1. Definición de una pantalla simple
            entry<Screen.BancoList> {
                BancoListScreen(
                    onAddBanco = { backStack.add(Screen.BancoEdit(0)) },
                    onNavigateToEdit = { id -> backStack.add(Screen.BancoEdit(id)) }
                )
            }

            // 2. Definición de una pantalla con argumentos
            entry<Screen.BancoEdit> { key ->
                EditBancoScreen(
                    bancoId = key.bancoId,
                    onBack = {
                        if (backStack.isNotEmpty()) {
                            backStack.removeAt(backStack.size - 1)
                        }
                    }
                )
            }
        }
    )
}
```

## 4. Navegar entre Pantallas

Para navegar, simplemente opera sobre la colección de la pila (`backStack`) añadiendo o eliminando instancias exactas de los objetos de tu ruta.

```kotlin
// Navegar a una pantalla sin argumentos
backStack.add(Screen.BancoList)

// Navegar a una pantalla CON argumentos
backStack.add(Screen.BancoEdit(bancoId = 5))

// Volver atrás
backStack.removeAt(backStack.size - 1)
```

## 5. Recibir Argumentos en el Destino

Al prescindir del contenedor opaco del framework (`SavedStateHandle`), el ViewModel se limpia por completo. La pantalla extrae el parámetro tipado de forma nativa e inicia la reactividad mediante `LaunchedEffect`.

> **Desacoplamiento Total:** El EditBancoScreen extrae el ID de la ruta y activa la carga en el ViewModel. El ViewModel se mantiene puro y sin librerías de navegación.

### Implementación en la Pantalla (EditBancoScreen.kt)
```kotlin
entry<Screen.BancoEdit> { key -> 
    val id: Int = key.bancoId // ID extraído 100% Type-Safe
    
    EditBancoScreen(
        bancoId = id,
        onBack = {
            if (backStack.isNotEmpty()) {
                backStack.removeAt(backStack.size - 1)
            }
        }
    )
}
```

### Implementación en el ViewModel (EditBancoViewModel.kt)
```kotlin
package edu.ucne.repaso.presentation.registrosbancos.edit

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import dagger.hilt.android.lifecycle.HiltViewModel
import edu.ucne.repaso.domain.registrosbancos.model.Banco
import edu.ucne.repaso.domain.registrosbancos.usecase.*
import kotlinx.coroutines.flow.*
import kotlinx.coroutines.launch
import java.time.LocalDate
import javax.inject.Inject

@HiltViewModel
class EditBancoViewModel @Inject constructor(
    private val getBancoUseCase: GetBancoUseCase,
    private val upsertBancoUseCase: UpsertBancoUseCase,
    private val deleteBancoUseCase: DeleteBancoUseCase
) : ViewModel() {

    private val _state = MutableStateFlow(EditBancoUiState())
    val state: StateFlow<EditBancoUiState> = _state.asStateFlow()

    fun onEvent(event: EditBancoUiEvent) {
        when (event) {
            is EditBancoUiEvent.Load -> loadBanco(event.id)
            /* ... resto de eventos ... */
        }
    }

    private fun loadBanco(id: Int?) {
        /* ... lógica pura sin SavedStateHandle ... */
    }
}
```

## Resumen de Buenas Prácticas

* **Pila Persistente:** Utiliza siempre `rememberNavBackStack`. Es el único gestor nativo de la librería que garantiza que el usuario no regrese al inicio al rotar la pantalla.
* **Eventos hacia arriba (State Hoisting):** Pasa lambdas (ej. `onAddBanco = { ... }`) desde tus pantallas hacia el contenedor principal. Nunca pases el objeto `backStack` a tus Composables individuales.
* **ViewModels Puros:** Al eliminar `SavedStateHandle`, tus ViewModels se convierten en clases de Kotlin estándar, facilitando la creación de pruebas unitarias (Unit Tests) al no depender del framework de navegación para recibir argumentos.
