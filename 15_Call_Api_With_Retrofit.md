# **Talk with your api**


## **1\. Configuración (build.gradle.kts)**

Para usar **Type-Safe Navigation**, necesitas agregar el plugin de serialización y actualizar la versión de navegación.

### **Plugins block**

`plugins {`  
    `id("com.android.application")`  
    `id("org.jetbrains.kotlin.android")`  
    `id("com.google.dagger.hilt.android")`  
    `id("com.google.devtools.ksp")`  
    `// Necesario para Type-Safe Navigation`  
    `kotlin("plugin.serialization") version "1.9.22"`  
`}`

### **Dependencies block**

`dependencies {`  
    `// Android & Compose`  
    `implementation("androidx.core:core-ktx:1.12.0")`  
    `implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")`  
    `implementation("androidx.activity:activity-compose:1.8.2")`  
    `implementation(platform("androidx.compose:compose-bom:2024.02.00")) // BOM más reciente`  
    `implementation("androidx.compose.ui:ui")`  
    `implementation("androidx.compose.material3:material3")`  
      
    `// Navegación Type-Safe (Requiere 2.8.0+)`  
    `implementation("androidx.navigation:navigation-compose:2.8.0-beta02")`  
    `implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.6.3")`  
      
    `// Imagenes`  
    `implementation("io.coil-kt:coil-compose:2.6.0")`

    `// Hilt (Inyección de Dependencias)`  
    `implementation("com.google.dagger:hilt-android:2.50")`  
    `kapt("com.google.dagger:hilt-android-compiler:2.50")`  
    `implementation("androidx.hilt:hilt-navigation-compose:1.2.0")`

    `// Room (Base de datos local)`  
    `implementation("androidx.room:room-runtime:2.6.1")`  
    `implementation("androidx.room:room-ktx:2.6.1")`  
    `kapt("androidx.room:room-compiler:2.6.1")`

    `// Retrofit (Red)`  
    `implementation("com.squareup.retrofit2:retrofit:2.9.0")`  
    `implementation("com.squareup.retrofit2:converter-gson:2.9.0")`  
`}`

## **2\. Capa de Dominio (Domain Layer)**

Esta capa es pura y no conoce detalles de Android o bases de datos.

### **Modelo (Character.kt)**

`package com.example.dragonball.domain.model`

`data class DragonBallCharacter(`  
    `val id: Int,`  
    `val name: String,`  
    `val ki: String,`  
    `val race: String,`  
    `val gender: String,`  
    `val description: String,`  
    `val image: String,`  
    `val maxKi: String`  
`)`

### **Resource State (Resource.kt)**

Tal como aparece en el PDF para manejar estados de carga y error.  
`package com.example.dragonball.common`

`sealed class Resource<T>(val data: T? = null, val message: String? = null) {`  
    `class Success<T>(data: T) : Resource<T>(data)`  
    `class Error<T>(message: String, data: T? = null) : Resource<T>(data, message)`  
    `class Loading<T>(data: T? = null) : Resource<T>(data)`  
`}`

### **Repositorio Interface (CharacterRepository.kt)**

`package com.example.dragonball.domain.repository`

`import com.example.dragonball.common.Resource`  
`import com.example.dragonball.domain.model.DragonBallCharacter`  
`import kotlinx.coroutines.flow.Flow`

`interface CharacterRepository {`  
    `// Observa la base de datos local (Single Source of Truth)`  
    `fun getCharacters(): Flow<List<DragonBallCharacter>>`  
      
    `// Obtiene detalle (intenta local, si no existe o es viejo, busca remoto)`  
    `suspend fun getCharacterDetail(id: Int): Resource<DragonBallCharacter>`  
      
    `// Sincroniza datos remotos a locales`  
    `suspend fun syncSaiyans(): Resource<Unit>`  
`}`

### **Use Cases**

El PDF enfatiza el uso de Use Cases para la lógica de negocio.  
**GetSaiyansUseCase.kt**  
`package com.example.dragonball.domain.usecase`

`import com.example.dragonball.domain.repository.CharacterRepository`  
`import javax.inject.Inject`

`class GetSaiyansUseCase @Inject constructor(`  
    `private val repository: CharacterRepository`  
`) {`  
    `// Retorna el Flow de la DB`  
    `operator fun invoke() = repository.getCharacters()`  
