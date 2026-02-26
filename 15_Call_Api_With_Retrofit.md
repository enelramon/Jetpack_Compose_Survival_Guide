
# 🐉 Habla con tu API usando Retrofit 

---

## 🎯 Objetivo

Crear una aplicación Android que muestre personajes de Dragon Ball, permita buscarlos mediante filtros y navegar a una pantalla de detalle.

---

## 🪜 Paso 1: Crear el Proyecto

1. Crear un proyecto Android con **Empty Compose Activity**.
1. Ejecutar la aplicación para verificar que funcione correctamente.

---

## 🌍 Paso 2: Permiso de Internet

Agregar el permiso de internet en el archivo `AndroidManifest.xml`.

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

##  Paso 3: Dependencias

Agregar estas dependencias en el archvo `build.gradle.kts (Module :app)`.

```kotlin
// Retrofit
implementation(libs.retrofit)
implementation(libs.retrofit2.kotlinx.serialization.converter)
implementation(libs.kotlinx.serialization.json)
implementation(libs.squareup.moshi.kotlin)
implementation(libs.converter.moshi)
```
Y agregar estas en el archivo `libs.versions.toml`.

```kotlin
// [versions]
kotlinSerializationJson = "1.10.0"
retrofit2KotlinxSerializationConverter = "1.0.0"
retrofit = "3.0.0"
moshiKotlin = "1.15.2"
converterMoshi = "3.0.0"

// [libraries]
retrofit = { module = "com.squareup.retrofit2:retrofit", version.ref = "retrofit" }
retrofit2-kotlinx-serialization-converter = { module = "com.jakewharton.retrofit:retrofit2-kotlinx-serialization-converter", version.ref = "retrofit2KotlinxSerializationConverter" }
kotlinx-serialization-json = { module = "org.jetbrains.kotlinx:kotlinx-serialization-json", version.ref = "kotlinSerializationJson" }
converter-moshi = { module = "com.squareup.retrofit2:converter-moshi", version.ref = "converterMoshi" }
squareup-moshi-kotlin = { module = "com.squareup.moshi:moshi-kotlin", version.ref = "moshiKotlin" }
```

---
## Dependency Injection
`di/AppModule.kt`
```kotlin
    @Singleton
    @Provides
    fun provideMoshi(): Moshi {
        return Moshi
            .Builder()
            .add(KotlinJsonAdapterFactory())
            .build()
    }

    @Provides
    @Singleton
    fun provideApi(moshi: Moshi): DragonBallApi {
        val json = Json { ignoreUnknownKeys = true }
        return Retrofit.Builder()
            .baseUrl("https://dragonball-api.com/api/")
            .addConverterFactory(MoshiConverterFactory.create(moshi))
            //.addConverterFactory(json.asConverterFactory("application/json".toMediaType()))
            .build()
            .create(DragonBallApi::class.java)
    }

    @Provides
    @Singleton
    fun provideRepository(api: DragonBallApi): CharacterRepository {
        return CharacterRepositoryImpl(api)
    }
```

---

## 🔄 Paso 4: Estados de Carga

Crear una clase para representar los estados de una operación.

`remote\Resource.kt`
```kotlin
sealed class Resource<T>(
    val data: T? = null,
    val message: String? = null
) {
    class Success<T>(data: T) : Resource<T>(data)
    class Error<T>(message: String) : Resource<T>(null, message)
    class Loading<T> : Resource<T>()
}
```

---
## 📦 Paso 5: Modelo de Datos

Crear la clase que representa un personaje.
`domain\model\Character.kt`
```kotlin
data class Character(
    val id: Int,
    val name: String,
    val ki: String,
    val race: String,
    val gender: String,
    val description: String,
    val image: String,
    val maxKi: String
)
```
---

## 🧾 Paso 6: DTOs

Clases que representan la respuesta de la API.

