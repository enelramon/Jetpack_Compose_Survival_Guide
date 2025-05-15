# Hilt DI: Android Quick Reference (Beginner)

This is a concise guide to core Hilt DI concepts for Android development. Hilt is built on top of Dagger.

---

## 1. Gradle Setup

Add to your `build.gradle.kts` files:

* **Project-level `build.gradle.kts`:**
    ```kotlin
    plugins {
        id("com.google.dagger.hilt.android") version "LATEST_HILT_VERSION" apply false
        id("com.google.devtools.ksp") version "LATEST_KSP_VERSION" apply false // Or your KSP version
    }
    ```

* **App-level `build.gradle.kts`:**
    ```kotlin
    plugins {
        id("com.google.dagger.hilt.android")
        id("com.google.devtools.ksp") // Preferred over kapt
    }

    android {
        // ...
    }

    dependencies {
        implementation("com.google.dagger:hilt-android:LATEST_HILT_VERSION")
        ksp("com.google.dagger:hilt-compiler:LATEST_HILT_VERSION") // Use ksp

        // For Jetpack Compose ViewModels (optional)
        // implementation("androidx.hilt:hilt-navigation-compose:LATEST_COMPOSE_NAV_VERSION")
    }
    ```
    *Replace `LATEST_HILT_VERSION`, `LATEST_KSP_VERSION`, etc., with actual latest versions from official documentation.*

---

## 2. Initialization (in `Application` class)

Annotate your `Application` class with `@HiltAndroidApp`.

```kotlin
import android.app.Application
import dagger.hilt.android.HiltAndroidApp

@HiltAndroidApp
class MyApplication : Application() {
    // Hilt will generate the necessary Dagger components
}
