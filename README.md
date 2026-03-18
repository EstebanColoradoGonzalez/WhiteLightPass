# WhiteLightPass

Un gestor de contraseñas multiplataforma con cifrado local soberano, resolución de conflictos basada en CRDT y sincronización automática a través del Google Drive personal del usuario. Sin servidores propietarios, sin custodia de llaves por terceros.

## Descripción General

WhiteLightPass es un gestor de credenciales soberano diseñado para usuarios que necesitan que su bóveda cifrada viaje de forma transparente entre múltiples dispositivos — sin delegar confianza a ningún proveedor externo. El archivo cifrado vive en la cuenta de Google Drive del propio usuario, y cada dispositivo resuelve las ediciones concurrentes localmente mediante un motor CRDT determinista.

### Principios Clave

- **Soberanía Total** — El usuario es el único custodio criptográfico. Ningún servidor, proveedor o intermediario puede leer, inferir o reconstruir los secretos almacenados.
- **Resolución de Conflictos CRDT** — Las ediciones concurrentes offline desde distintos dispositivos se fusionan automáticamente sin pérdida de datos y sin "Última Escritura Gana".
- **Cero Infraestructura Propietaria** — Todo el almacenamiento remoto se delega a la nube personal del usuario (Google Drive). Sin suscripciones, sin dependencia de proveedor.
- **Mitigación de Análisis de Tráfico** — Padding dinámico y flujo de cobertura ocultan el tamaño de la bóveda y la frecuencia de sincronización ante observadores en la red.
- **Adaptabilidad Criptográfica** — El formato del contenedor soporta migración de algoritmos (parámetros KDF, suites de cifrado) sin romper compatibilidad.

## Plataformas

| Plataforma | Tecnología | Directorio | Prioridad |
|------------|-----------|-----------|-----------|
| Escritorio | .NET / WinUI | `WhiteLightPassDesktop/` | Primaria |
| Móvil | Kotlin / Android | `WhiteLightPassMobile/` | Secundaria |
| Web | Angular / TypeScript | `WhiteLightPassWeb/` | Terciaria |

Los tres clientes operan sobre el **mismo contenedor cifrado** almacenado en Google Drive. El formato binario es independiente de la plataforma.

## Stack Criptográfico

| Componente | Primitiva |
|-----------|-----------|
| KDF | Argon2id (≥ 64 MiB, ≥ 3 iteraciones) |
| AEAD | XChaCha20-Poly1305 (nonce de 192 bits) |
| Derivación de Sub-llaves | HKDF-SHA256 |
| Autenticación por Bloque | HMAC-SHA256 (Encrypt-then-MAC) |
| Cifrado JIT en Memoria | XChaCha20 (IV único por activo) |
| CSPRNG | Nativo del SO (`BCryptGenRandom`, `getrandom()`, `crypto.getRandomValues()`) |

## Estructura del Proyecto

```
WhiteLightPass/
├── docs/
│   └── domain/
│       └── definition/
│           ├── system_definition_document.md
│           ├── product_vision_document.md
│           └── requirements_document.md
├── WhiteLightPassDesktop/          # .NET / WinUI
├── WhiteLightPassMobile/           # Kotlin / Android
└── WhiteLightPassWeb/              # Angular / TypeScript
```

## Documentación

La definición del dominio está completamente especificada en tres documentos:

- **[Definición del Sistema](docs/domain/definition/system_definition_document.md)** — Arquitectura abstracta del sistema: ontología, dinámica, invariantes, modelo de adaptación. Agnóstico a la implementación.
- **[Visión del Producto](docs/domain/definition/product_vision_document.md)** — Declaración del problema, propuesta de valor, público objetivo, objetivos, KPIs y anti-objetivos.
- **[Requerimientos](docs/domain/definition/requirements_document.md)** — 42 requerimientos funcionales (10 módulos) y 37 requerimientos no funcionales (8 categorías de atributos de calidad).

## Estado

> **Fase: Definición del Dominio Completa**
>
> La definición del sistema, la visión del producto y los requerimientos funcionales/no funcionales han sido completamente especificados y validados cruzadamente. La arquitectura y la implementación aún no han iniciado.

## Licencia

Por definir.