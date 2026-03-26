# Guía de Estudio - Parcial Aplicaciones para Dispositivos Móviles

## Objetivo
Este resumen está pensado para estudiar el parcial de la asignatura y, además, poder defender el proyecto `BBTraveling` en una posible pregunta oral o teórico-práctica.

---

# Parte Teórica

## ¿Qué es Android Fundamentals?
Android es un sistema operativo para dispositivos móviles basado en Linux. Una app Android se organiza en componentes, recursos y archivos de configuración, y su ejecución está controlada por el sistema mediante un ciclo de vida.

## ¿Qué es una Activity?
Una `Activity` representa una pantalla o punto de interacción con el usuario. Es uno de los componentes principales de Android.

## ¿Qué es el Lifecycle?
El `lifecycle` es el ciclo de vida de una `Activity` o componente Android. Incluye métodos como `onCreate`, `onStart`, `onResume`, `onPause`, `onStop` y `onDestroy`, y permite saber en qué estado está la pantalla.

## ¿Qué es un Intent?
Un `Intent` es un mecanismo de Android para abrir otra `Activity`, lanzar una acción o comunicar componentes entre sí.

## ¿Qué es Gradle?
Gradle es el sistema de build del proyecto. Se encarga de compilar la app, gestionar dependencias, configurar versiones de SDK y organizar módulos como `app`.

## ¿Qué es la UI en Android?
La interfaz de usuario es la parte visual de la app. En Android puede construirse con Views clásicas o con Jetpack Compose.

## ¿Qué es un Composable?
Un `Composable` es una función de Jetpack Compose que dibuja parte de la interfaz.

## ¿Qué son los Layouts en Compose?
Los layouts organizan visualmente los elementos en pantalla. Los más comunes son:
- `Column`: coloca elementos en vertical.
- `Row`: coloca elementos en horizontal.
- `Box`: permite superponer o alinear elementos.
- `LazyColumn`: muestra listas verticales scrolleables.
- `Scaffold`: estructura base de una pantalla.

## ¿Qué es el estado en Compose?
El estado son los datos que determinan lo que se ve en pantalla. Cuando el estado cambia, la interfaz puede recomponerse para reflejar esos cambios.

## ¿Qué es la persistencia del estado?
Es mantener información importante aunque la pantalla se recree o cambie la configuración. El estado importante no debe quedarse solo en la UI.

## ¿Qué son los cambios de configuración?
Son cambios como la rotación de pantalla o cambio de idioma. Estos pueden recrear la `Activity`, por lo que el estado debe protegerse usando `ViewModel` u otros mecanismos.

## ¿Qué es el patrón Observer?
Es un patrón donde un objeto observa cambios en otro y se actualiza automáticamente. En Android aparece en `LiveData`, `StateFlow` y `State` de Compose.

## ¿Qué es un Singleton?
Es un patrón de diseño que garantiza que una clase tenga una única instancia compartida en toda la app. Se usa en recursos comunes como repositorios, gestores o bases de datos.

## ¿Qué es Dependency Injection?
La inyección de dependencias consiste en proporcionar dependencias desde fuera de una clase en lugar de crearlas dentro. Mejora mantenimiento, reutilización y testing.

## ¿Qué es un Repository?
Es una capa que centraliza el acceso a datos y separa la lógica de datos de la UI.

## ¿Qué es MVVM?
`MVVM` significa `Model - View - ViewModel`. La UI muestra datos, el `ViewModel` prepara el estado y coordina acciones, y la capa de datos se gestiona a través del `Repository`.

## ¿Qué es Testing?
Las pruebas unitarias validan la lógica de negocio de forma aislada, sin depender de la interfaz ni del dispositivo.

---

# Parte Práctica / Teórico-Práctica

## ¿Qué significa `val` y `var` en Kotlin?
`val` define una variable inmutable.  
`var` define una variable mutable.

## ¿Qué son las funciones?
Una función encapsula una tarea concreta. Puede recibir parámetros y devolver un resultado.

