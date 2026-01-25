# Guía de Navegación en Jetpack Compose (Type-Safe)

Esta guía explica cómo implementar la navegación en Jetpack Compose utilizando el enfoque moderno "Type-Safe" con Kotlin Serialization, tal como se utiliza en este proyecto.

## 1. Dependencias Necesarias

Para utilizar la navegación segura por tipos, asegúrate de tener las siguientes dependencias en tu `libs.versions.toml` o `build.gradle.kts`:

- **Navigation Compose**: `androidx.navigation:navigation-compose`
- **Kotlin Serialization**: Plugin y librería de JSON.

En el `build.gradle.kts` (Module: app):
```kotlin
plugins {
    alias(libs.plugins.jetbrains.kotlin.serialization)
}

dependencies {
    implementation(libs.androidx.navigation.compose)
    implementation(libs.kotlinx.serialization.json)
}
```

## 2. Definir las Rutas (Screen.kt)

En lugar de usar cadenas de texto hardcodeadas (como `"home/{id}"`), definimos un `sealed class` donde cada pantalla es un objeto (`data object`) o una clase de datos (`data class`).

Deben estar anotadas con `@Serializable`.

**Ejemplo (`Screen.kt`):**

```kotlin
import kotlinx.serialization.Serializable

sealed class Screen {
    // Pantalla sin argumentos
    @Serializable
    data object TicketList : Screen()

    // Pantalla con argumentos (ej. un ID)
    @Serializable
    data class Ticket(val ticketId: Int) : Screen()
    
    @Serializable
    data object SistemaList : Screen()
}
```

## 3. Configurar el NavHost

El `NavHost` es el contenedor donde se intercambian las pantallas. Se configura en una función Composable (por ejemplo, `DemoAp2NavHost`).

Se utiliza la función `composable<T>` para definir el destino.

**Ejemplo (`DemoAp2NavHost.kt`):**

```kotlin
@Composable
fun DemoAp2NavHost(
    navHostController: NavHostController
) {
    NavHost(
        navController = navHostController,
        startDestination = Screen.TicketList // Ruta inicial
    ) {
        
        // 1. Definición de una pantalla simple
        composable<Screen.TicketList> {
            TicketListScreen(
                // Evento para navegar a otra pantalla
                goToTicket = { id ->
                    navHostController.navigate(Screen.Ticket(id))
                },
                createTicket = {
                    navHostController.navigate(Screen.Ticket(0))
                }
            )
        }

        // 2. Definición de una pantalla con argumentos
        composable<Screen.Ticket> { backStackEntry ->
            // Si necesitamos acceder a los argumentos manualmente (opcional, 
            // generalmente pasamos la función de navegación y el ViewModel se encarga)
            // val args = backStackEntry.toRoute<Screen.Ticket>()
            
            TicketScreen(
                goBack = {
                    navHostController.navigateUp() // Volver atrás
                }
            )
        }
    }
}
```

## 4. Navegar entre Pantallas

Para navegar, simplemente llama a `navigate` pasando una instancia del objeto de tu ruta.

```kotlin
// Navegar a una pantalla sin argumentos
navController.navigate(Screen.TicketList)

// Navegar a una pantalla CON argumentos
navController.navigate(Screen.Ticket(ticketId = 5))
```

## 5. Recibir Argumentos en el Destino

Gracias a Navigation Compose y Hilt, a menudo no necesitas extraer los argumentos manualmente en el `NavHost`. Inyectas `SavedStateHandle` en tu ViewModel.

**En el ViewModel:**

```kotlin
@HiltViewModel
class TicketViewModel @Inject constructor(
    savedStateHandle: SavedStateHandle,
    private val repository: TicketRepository
) : ViewModel() {

    // Los argumentos de la ruta están disponibles en savedStateHandle
    // usando la navegación Type-Safe, navigation-compose extrae los valores automáticamente.
    // También puedes obtener el objeto ruta completo:
    
    val args = savedStateHandle.toRoute<Screen.Ticket>()
    val ticketId = args.ticketId

    init {
        if (ticketId > 0) {
            // Cargar ticket
        }
    }
}
```

## Resumen de Buenas Prácticas
1.  **Hilt + SavedStateHandle**: Deja que el ViewModel recupere los datos de navegación, en lugar de pasarlos como parámetros a través de todos los Composables, si es posible.
2.  **Eventos hacia arriba**: Pasa lambdas (`onClick = { ... }`) desde tus pantallas hacia el `NavHost` para manejar la navegación allí. No pases el `navController` a tus pantallas individuales.
