# Aprendiendo Kotlin - Tarjeta de Felicitación

Este proyecto es una aplicación Android desarrollada como parte de mi proceso de aprendizaje de Kotlin y desarrollo Android. Consiste en una tarjeta de felicitación digital simple que muestra un mensaje personalizable.

## Lo que he aprendido

### Kotlin
- **Sintaxis básica de Kotlin**: Kotlin utiliza una sintaxis más concisa que Java, eliminando elementos como los punto y coma al final de las líneas y simplificando la declaración de clases y funciones. Por ejemplo, en mi código uso la palabra clave `class` para definir `MainActivity` y la sintaxis de funciones con `fun`.

- **Funciones en Kotlin con parámetros por defecto**: En Kotlin puedo definir valores predeterminados para los parámetros, como `modifier = Modifier` en mi función `GreetingText()`. Esto me permite llamar a la función sin tener que especificar todos los parámetros, simplificando el código y evitando sobrecargas.

- **Parámetros nombrados**: Kotlin permite especificar el nombre del parámetro al llamar a una función (como `from = "De David"`), lo que hace el código más legible y evita confusiones en el orden de los parámetros, especialmente útil cuando hay varios parámetros del mismo tipo.

### Jetpack Compose
- **Componentes básicos**:
  - `Surface`: Es un contenedor que implementa la semántica de diseño Material. En mi app, lo uso como contenedor principal que abarca toda la pantalla. Surface maneja automáticamente propiedades como la elevación, el color de fondo y efectos de sombra, aplicando el tema Material de forma coherente.

  - `Text`: Este componente muestra texto en pantalla. A diferencia de los TextView en Android tradicional, Text de Compose es más flexible y se puede personalizar directamente en el código Kotlin. En mi aplicación, utilizo propiedades como `fontSize` para controlar el tamaño, `lineHeight` para el espaciado entre líneas y `textAlign` para la alineación, todo sin necesidad de definir estilos XML.

  - `Column`: Es un layout que organiza sus hijos en una columna vertical. Funciona como un LinearLayout vertical en Android tradicional, pero con una API más sencilla. En mi app, uso Column con `verticalArrangement = Arrangement.Center` para centrar verticalmente los textos, lo que elimina la necesidad de especificar pesos o gravity como en XML.

  - `Button`: Crea un botón siguiendo el diseño Material. En mi función `TestButton()`, implemento un botón básico con un callback `onClick` que actualmente solo imprime un mensaje en la consola. Este patrón de manejo de eventos es más directo que los OnClickListener tradicionales.

- **Composables**: Son funciones anotadas con `@Composable` que definen la interfaz de usuario. Funcionan como bloques de construcción reutilizables. A diferencia de las vistas XML tradicionales:
  - No mantienen estado por sí mismas
  - Se ejecutan en el hilo principal
  - Pueden llamarse directamente en código Kotlin
  - Se recomponen automáticamente cuando cambian sus parámetros
  
  En mi app, `GreetingText()` es un composable que encapsula la lógica de presentación del mensaje de felicitación.

- **Modificadores (Modifier)**: Son elementos clave en Compose que permiten personalizar la apariencia y el comportamiento de los componentes. Funcionan como una cadena de transformaciones aplicadas en orden:
  - `fillMaxSize()`: Hace que un componente ocupe todo el espacio disponible de su contenedor, similar a `match_parent` en XML.
  - `padding()`: Añade espacio alrededor de un componente, especificado en dp (density-independent pixels).
  - `align()`: Controla la alineación del componente dentro de su contenedor. En mi código, uso `Alignment.End` para alinear el texto del remitente a la derecha.
  
  La potencia de los modificadores está en su capacidad de combinarse mediante encadenamiento (como `Modifier.padding(16.dp).align(alignment = Alignment.End)`).

- **Previsualización**: La anotación `@Preview` permite ver cómo se verán los componentes directamente en Android Studio sin necesidad de ejecutar la app. Esto acelera significativamente el desarrollo al proporcionar retroalimentación visual inmediata. Mi función `GreetingPreview()` muestra cómo se verá la tarjeta de felicitación completa.

- **Parámetros en Compose**: Los componentes composable reciben datos a través de parámetros regulares de Kotlin. Cuando estos parámetros cambian, Compose recompone automáticamente la UI para reflejar el nuevo estado. En mi app, `message` y `from` son parámetros que determinan el contenido de la tarjeta.

### Material Design 3
- **Implementación del tema básico**: `CalculatorTheme` (definido en Theme.kt) encapsula todos los colores, tipografías y formas de Material Design 3. Funciona aplicando estas propiedades de diseño consistentemente en toda la aplicación. Al envolver mis composables con este tema, garantizo que la app siga las directrices de diseño de Google.

- **Superficies y colores**: Material Design 3 utiliza un sistema de capas donde los elementos se organizan en diferentes niveles de elevación. `Surface` implementa este concepto, y al establecer `color = MaterialTheme.colorScheme.background`, estoy accediendo al esquema de colores dinámico de Material 3 que se adapta automáticamente a los temas claro y oscuro.

- **Tipografía**: Material Design 3 define una escala tipográfica coherente. En mi código, uso `fontSize` y `lineHeight` para aplicar tamaños tipográficos apropiados para el mensaje principal (100.sp) y el remitente (36.sp). Los valores sp (scale-independent pixels) aseguran que el texto se escale según las preferencias del usuario.

