---
title: replace
description: Artículo de referencia para el comando Replace, que puede reemplazar los archivos existentes o agregar nuevos a un directorio.
ms.topic: reference
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5dfab76427a8f91339c29ac37607ce422d4f7e39
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037023"
---
# <a name="replace"></a>replace

Reemplazar los archivos existentes en un directorio. Si se usa con la opción **/a** , este comando agrega nuevos archivos a un directorio en lugar de reemplazar los archivos existentes.

## <a name="syntax"></a>Sintaxis

```
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/a] [/p] [/r] [/w]
replace [<drive1>:][<path1>]<filename> [<drive2>:][<path2>] [/p] [/r] [/s] [/w] [/u]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive1>:][<path1>]<filename>` | Especifica la ubicación y el nombre del archivo de código fuente o del conjunto de archivos. La opción *filename* es obligatoria y puede incluir caracteres comodín (**&#42;** y **?**). |
| `[<drive2>:][<path2>]` | Especifica la ubicación del archivo de destino. No se puede especificar un nombre de archivo para los archivos que se reemplacen. Si no especifica una unidad o una ruta de acceso, este comando usa la unidad y el directorio actuales como destino. |
| /a | Agrega nuevos archivos al directorio de destino en lugar de reemplazar los archivos existentes. No puede usar esta opción de línea de comandos con la opción de línea de comandos **/s** o **/u** . |
| /p | Le pide confirmación antes de reemplazar un archivo de destino o de agregar un archivo de origen. |
| /r | Reemplaza los archivos de solo lectura y los no protegidos. Si intenta reemplazar un archivo de solo lectura, pero no especifica **/r**, se genera un error y se detiene la operación de reemplazo. |
| /w | Espera a que inserte un disco antes de que se inicie la búsqueda de archivos de código fuente. Si no especifica **/w**, este comando comienza a reemplazar o agregar archivos inmediatamente después de presionar entrar. |
| /s | Busca todos los subdirectorios en el directorio de destino y reemplaza los archivos coincidentes. No se puede usar **/s** con la opción de línea de comandos **/a** . El comando no busca en los subdirectorios especificados en *ruta1*. |
| /U | Reemplaza solo los archivos del directorio de destino que sean más antiguos que los del directorio de origen. No se puede usar **/u** con la opción de línea de comandos **/a** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- A medida que este comando agrega o reemplaza archivos, los nombres de archivo aparecen en la pantalla. Una vez realizado este comando, se muestra una línea de resumen en uno de los siguientes formatos:

  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```

- Si utiliza disquetes y necesita cambiar de disco mientras ejecuta este comando, puede especificar la opción de línea de comandos **/w** para que este comando espere a que cambie los discos.

- No puede usar este comando para actualizar archivos ocultos o de sistema.

- En la tabla siguiente se muestra cada código de salida y una breve descripción de su significado:

  | Código de salida | Descripción |
  |--|--|
  | 0 | Este comando ha reemplazado o agregado los archivos correctamente. |
  | 1 | Este comando encontró una versión incorrecta de MS-DOS. |
  | 2 | Este comando no encontró los archivos de origen. |
  | 3 | Este comando no encontró la ruta de acceso de origen o de destino. |
  | 5 | El usuario no tiene acceso a los archivos que desea reemplazar. |
  | 8 | No hay suficiente memoria del sistema para ejecutar el comando. |
  | 11 | El usuario usó la sintaxis incorrecta en la línea de comandos. |

> [!NOTE]
> Puede usar el parámetro ERRORLEVEL en la línea de comandos **If** en un programa por lotes para procesar códigos de salida devueltos por este comando.

## <a name="examples"></a>Ejemplos

Para actualizar todas las versiones de un archivo denominado *PHONES. CLI* (que aparecen en varios directorios de la unidad C:), con la versión más reciente del archivo *PHONES. CLI* de un disquete en la unidad a:, escriba:

```
replace a:\phones.cli c:\ /s
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
