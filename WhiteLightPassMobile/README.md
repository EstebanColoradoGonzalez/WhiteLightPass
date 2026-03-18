# WhiteLightPass — Cliente Móvil (Android)

[![Kotlin](https://img.shields.io/badge/Kotlin-Android-7F52FF?logo=kotlin)](https://kotlinlang.org/)
[![Android](https://img.shields.io/badge/Android-API_26+-3DDC84?logo=android)](https://developer.android.com/)

## Propósito

Cliente móvil del ecosistema **WhiteLightPass**. Construido de forma nativa con **Kotlin y el SDK de Android** para proporcionar acceso táctil y resiliente a la bóveda cifrada desde dispositivos Android.

Esta es la **segunda plataforma** en orden de construcción. Al igual que el cliente de escritorio, toda la lógica criptográfica, el motor CRDT y la serialización del Contenedor Criptográfico se ejecutan **localmente** en el dispositivo; no existe backend ni servidor intermedio. La sincronización se delega al Google Drive personal del usuario.

> **Estado actual:** Fase de diseño y definición del dominio. La implementación aún no ha iniciado.

## Requisitos Previos

- **IDE:** [Android Studio](https://developer.android.com/studio) (versión actual o superior).
- **JDK:** Versión 17 (configurada en los Gradle Settings del IDE).
- **SDK de Android:**
    - `compileSdk`: 35
    - `minSdk`: 26 (Android 8.0 Oreo)
    - `targetSdk`: 35
- **Dispositivo/Emulador:** Con al menos Android 8.0 y depuración USB habilitada.

## Configuración Local

1. **Clonar el repositorio:**

    ```bash
    git clone https://github.com/[usuario]/WhiteLightPass.git
    cd WhiteLightPass/WhiteLightPassMobile
    ```

2. **Sincronizar Gradle:**
    - Abrir el proyecto en Android Studio. El IDE ejecutará automáticamente un Gradle Sync.
    - Si falla: `File > Sync Project with Gradle Files`.

3. **Compilar y ejecutar:**
    - Seleccionar emulador o dispositivo físico en la barra de ejecución.
    - Presionar `Shift + F10` para instalar el APK de depuración.

## Estructura del Proyecto

```
WhiteLightPassMobile/
├── app/
│   ├── src/main/java/      # Código fuente Kotlin
│   ├── src/main/res/       # Recursos (layouts, strings, drawables)
│   └── build.gradle.kts    # Configuración del módulo app
├── build.gradle.kts        # Configuración raíz
├── settings.gradle.kts
└── gradle/                 # Gradle Wrapper
```

> La estructura se expandirá cuando inicie la fase de arquitectura e implementación.

## Comandos Frecuentes

| Propósito | Comando |
|----------|----------|
| Limpiar compilaciones | `gradlew.bat clean` |
| Ensamblar APK debug | `gradlew.bat assembleDebug` |
| Ejecutar pruebas unitarias | `gradlew.bat testDebugUnitTest` |
| Análisis estático (Lint) | `gradlew.bat lintDebug` |

## Documentación del Dominio

Este cliente implementa las reglas definidas en los documentos de dominio del proyecto principal:

- [Definición del Sistema](../docs/domain/definition/system_definition_document.md)
- [Visión del Producto](../docs/domain/definition/product_vision_document.md)
- [Requerimientos](../docs/domain/definition/requirements_document.md)