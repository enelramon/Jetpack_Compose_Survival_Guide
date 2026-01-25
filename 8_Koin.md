# Koin DI: Android Quick Reference (Beginner)

This is a concise guide to core Koin DI concepts for Android development.

---

## 1. Gradle Setup

Add to your `build.gradle.kts` (Module :app):

```kotlin
dependencies {
    // Koin for Android
    implementation("io.insert-koin:koin-android:LATEST_VERSION")
    // Optional: Koin for Jetpack Compose (if you use Compose)
    // implementation("io.insert-koin:koin-androidx-compose:LATEST_VERSION")
    // Optional: Koin for WorkManager
    // implementation("io.insert-koin:koin-androidx-workmanager:LATEST_VERSION")
}