## ¿Qué son los parámetros?
Son los datos de entrada de una función. Permiten reutilizar código con diferentes valores.

## ¿Qué es un menú en UI y navegación?
Un menú es un conjunto de opciones que permite ejecutar acciones o navegar entre pantallas. Puede implementarse con una `NavigationBar`, un `DropdownMenu` o botones.

## ¿Qué es Scaffold?
`Scaffold` es la estructura base de una pantalla Compose. Organiza zonas como `topBar`, `bottomBar`, `floatingActionButton` y contenido principal.

## ¿Qué es LazyColumn?
`LazyColumn` es una lista vertical scrolleable que solo compone los elementos visibles, por eso es eficiente.

## ¿Qué diferencia hay entre Column y Row?
`Column` coloca elementos en vertical.  
`Row` coloca elementos en horizontal.

## ¿Qué es Composable Navigation?
Es el sistema de navegación de Jetpack Compose. Usa `NavController`, `NavHost` y rutas para cambiar entre pantallas.

## ¿Cuál es la estructura MVVM de Sprint 02?
La estructura esperada es:

`UI -> ViewModel -> Repository -> Data Source`

## ¿Qué significa separación por capas?
Significa que cada capa tiene una responsabilidad concreta. La UI no debe contener lógica de negocio ni acceder directamente a la fuente de datos.

## ¿Para qué sirve ViewModel?
El `ViewModel` mantiene el estado de la pantalla y coordina acciones de la UI. Ayuda a evitar pérdida de datos en rotaciones o recreaciones.

## ¿Qué hace `remember`?
`remember` guarda estado local mientras el composable siga en composición.

## ¿Qué hace `mutableState`?
`mutableState` crea un estado observable. Cuando cambia, Compose recompone la UI.

## ¿Qué es la internacionalización?
Es preparar la app para varios idiomas y configuraciones regionales separando los textos en recursos.

## ¿Cómo se implementa el multiidioma?
Con distintos archivos `strings.xml`, por ejemplo:
- `values/strings.xml`
- `values-es/strings.xml`
- `values-ca/strings.xml`

## ¿Qué es Repository Pattern?
Es una forma de implementar la capa de datos con una interfaz o clase que centraliza el acceso a la información y oculta su origen.

## ¿Cómo es el flujo completo UI -> ViewModel -> Repository?
La UI recoge una acción del usuario, el `ViewModel` valida y coordina, y el `Repository` consulta o modifica los datos.

## ¿Qué es Hilt?
`Hilt` es una librería de inyección de dependencias para Android. Automatiza la creación e inyección de dependencias usando anotaciones como `@Inject`, `@Singleton` y `@AndroidEntryPoint`.

## ¿Qué significa que Sprint 3 esté orientado a Hilt?
A nivel teórico, Sprint 3 parece introducir DI con `Hilt`, aunque en nuestro proyecto real de Sprint 1 y 2 no se usó.

## ¿Qué es debugging con Logs y Logcat?
Es el proceso de detectar errores o comportamientos inesperados usando mensajes de log visibles en `Logcat`.

## ¿Cuáles son buenas prácticas con logs?
- `INFO` para operaciones correctas.
- `WARNING` para validaciones o problemas esperados.
- `ERROR` para fallos reales o inesperados.

---

# Parte del Proyecto

## ¿Cómo se llama nuestro proyecto?
Nuestro proyecto se llama `BBTraveling`.

## ¿Qué arquitectura usa?
Usa arquitectura `MVVM` con este flujo:

`UI -> ViewModel -> Repository -> Data Source`

## ¿Qué hicimos en Sprint 01?
En Sprint 01 desarrollamos la base visual:
- navegación
- pantallas mock
- estructura inicial del proyecto
- dominio
- datos hardcoded
- documentación inicial

