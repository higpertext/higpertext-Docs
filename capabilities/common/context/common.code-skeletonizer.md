# Capability: `common.code-skeletonizer`

**Nombre**: Code Skeletonizer
**Versión**: 1.0.0

## Propósito
Genera una versión 'esqueleto' de un archivo de código fuente (especialmente Python mediante AST, y otros lenguajes mediante expresiones regulares), extrayendo solo firmas de clases, funciones/métodos, docstrings e imports, omitiendo el código de implementación para ahorrar tokens de contexto.

**Entrypoint**: `capabilities/common/scripts/core/context/code_skeletonizer.py`
**Lenguaje**: `python`

## Parámetros

| Parámetro | Requerido | Descripción |
|---|---|---|
| `--path` | Sí | Ruta absoluta o relativa del archivo de código a esqueletizar. |
| `--output` | No | Ruta de destino opcional para guardar el archivo esqueletizado. Si no se especifica, se imprime en consola. |

## Contrato Técnico (Reglas)

- Debe validar que el archivo de entrada exista.
- Debe extraer imports, nombres de clases, docstrings y firmas de métodos/funciones.
- Debe reemplazar la implementación interna de métodos y funciones por el token '...'.
- Para Python debe utilizar la biblioteca 'ast' para un parsing sintáctico robusto.
- Debe imprimir en la salida estándar la representación del esqueleto si no se especifica un archivo de salida.

## Intercepción de Bash

- **Comando Bash equivalente**: `find src -type f`
- **Descripción**: Explorar estructura de archivos de código
- **Ejemplo de reemplazo**: `htx task common.code-skeletonizer --path src/my_module.py`
<!-- higpertext:generated-by=common.docs-sync -->
