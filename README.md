# Kotlin and Jetpack Compose Survival Guide

## Índice

1. [Introducción](#introducción)
1. #### Kotlin Basics
   1. [Kotlin Introduction](./1_Kotlin_Intro.md)
   1. [Kotlin Control Flow](./2_Kotlin_Control_Flow.md)
   1. [Kotlin Functions](./3_Kotlin_Functions.md)
   1. [Kotlin OOP](./4_Kotlin_OOP.md)
   1. [Kotlin Exception Handling](./5_Kotlin_Exception_Handling.md)
   1. [Kotlin Collections](./6_Kotlin_Collections.md)
   1. [Kotlin Testing](./7_Testing.md)
1. [Errores comunes](#errores-comunes)
   1. [Configuración del entorno](#configuración-del-entorno)
   1. [Problemas de diseño en UI](#problemas-de-diseño-en-ui)
   1. [Errores con bases de datos](#errores-con-bases-de-datos)
   1. [Errores al implementar lógica](#errores-al-implementar-lógica)
1. [Recursos adicionales](#recursos-adicionales)

---

## Introducción
Bienvenido a la guía de supervivencia para estudiantes que trabajan con Android. Aquí encontrarás los errores más comunes y sus respectivas soluciones.

## Errores comunes

### Configuración del entorno
Descripción de errores como: 
- Problemas al instalar Android Studio.
- Versiones incompatibles de SDK.
- Codificar JSON y otros formatos en Kotlin

### Problemas de diseño en UI
Errores relacionados con ConstraintLayout, vistas, etc.

### Errores con bases de datos
Problemas al usar Room, SQLite o APIs de persistencia.

### Errores al implementar lógica
Errores en ViewModel, LiveData o navegación.

## Soluciones
Detallaremos las soluciones paso a paso en cada sección.
Detallaremos las soluciones paso a paso en cada sección.
- Para pode codificar JSON y otros formatos en Kotlin debemos agregar la siguiente bibloteca manualmente:
      dependencies {
            implementation(libs.kotlinx.serialization.json)
      }
en la sigueinte ruta: "build.gradle.kts (module :app)", Los siguientes plugins:
      plugins {
         kotlin("jvm") version "2.1.0" apply false
         kotlin("plugin.serialization") version "2.1.0" apply false
      }
en la ruta: "build.gradle.kts (project :nombredetuproyecto)".
![image](https://github.com/user-attachments/assets/c965b1e1-d006-4cc1-9a1f-299acf31f584)


## Recursos adicionales
- [Documentación oficial de Android](https://developer.android.com/docs)
- [Foro de Stack Overflow](https://stackoverflow.com/)