`remote\CharacterResponseDto.kt`
```kotlin
data class CharacterResponseDto(
    val items: List<CharacterDto>
)

data class CharacterDto(
    val id: Int,
    val name: String,
    val ki: String,
    val race: String,
    val gender: String,
    val description: String,
    val image: String,
    val maxKi: String,
) {
    fun toDomain() = Character(
        id, name, ki, race, gender, description, image, maxKi,
    )
}

---

## 📡 Paso 7: Interfaz de la API

Definir los endpoints para obtener los datos desde internet.
`remote\NombreDeTuApi.kt`
```kotlin
interface DragonBallApi {

    @GET("characters")
    suspend fun getCharacters(
        @Query("page") page: Int,
        @Query("limit") limit: Int,
        @Query("name") name: String?,
        @Query("gender") gender: String?,
        @Query("race") race: String?
    ): Response<CharacterResponseDto>

    @GET("characters/{id}")
    suspend fun getCharacterDetail(
        @Path("id") id: Int
    ): Response<CharacterDto>
}

```
---

```
## Paso 8: Repository (Domamin)
`domain\repository\CharacterRepository.kt`
```kotlin
interface CharacterRepository {
    suspend fun getCharacters(
        page: Int,
        limit: Int,
        name: String?,   
        gender: String?, 
        race: String?,    
    ): Resource<List<Character>>

    suspend fun getCharacterDetail(id: Int): Resource<Character>
}
```
--- 
## Paso 9: Usecases
`domain\usecase\GetCharactersUseCase.kt`
```kotlin
class GetCharactersUseCase @Inject constructor(
    private val repository: CharacterRepository
) {
    operator fun invoke(
        page: Int = 1,
        limit: Int = 10,
        name: String? = null,
        gender: String? = null,
        race: String? = null
    ) = repository.getCharacters(page, limit, name, gender, race)
}
```
## Paso 10: RemoteDataSource
`data\remote\remotedatasource\CharacterRemoteDataSource.kt`

```kotlin
class CharacterRemoteDataSource @Inject constructor(
    private val api: DragonBallApi
) {
    
    suspend fun getCharacters( 
        page: Int,
        limit: Int,
        name: String?,   
        gender: String?, 
        race: String?,  
    ): Result<CharacterResponseDto> {
        try {
            val response = api.getCharacters(page, limit, name, gender, race)
            if (!response.isSuccessful) {
                return Result.failure(Exception("Error de red ${response.code()}"))
            }
            return Result.success(response.body()!!)
        } catch (e: HttpException) {
            return Result.failure(Exception("Error de servidor", e))
        } catch (e: Exception) {
            return Result.failure(Exception("Error desconocido", e))
        }
    }

    suspend fun getCharacterDetail(id: Int): Result<CharacterDto> {
        try {
            val response = api.getCharacterDetail(id)
            if (!response.isSuccessful) {
                return Result.failure(Exception("Error de red ${response.code()}"))
            }
            return Result.success(response.body()!!)
        } catch (e: HttpException) {
            return Result.failure(Exception("Error de servidor", e))
        } catch (e: Exception) {
            return Result.failure(Exception("Error desconocido", e))
        }
    }
}
```

## Paso 10: Repository Implementation (IMPORTANTE: De aqui en adelante me cambioa Planet esto es un ejemplo)

`data\repository\PlanetRepositoryImpl.kt`

```kotlin

class PlanetRepositoryImpl @Inject constructor(
    private val remoteDataSource: PlanetRemoteDataSource
) : PlanetRepository {

    override fun getPlanets(
        page: Int,
        limit: Int,
        name: String?
    ): Flow<Resource<List<Planet>>> = flow {

        emit(Resource.Loading())

        val response = remoteDataSource.getPlanets(page, limit, name)
        response.onSuccess { planets ->
            emit(Resource.Success(planets.items.map { it.toDomain() }))
        }.onFailure {
            emit(Resource.Error(it.message ?: "Error desconocido"))
        }
    }

     override fun getPlanetDetail(id: Int): Flow<Resource<Planet>> = flow {
        emit(Resource.Loading())

        val response = remoteDataSource.getPlanetDetail(id)
        response.onSuccess { planets ->
            emit(Resource.Success(planets.toDomain() ))
        }.onFailure {
            emit(Resource.Error(it.message ?: "Error desconocido"))
        }
    }
}
```

---

## 🧠 Paso 11: Estado de la Pantalla de Lista