`}`

**SyncSaiyansUseCase.kt**  
`package com.example.dragonball.domain.usecase`

`import com.example.dragonball.domain.repository.CharacterRepository`  
`import javax.inject.Inject`

`class SyncSaiyansUseCase @Inject constructor(`  
    `private val repository: CharacterRepository`  
`) {`  
    `suspend operator fun invoke() = repository.syncSaiyans()`  
`}`

**GetCharacterDetailUseCase.kt**  
`package com.example.dragonball.domain.usecase`

`import com.example.dragonball.domain.repository.CharacterRepository`  
`import javax.inject.Inject`

`class GetCharacterDetailUseCase @Inject constructor(`  
    `private val repository: CharacterRepository`  
`) {`  
    `suspend operator fun invoke(id: Int) = repository.getCharacterDetail(id)`  
`}`

## **3\. Capa de Datos (Data Layer)**

### **Retrofit API (DragonBallApi.kt)**

Definimos los endpoints solicitados.  
`package com.example.dragonball.data.remote`

`import com.google.gson.annotations.SerializedName`  
`import retrofit2.Response`  
`import retrofit2.http.GET`  
`import retrofit2.http.Path`  
`import retrofit2.http.Query`

`interface DragonBallApi {`  
    `@GET("characters")`  
    `suspend fun getSaiyans(`  
        `@Query("page") page: Int = 1,`  
        `@Query("limit") limit: Int = 10,`  
        `@Query("name") name: String = "g",`  
        `@Query("gender") gender: String = "Male",`  
        `@Query("race") race: String = "Saiyan"`  
    `): Response<CharacterResponseDto>`

    `@GET("characters/{id}")`  
    `suspend fun getCharacterDetail(@Path("id") id: Int): Response<CharacterDto>`  
`}`

`// DTOs`  
`data class CharacterResponseDto(`  
    `@SerializedName("items") val items: List<CharacterDto>`  
`)`

`data class CharacterDto(`  
    `val id: Int,`  
    `val name: String,`  
    `val ki: String,`  
    `val race: String,`  
    `val gender: String,`  
    `val description: String,`  
    `val image: String,`  
    `val maxKi: String`  
`) {`  
    `fun toDomain() = com.example.dragonball.domain.model.DragonBallCharacter(`  
        `id, name, ki, race, gender, description, image, maxKi`  
    `)`  
      
    `fun toEntity() = com.example.dragonball.data.local.CharacterEntity(`  
        `id, name, ki, race, gender, description, image, maxKi`  
    `)`  
`}`

### **Room Database (AppDatabase.kt & Dao)**

**CharacterEntity.kt**  
`package com.example.dragonball.data.local`

`import androidx.room.Entity`  
`import androidx.room.PrimaryKey`  
`import com.example.dragonball.domain.model.DragonBallCharacter`

`@Entity(tableName = "characters")`  
`data class CharacterEntity(`  
    `@PrimaryKey val id: Int,`  
    `val name: String,`  
    `val ki: String,`  
    `val race: String,`  
    `val gender: String,`  
    `val description: String,`  
    `val image: String,`  
    `val maxKi: String`  
`) {`  
    `fun toDomain() = DragonBallCharacter(`  
        `id, name, ki, race, gender, description, image, maxKi`  
    `)`  
`}`

**CharacterDao.kt**  
`package com.example.dragonball.data.local`

`import androidx.room.Dao`  
`import androidx.room.Insert`  
`import androidx.room.OnConflictStrategy`  
`import androidx.room.Query`  
`import kotlinx.coroutines.flow.Flow`

`@Dao`  
`interface CharacterDao {`  
    `@Query("SELECT * FROM characters")`  
    `fun observeCharacters(): Flow<List<CharacterEntity>>`

    `@Query("SELECT * FROM characters WHERE id = :id")`  
    `suspend fun getCharacterById(id: Int): CharacterEntity?`

    `@Insert(onConflict = OnConflictStrategy.REPLACE)`  
    `suspend fun upsertAll(characters: List<CharacterEntity>)`  
      
    `@Insert(onConflict = OnConflictStrategy.REPLACE)`  
    `suspend fun upsert(character: CharacterEntity)`  
`}`

