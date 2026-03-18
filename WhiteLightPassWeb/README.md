# WhiteLightPass — Cliente Web (Angular)

[![Angular 20](https://img.shields.io/badge/Angular-20-DD0031?logo=angular)](https://angular.dev/)
[![TypeScript 5.9](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript)](https://www.typescriptlang.org/)

## Propósito

Cliente web del ecosistema **WhiteLightPass**. Construido con **Angular 20 y TypeScript 5.9** para proporcionar acceso universal a la bóveda cifrada desde cualquier navegador moderno (Chrome, Edge, Firefox).

Esta es la **tercera plataforma** en orden de construcción, orientada a situaciones de emergencia o acceso desde equipos ajenos donde no se dispone de la aplicación de escritorio ni del teléfono. Toda la lógica criptográfica se ejecuta **íntegramente en el navegador** mediante WebAssembly (libsodium.js); no existe backend ni servidor intermedio. La sincronización se delega al Google Drive personal del usuario vía OAuth.

> **Estado actual:** Fase de diseño y definición del dominio. La implementación aún no ha iniciado.
>
> **Nota de seguridad:** El entorno web ofrece garantías de aislamiento de memoria inferiores a los clientes nativos (no existe `mlock` ni zeroing garantizado en JavaScript). Esta limitación se documenta explícitamente al usuario.

## Requisitos Previos

- **Runtime:** [Node.js](https://nodejs.org/) (versión LTS actual).
- **Gestor de paquetes:** npm (incluido con Node.js).
- **CLI:** Angular CLI (`npm install -g @angular/cli`).
- **IDE:** Visual Studio Code (recomendado) con la extensión Angular Language Service.

## Configuración Local

1. **Clonar el repositorio:**

    ```bash
    git clone https://github.com/[usuario]/WhiteLightPass.git
    cd WhiteLightPass/WhiteLightPassWeb
    ```

2. **Instalar dependencias:**

    ```bash
    npm install
    ```

3. **Iniciar servidor de desarrollo:**

    ```bash
    npm start
    ```

    Accesible en `http://localhost:4200`.

## Estructura del Proyecto

```
WhiteLightPassWeb/
├── src/
│   ├── app/                # Código fuente Angular
│   └── public/             # Recursos estáticos
├── angular.json            # Configuración del workspace Angular
├── package.json            # Dependencias y scripts
└── tsconfig.json           # Configuración TypeScript
```

> La estructura se expandirá cuando inicie la fase de arquitectura e implementación.

## Comandos Frecuentes

| Propósito | Comando |
|----------|----------|
| Servidor de desarrollo | `npm start` |
| Compilar para producción | `npm run build` |
| Ejecutar pruebas unitarias | `npm test` |
| Análisis estático (Lint) | `npm run lint` |

## Navegadores Soportados

- Chrome (2 últimas versiones)
- Edge (2 últimas versiones)
- Firefox (2 últimas versiones)

Safari no es requisito.

## Documentación del Dominio

Este cliente implementa las reglas definidas en los documentos de dominio del proyecto principal:

- [Definición del Sistema](../docs/domain/definition/system_definition_document.md)
- [Visión del Producto](../docs/domain/definition/product_vision_document.md)
- [Requerimientos](../docs/domain/definition/requirements_document.md)