Clase que representa el estado visible de la pantalla principal.

```kotlin
data class ListUiState(
    val isLoading: Boolean = false,
    val characters: List<Character> = emptyList(),
    val error: String? = null,
    val filterName: String = "",
    val filterGender: String = "",
    val filterRace: String = ""
)
```

---

## 🎯 Paso 12: Eventos de la Pantalla

Eventos que se generan desde la interfaz.
```kotlin
sealed interface ListEvent {

    data class UpdateFilters(
        val name: String,
        val gender: String,
        val race: String
    ) : ListEvent

    data object Search : ListEvent
}
```

---

## 🧠 Paso 13: ViewModel de Lista

Clase encargada de manejar el estado y responder a los eventos.
```kotlin

@HiltViewModel
class ListViewModel @Inject constructor(
    private val getCharactersUseCase: GetCharactersUseCase
) : ViewModel() {

    private val _state = MutableStateFlow(ListUiState())
    val state = _state.asStateFlow()

    init {
        loadCharacters()
    }

    fun onEvent(event: ListEvent) {
        when (event) {
            is ListEvent.UpdateFilters -> _state.update {
                    it.copy(
                        filterName = event.name,
                        filterGender = event.gender,
                        filterRace = event.race
                    )
                }
            
            ListEvent.Search -> loadCharacters()
        }
    }

    private fun loadCharacters() {
        viewModelScope.launch {
            val current = _state.value

          getCharactersUseCase(
                name = current.filterName.takeIf { it.isNotBlank() },
                gender = current.filterGender.takeIf { it.isNotBlank() },
                race = current.filterRace.takeIf { it.isNotBlank() }
           ).collect { result ->

                when (result) {
                    is Resource.Loading -> _state.update { it.copy(isLoading = true) }
                
                    is Resource.Success ->
                        _state.update {
                            it.copy(
                                isLoading = false,
                                characters = result.data ?: emptyList()
                            )
                        }

                    is Resource.Error ->
                        _state.update {
                            it.copy(
                                isLoading = false,
                                error = result.message
                            )
                        }
                }
           }
        }
    }
}
```

---

## 🎨 Paso 14: Screen de Lista

Pantalla que muestra filtros y lista de personajes.
```kotlin

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ListScreen(
    viewModel: ListViewModel = hiltViewModel(),
    onCharacterClick: (Int) -> Unit
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
    ListBodyScreen(
        state = state,
        onEvent = viewModel::onEvent,
        onCharacterClick = onCharacterClick
    )
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ListBodyScreen(
    state: ListUiState,
    onEvent: (ListEvent) -> Unit,
    onCharacterClick: (Int) -> Unit,
) {
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(
                title = { Text("Personajes Dragon Ball") }
            )
        }
    ) { padding ->

        Column(
            modifier = Modifier
                .padding(padding)
                .fillMaxSize()
        ) {

            FilterSection(
                name = state.filterName,
                gender = state.filterGender,
                race = state.filterRace,
                onEvent = onEvent,
            )

            if (state.isLoading) {
                CircularProgressIndicator(
                    modifier = Modifier.align(Alignment.CenterHorizontally)
                )
            }

            LazyColumn(
                contentPadding = PaddingValues(16.dp)
            ) {
                items(state.characters) { character ->
                    CharacterItem(
                        character = character,
                        onClick = { onCharacterClick(character.id) }
                    )
                    Spacer(modifier = Modifier.height(4.dp))
                }
            }
        }
    }
}


@Composable
fun FilterSection(
    name: String,
    gender: String,
    race: String,
    onEvent: (ListEvent) -> Unit,
) {

    ElevatedCard(
        modifier = Modifier
            .padding(16.dp)
            .fillMaxWidth(),
    ) {
        Column(
            modifier = Modifier.padding(16.dp),
            verticalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            OutlinedTextField(
                value = name,
                onValueChange = { onEvent(ListEvent.UpdateFilters(it, gender, race)) },
                label = { Text("Nombre (ej. Goku)") },
                modifier = Modifier.fillMaxWidth()
            )

            Row(horizontalArrangement = Arrangement.spacedBy(8.dp)) {
                OutlinedTextField(
                    value = gender,
                    onValueChange = { onEvent(ListEvent.UpdateFilters(name, it, race)) },
                    label = { Text("Género") },
                    modifier = Modifier.weight(1f)
                )
                OutlinedTextField(
                    value = race,
                    onValueChange = { onEvent(ListEvent.UpdateFilters(name, gender, it)) },
                    label = { Text("Raza") },
                    modifier = Modifier.weight(1f)
                )
            }

            Button(
                onClick = { onEvent(ListEvent.Search) },
                modifier = Modifier.align(Alignment.End)
            ) {
                Text("Buscar")
            }
        }
    }
}

@Preview(showBackground = true)
@Composable
fun ListBodyScreenPreview() {
    val sampleCharacters = listOf(
        Character(
            id = 1,
            name = "Goku",
            ki = "60.000.000",
            race = "Saiyan",
            gender = "Male",
            description = "The main protagonist of the series.",
            image = "",
            maxKi = "90.000.000.000"
        ),
        Character(
            id = 2,
            name = "Vegeta",
            ki = "50.000.000",
            race = "Saiyan",
            gender = "Male",
            description = "The prince of all Saiyans.",
            image = "",
            maxKi = "80.000.000.000"
        )
    )
    val state = ListUiState(
        characters = sampleCharacters,
        filterName = "Goku"
    )

    DragonBallAppTheme {
        Surface {
            ListBodyScreen(
                state = state,
                onEvent = {},
                onCharacterClick = {}
            )
        }
    }
}
```