### **Implementación del Repositorio (CharacterRepositoryImpl.kt)**

Aquí reside la lógica **Offline-First**. Observamos la DB y sincronizamos la red aparte.  
`package com.example.dragonball.data.repository`

`import com.example.dragonball.common.Resource`  
`import com.example.dragonball.data.local.CharacterDao`  
`import com.example.dragonball.data.remote.DragonBallApi`  
`import com.example.dragonball.domain.model.DragonBallCharacter`  
`import com.example.dragonball.domain.repository.CharacterRepository`  
`import kotlinx.coroutines.flow.Flow`  
`import kotlinx.coroutines.flow.map`  
`import javax.inject.Inject`

`class CharacterRepositoryImpl @Inject constructor(`  
    `private val api: DragonBallApi,`  
    `private val dao: CharacterDao`  
`) : CharacterRepository {`

    `// 1. La UI siempre observa la base de datos (Flow)`  
    `override fun getCharacters(): Flow<List<DragonBallCharacter>> {`  
        `return dao.observeCharacters().map { entities ->`  
            `entities.map { it.toDomain() }`  
        `}`  
    `}`

    `// 2. Este método se llama para actualizar la base de datos desde la red`  
    `override suspend fun syncSaiyans(): Resource<Unit> {`  
        `return try {`  
            `val response = api.getSaiyans(limit = 5) // Traemos 5 para probar`  
            `if (response.isSuccessful && response.body() != null) {`  
                `val characters = response.body()!!.items`  
                `// Guardamos en local, esto disparará el Flow automáticamente`  
                `dao.upsertAll(characters.map { it.toEntity() })`  
                `Resource.Success(Unit)`  
            `} else {`  
                `Resource.Error(response.message())`  
            `}`  
        `} catch (e: Exception) {`  
            `Resource.Error(e.localizedMessage ?: "Error de conexión")`  
        `}`  
    `}`

    `override suspend fun getCharacterDetail(id: Int): Resource<DragonBallCharacter> {`  
        `// Estrategia: Buscar local primero, si no, buscar remoto`  
        `val local = dao.getCharacterById(id)`  
        `if (local != null) {`  
             `// Podríamos añadir lógica para verificar si el dato es viejo aquí`  
            `return Resource.Success(local.toDomain())`  
        `}`

        `return try {`  
            `val response = api.getCharacterDetail(id)`  
            `if (response.isSuccessful && response.body() != null) {`  
                `val remoteChar = response.body()!!`  
                `dao.upsert(remoteChar.toEntity())`  
                `Resource.Success(remoteChar.toDomain())`  
            `} else {`  
                `Resource.Error("Error al obtener detalle")`  
            `}`  
        `} catch (e: Exception) {`  
            `Resource.Error(e.message ?: "Error desconocido")`  
        `}`  
    `}`  
`}`

## **4\. Inyección de Dependencias (Hilt Modules)**

`@Module`  
`@InstallIn(SingletonComponent::class)`  
`object AppModule {`

    `@Provides`  
    `@Singleton`  
    `fun provideApi(): DragonBallApi {`  
        `return Retrofit.Builder()`  
            `.baseUrl("[https://dragonball-api.com/api/](https://dragonball-api.com/api/)")`  
            `.addConverterFactory(GsonConverterFactory.create())`  
            `.build()`  
            `.create(DragonBallApi::class.java)`  
    `}`

    `@Provides`  
    `@Singleton`  
    `fun provideDatabase(@ApplicationContext context: Context): AppDatabase {`  
        `return Room.databaseBuilder(`  
            `context,`  
            `AppDatabase::class.java,`  
            `"dragon_ball.db"`  
        `).build()`  
    `}`

    `@Provides`  
    `@Singleton`  
    `fun provideRepository(api: DragonBallApi, db: AppDatabase): CharacterRepository {`  
        `return CharacterRepositoryImpl(api, db.characterDao())`  
    `}`  
`}`

## **5\. Capa de UI (MVI Pattern & Type-Safe Nav)**

### **Definición de Rutas (Screen.kt)**

Definimos las pantallas como objetos serializables.  
`package com.example.dragonball.presentation.navigation`

`import kotlinx.serialization.Serializable`

`sealed interface Screen {`  
    `@Serializable`  
    `data object List : Screen`  
      
    `@Serializable`  
    `data class Detail(val id: Int) : Screen`  
