# WhiteLightPass — Cliente de Escritorio (WinUI 3)

[![.NET 8.0](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![WinUI 3](https://img.shields.io/badge/WinUI-3.0-0078D7?logo=windows)](https://learn.microsoft.com/windows/apps/winui/winui3/)

## Propósito

Cliente de escritorio del ecosistema **WhiteLightPass**. Construido con **.NET 8 y WinUI 3 (Windows App SDK)** para proporcionar una experiencia nativa de alto rendimiento en Windows.

Esta es la **plataforma primaria** del proyecto: la primera en construirse y la de uso más intensivo. Toda la lógica criptográfica, el motor CRDT y la serialización del Contenedor Criptográfico se ejecutan **localmente** en el dispositivo; no existe backend ni servidor intermedio. La sincronización se delega al Google Drive personal del usuario.

> **Estado actual:** Fase de diseño y definición del dominio. La implementación aún no ha iniciado.

## Requisitos Previos

- **Sistema Operativo:** Windows 10 (build 1903 o superior) o Windows 11.
- **IDE:** [Visual Studio 2022](https://visualstudio.microsoft.com/) (recomendado) o JetBrains Rider.
- **Workloads de Visual Studio:**
  - Desarrollo de escritorio de .NET (`.NET desktop development`).
  - Windows App SDK C# Templates.
- **SDK:** [.NET 8.0 SDK](https://dotnet.microsoft.com/download).
- **Target Framework:** `net8.0-windows10.0.19041.0`

## Configuración Local

1. **Clonar el repositorio:**

    ```bash
    git clone https://github.com/[usuario]/WhiteLightPass.git
    cd WhiteLightPass/WhiteLightPassDesktop
    ```

2. **Restaurar dependencias:**

    ```bash
    dotnet restore
    ```

3. **Compilar y ejecutar:**
    - En Visual Studio: seleccionar el proyecto `WhiteLightPassDesktop` y presionar `F5`.
    - Compilar bajo plataforma `x64` (WinUI 3 no soporta `Any CPU` por defecto).

## Estructura del Proyecto

```
WhiteLightPassDesktop/
├── Assets/                 # Recursos estáticos (iconos, imágenes)
├── App.xaml                # Punto de entrada y recursos globales
├── MainWindow.xaml         # Ventana principal
├── Properties/             # Configuración del proyecto
└── WhiteLightPassDesktop.csproj
```

> La estructura se expandirá cuando inicie la fase de arquitectura e implementación.

## Comandos Frecuentes

| Propósito | Comando |
|----------|----------|
| Limpiar solución | `dotnet clean` |
| Compilar | `dotnet build` |
| Ejecutar pruebas | `dotnet test` |

## Documentación del Dominio

Este cliente implementa las reglas definidas en los documentos de dominio del proyecto principal:

- [Definición del Sistema](../docs/domain/definition/system_definition_document.md)
- [Visión del Producto](../docs/domain/definition/product_vision_document.md)
- [Requerimientos](../docs/domain/definition/requirements_document.md)