---

## 🧩 Paso 15: Item de Personaje
```kotlin
@Composable
fun CharacterItem(
    character: Character,
    onClick: () -> Unit
) {
    ElevatedCard(
        modifier = Modifier
            .fillMaxWidth()
            .clickable { onClick() }
    ) {
        Row(
            modifier = Modifier.padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {

            AsyncImage(
                model = character.image,
                contentDescription = character.name,
                modifier = Modifier.size(64.dp)
            )

            Spacer(modifier = Modifier.width(16.dp))

            Column {
                Text(character.name)
                Text("${character.race} • ${character.gender}")
            }
        }
    }
}

```
---

## 🔍 Paso 16: ViewModel de Detalle
```kotlin
@HiltViewModel
class DetailViewModel @Inject constructor(
    private val getCharacterDetailUseCase: GetCharacterDetailUseCase,
    savedStateHandle: SavedStateHandle
) : ViewModel() {

    private val _state = MutableStateFlow(DetailUiState())
    val state = _state.asStateFlow()

    init {
        val args = savedStateHandle.toRoute<Screen.Detail>()
        loadCharacter(args.id)
    }

    private fun loadCharacter(id: Int) {
        viewModelScope.launch {
            _state.update { it.copy(isLoading = true) }

            when (val result = getCharacterDetailUseCase(id)) {
                is Resource.Success ->
                    _state.update {
                        it.copy(
                            isLoading = false,
                            character = result.data
                        )
                    }

                is Resource.Error ->
                    _state.update {
                        it.copy(
                            isLoading = false,
                            error = result.message
                        )
                    }

                is Resource.Loading -> Unit
            }
        }
    }
}
```

---

## 📄 Paso 17: Screen de Detalle
```kotlin
@Composable
fun DetailScreen(
    state: DetailUiState,
    onBack: () -> Unit
) {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Detalle del Personaje") },
                navigationIcon = {
                    IconButton(onClick = onBack) {
                        Icon(Icons.Default.ArrowBack, null)
                    }
                }
            )
        }
    ) { padding ->

        state.character?.let { character ->
            Column(
                modifier = Modifier
                    .padding(padding)
                    .padding(16.dp)
            ) {

                AsyncImage(
                    model = character.image,
                    contentDescription = character.name,
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(240.dp)
                )

                Text(character.name)
                Text("Raza: ${character.race}")
                Text("Género: ${character.gender}")
                Text("Ki: ${character.ki}")
                Text("Máx Ki: ${character.maxKi}")
                Text(character.description)
            }
        }
    }
}
// Agrega el preview
```