`}`

### **ViewModel (ListViewModel.kt)**

Implementa el patrón MVI con State y Event.  
`package com.example.dragonball.presentation.list`

`import androidx.lifecycle.ViewModel`  
`import androidx.lifecycle.viewModelScope`  
`import com.example.dragonball.common.Resource`  
`import com.example.dragonball.domain.model.DragonBallCharacter`  
`import com.example.dragonball.domain.usecase.GetSaiyansUseCase`  
`import com.example.dragonball.domain.usecase.SyncSaiyansUseCase`  
`import dagger.hilt.android.lifecycle.HiltViewModel`  
`import kotlinx.coroutines.flow.*`  
`import kotlinx.coroutines.launch`  
`import javax.inject.Inject`

`data class ListUiState(`  
    `val isLoading: Boolean = false,`  
    `val characters: List<DragonBallCharacter> = emptyList(),`  
    `val error: String? = null`  
`)`

`sealed interface ListEvent {`  
    `data object Refresh : ListEvent`  
`}`

`@HiltViewModel`  
`class ListViewModel @Inject constructor(`  
    `private val getSaiyansUseCase: GetSaiyansUseCase,`  
    `private val syncSaiyansUseCase: SyncSaiyansUseCase`  
`) : ViewModel() {`

    `private val _state = MutableStateFlow(ListUiState())`  
    `val state = _state.asStateFlow()`

    `init {`  
        `observeLocalData()`  
        `onEvent(ListEvent.Refresh)`  
    `}`

    `fun onEvent(event: ListEvent) {`  
        `when(event) {`  
            `ListEvent.Refresh -> syncData()`  
        `}`  
    `}`

    `private fun observeLocalData() {`  
        `viewModelScope.launch {`  
            `getSaiyansUseCase().collect { chars ->`  
                `_state.update { it.copy(characters = chars) }`  
            `}`  
        `}`  
    `}`

    `private fun syncData() {`  
        `viewModelScope.launch {`  
            `_state.update { it.copy(isLoading = true, error = null) }`  
            `when (val result = syncSaiyansUseCase()) {`  
                `is Resource.Success -> _state.update { it.copy(isLoading = false) }`  
                `is Resource.Error -> _state.update { it.copy(isLoading = false, error = result.message) }`  
                `else -> {}`  
            `}`  
        `}`  
    `}`  
`}`

### **Screen 1: Lista de Personajes (ListScreen.kt)**

`@Composable`  
`fun ListScreen(`  
    `viewModel: ListViewModel = hiltViewModel(),`  
    `onCharacterClick: (Int) -> Unit`  
`) {`  
    `val state by viewModel.state.collectAsStateWithLifecycle()`  
    `val snackbarHostState = remember { SnackbarHostState() }`

    `LaunchedEffect(state.error) {`  
        `state.error?.let { snackbarHostState.showSnackbar("Offline mode: $it") }`  
    `}`

    `Scaffold(`  
        `snackbarHost = { SnackbarHost(snackbarHostState) },`  
        `topBar = { CenterAlignedTopAppBar(title = { Text("Saiyan Warriors") }) }`  
    `) { padding ->`  
        `Box(modifier = Modifier.padding(padding).fillMaxSize()) {`  
            `if (state.isLoading && state.characters.isEmpty()) {`  
                `CircularProgressIndicator(modifier = Modifier.align(Alignment.Center))`  
            `}`

            `LazyColumn(`  
                `contentPadding = PaddingValues(16.dp),`  
                `verticalArrangement = Arrangement.spacedBy(8.dp)`  
            `) {`  
                `items(state.characters) { char ->`  
                    `CharacterItem(char, onClick = { onCharacterClick(char.id) })`  
                `}`  
            `}`  
        `}`  
    `}`  
`}`