- **Alineación de texto**: `textAlign = TextAlign.Center` controla cómo se alinea el texto dentro de su contenedor. Esto es esencial para una presentación visual equilibrada, especialmente en mensajes grandes como el de felicitación.

### Estructura básica de una app Android
- **Actividad principal**: `MainActivity` es el punto de entrada de mi aplicación. Al extender de `ComponentActivity` (en lugar de AppCompatActivity), estoy utilizando la clase base recomendada para apps basadas en Compose, que proporciona la integración necesaria con el sistema Android.

- **Ciclo de vida básico**: El método `onCreate()` es llamado por el sistema cuando se crea la actividad. Es parte del ciclo de vida de Android y representa el momento en que la actividad comienza a existir. Aquí es donde configuro la interfaz de usuario con `setContent`, que es específico para aplicaciones Compose.

- **Configuración de la UI**: `setContent` es una extensión de función que establece el contenido composable de la actividad. Reemplaza el tradicional `setContentView` que se usaba con layouts XML. Dentro de este bloque, defino toda la jerarquía de composables que forman mi interfaz de usuario.

## Implementación del Proyecto

Mi código principal en MainActivity.kt implementa:

```kotlin
// Actividad principal que sirve como punto de entrada de la aplicación
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            // Aplico el tema Material 3 a toda la UI
            CalculatorTheme {
               // Surface principal que ocupa toda la pantalla con el color de fondo del tema
               Surface(
                   modifier = Modifier.fillMaxSize(),
                   color = MaterialTheme.colorScheme.background
               ){
                   // Componente personalizado que muestra el mensaje de felicitación
                   GreetingText(
                       from = "De David",                     // Remitente del mensaje
                       message = "Feliz cumpleaños Abuela",   // Texto principal
                       modifier = Modifier.padding(8.dp)      // Espaciado alrededor del contenido
                   )
               }
            }
        }
    }
}

// Componente composable personalizado que muestra un mensaje de felicitación con remitente
// Recibe el mensaje, el remitente y un modificador opcional
@Composable
fun GreetingText(message: String, from: String, modifier: Modifier = Modifier) {
    // Column organiza los elementos verticalmente con el contenido centrado
    Column(
        verticalArrangement = Arrangement.Center,
        modifier = modifier
    ) {
        // Texto principal (felicitación) con tamaño grande y centrado
        Text(
            text = message,
            fontSize = 100.sp,              // Tamaño grande para destacar
            lineHeight = 116.sp,            // Espaciado entre líneas
            textAlign = TextAlign.Center    // Centrado horizontal del texto
        )
        // Texto del remitente en tamaño más pequeño y alineado a la derecha
        Text(
            text = from,
            fontSize = 36.sp,               // Tamaño secundario más pequeño
            modifier = Modifier
                .padding(16.dp)             // Espacio alrededor del texto
                .align(alignment = Alignment.End)  // Alineado a la derecha
        )
    }
}

// Función de previsualización que muestra cómo se verá el componente en Android Studio
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    // Aplico el mismo tema para mantener la coherencia visual
    CalculatorTheme {
        GreetingText("Feliz cumpleaños Abuela", "De David")
    }
}
```

## Estructura del Proyecto

La estructura de mi proyecto sigue el patrón estándar de Android:

```
app/
├── src/
│   ├── main/
│   │   ├── java/                          
│   │   │   └── com/example/calculator/    # Paquete principal
│   │   │       ├── MainActivity.kt        # Contiene toda la lógica de la UI
│   │   │       └── ui/theme/              # Configuración de Material Design 3
│   │   │           ├── Color.kt           # Define los colores de la app
│   │   │           ├── Theme.kt           # Configura el tema general
│   │   │           └── Type.kt            # Define la tipografía
│   │   ├── res/                          
│   │   │   ├── values/                    # Recursos de la app
│   │   │   │   ├── strings.xml            # Textos localizables
│   │   │   │   └── colors.xml             # Definición de colores
│   │   │   └── mipmap/                    # Iconos de la aplicación
│   │   └── AndroidManifest.xml            # Configuración general de la app
```

## Próximos Pasos
Basado en lo que he aprendido, podría mejorar esta app:

- **Imágenes y fondos personalizados**: Incorporar recursos de imagen utilizando `Image()` composable y el modificador `background()` para fondos.

- **Animaciones simples**: Implementar transiciones y animaciones con `animateColorAsState()` o `AnimatedVisibility()` para dar vida al texto.

- **Botón para compartir**: Extender mi `TestButton()` para usar `Intent.ACTION_SEND` y compartir la felicitación a través de otras apps.

- **Personalización del mensaje**: Añadir `TextField()` para permitir al usuario escribir su propio mensaje y usar `remember { mutableStateOf("") }` para gestionar el estado.

- **Adaptación a diferentes pantallas**: Usar `BoxWithConstraints` o modificadores condicionales basados en el tamaño de ventana.

## Recursos Utilizados
- [Documentación oficial de Kotlin](https://kotlinlang.org/docs/home.html) - Fundamental para entender la sintaxis y características del lenguaje
- [Documentación de Android Developers](https://developer.android.com/) - Guía completa sobre el desarrollo Android
- [Guía de Jetpack Compose](https://developer.android.com/jetpack/compose) - Tutoriales y referencia para la construcción de UI con Compose
- [Material Design](https://m3.material.io/) - Directrices de diseño seguidas en la aplicación