## ¿Qué hicimos en Sprint 02?
En Sprint 02 añadimos la parte funcional:
- CRUD en memoria de viajes
- CRUD en memoria de actividades
- validaciones
- persistencia de ajustes con `SharedPreferences`
- multiidioma
- logs
- tests unitarios
- documentación final

## ¿Cómo está implementado el proyecto?
Usamos:
- Kotlin
- Jetpack Compose
- Compose Navigation
- `StateFlow` y `collectAsState`
- `SharedPreferences`
- datos en memoria
- arquitectura `MVVM`

## ¿Usamos Hilt en nuestro proyecto?
No. En nuestro proyecto no usamos `Hilt`. La inyección de dependencias está hecha manualmente con `AppContainer` y `ViewModelFactory`.

## ¿Qué hace el Repository en nuestro proyecto?
`TripRepositoryImpl` centraliza la lógica de viajes y actividades.

## ¿Cómo persistimos los ajustes?
Con `SharedPreferencesSettingsRepository`, que guarda:
- usuario
- fecha de nacimiento
- idioma
- tema
- términos aceptados

## ¿Cómo gestionamos el estado?
El estado principal se maneja con `StateFlow` desde repositorio y `ViewModel`.  
La UI local usa `remember` y `rememberSaveable`.

## ¿Qué testing tiene el proyecto?
Tenemos pruebas unitarias de dominio y repositorio para:
- CRUD
- validaciones
- presupuesto
- reprogramación de actividades

## ¿Qué logs usamos?
Los principales tags son:
- `TripsViewModel`
- `SettingsViewModel`

## ¿Cómo es la navegación del proyecto?
Tenemos un grafo principal con pantallas como:
- `Splash`
- `Main`
- `TripDetail`
- `Gallery`
- pantallas de ajustes

## ¿Cómo está hecho el multiidioma?
Está implementado en:
- `en`
- `es`
- `ca`

---

# Preguntas Probables De Examen

## ¿Qué es MVVM y cómo lo aplicasteis?
`MVVM` es un patrón de arquitectura que separa UI, lógica de presentación y datos. En nuestro proyecto la UI llama al `ViewModel`, el `ViewModel` coordina acciones y el `Repository` gestiona los datos.

## ¿Cómo fluye una acción como crear un viaje?
La UI recoge los datos, el `ViewModel` valida y coordina, el `Repository` aplica la lógica y actualiza el `DataSource`, y la UI se actualiza al cambiar el estado.

## ¿Dónde guardáis los ajustes del usuario?
En `SharedPreferences`.

## ¿Usasteis Hilt?
No. En nuestro proyecto la DI es manual con `AppContainer` y `ViewModelFactory`.

## ¿Qué diferencia hay entre `remember` y `ViewModel`?
`remember` guarda estado local temporal del composable.  
`ViewModel` mantiene estado más importante de la pantalla y sobrevive mejor a cambios de configuración.

## ¿Qué es un Singleton?
Es una clase con una única instancia compartida en toda la app.

## ¿Qué es Composable Navigation?
Es la navegación entre pantallas en Compose usando `NavController`, `NavHost` y rutas.

## ¿Qué es un Repository?
Es la capa que centraliza el acceso a datos y separa la lógica de datos de la UI.

## ¿Qué hace una Activity?
Representa una pantalla y participa en el ciclo de vida de Android.

## ¿Qué hace un Intent?
Permite abrir otra pantalla o comunicar componentes.

---

# Respuesta Corta Para Defender El Proyecto

`BBTraveling` es una app Android de planificación de viajes. En Sprint 01 desarrollamos la base visual con Kotlin y Jetpack Compose: pantallas, navegación y datos mock. En Sprint 02 implementamos la lógica funcional con arquitectura `MVVM`, CRUD en memoria de viajes y actividades, validaciones, persistencia con `SharedPreferences`, multiidioma, logs y tests. La arquitectura sigue el flujo `UI -> ViewModel -> Repository -> Data Source`, y en nuestro caso la inyección de dependencias está hecha manualmente, no con Hilt.