`@Composable`  
`fun CharacterItem(character: DragonBallCharacter, onClick: () -> Unit) {`  
    `Card(`  
        `modifier = Modifier.fillMaxWidth().clickable { onClick() },`  
        `elevation = CardDefaults.cardElevation(4.dp)`  
    `) {`  
        `Row(modifier = Modifier.padding(16.dp), verticalAlignment = Alignment.CenterVertically) {`  
            `AsyncImage(`  
                `model = character.image,`  
                `contentDescription = null,`  
                `modifier = Modifier.size(60.dp),`  
                `contentScale = ContentScale.Crop`  
            `)`  
            `Spacer(modifier = Modifier.width(16.dp))`  
            `Column {`  
                `Text(text = character.name, style = MaterialTheme.typography.titleMedium)`  
                `Text(text = "Ki: ${character.ki}", style = MaterialTheme.typography.bodySmall)`  
                `Text(text = character.race, style = MaterialTheme.typography.labelSmall, color = Color.Gray)`  
            `}`  
        `}`  
    `}`  
`}`

### **Screen 2: Detalle (DetailScreen.kt)**

**DetailViewModel.kt**  
Aquí usamos toRoute para obtener los argumentos de forma segura.  
`package com.example.dragonball.presentation.detail`

`import androidx.lifecycle.SavedStateHandle`  
`import androidx.lifecycle.ViewModel`  
`import androidx.lifecycle.viewModelScope`  
`import androidx.navigation.toRoute`  
`import com.example.dragonball.common.Resource`  
`import com.example.dragonball.domain.model.DragonBallCharacter`  
`import com.example.dragonball.domain.usecase.GetCharacterDetailUseCase`  
`import com.example.dragonball.presentation.navigation.Screen`  
`import dagger.hilt.android.lifecycle.HiltViewModel`  
`import kotlinx.coroutines.flow.MutableStateFlow`  
`import kotlinx.coroutines.flow.asStateFlow`  
`import kotlinx.coroutines.flow.update`  
`import kotlinx.coroutines.launch`  
`import javax.inject.Inject`

`@HiltViewModel`  
`class DetailViewModel @Inject constructor(`  
    `private val getCharacterDetailUseCase: GetCharacterDetailUseCase,`  
    `savedStateHandle: SavedStateHandle`  
`) : ViewModel() {`

    `private val _state = MutableStateFlow(DetailUiState())`  
    `val state = _state.asStateFlow()`

    `init {`  
        `// Recuperación Type-Safe del argumento`  
        `val args = savedStateHandle.toRoute<Screen.Detail>()`  
        `loadCharacter(args.id)`  
    `}`

    `private fun loadCharacter(id: Int) = viewModelScope.launch {`  
        `_state.update { it.copy(isLoading = true) }`  
        `when(val result = getCharacterDetailUseCase(id)) {`  
            `is Resource.Success -> {`  
                `_state.update { it.copy(isLoading = false, character = result.data) }`  
            `}`  
            `is Resource.Error -> {`  
                `_state.update { it.copy(isLoading = false, error = result.message) }`  
            `}`  
            `else -> {}`  
        `}`  
    `}`  
`}`

`data class DetailUiState(`  
    `val isLoading: Boolean = false,`  
    `val character: DragonBallCharacter? = null,`  
    `val error: String? = null`  
`)`

**DetailScreen.kt**  
`@Composable`  
`fun DetailScreen(`  
    `viewModel: DetailViewModel = hiltViewModel(),`  
    `onBack: () -> Unit`  
`) {`  
    `val state by viewModel.state.collectAsStateWithLifecycle()`

    `Scaffold(`  
        `topBar = {`  
            `TopAppBar(`  
                `title = { Text(state.character?.name ?: "Detalle") },`  
                `navigationIcon = {`  
                    `IconButton(onClick = onBack) {`  
                        `Icon(Icons.Default.ArrowBack, contentDescription = "Back")`  
                    `}`  
                `}`  
            `)`  
        `}`  
    `) { padding ->`  
        `Box(modifier = Modifier.padding(padding).fillMaxSize()) {`  
            `if (state.isLoading) {`  
                `CircularProgressIndicator(modifier = Modifier.align(Alignment.Center))`  
            `}`  
              
            `state.character?.let { char ->`  
                `Column(`  
                    `modifier = Modifier.verticalScroll(rememberScrollState()).padding(16.dp),`  
                    `horizontalAlignment = Alignment.CenterHorizontally`  
                `) {`  
                    `AsyncImage(`  
                        `model = char.image,`  
                        `contentDescription = null,`  
                        `modifier = Modifier.height(300.dp).fillMaxWidth(),`  
                        `contentScale = C