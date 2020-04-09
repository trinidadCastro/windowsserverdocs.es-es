---
title: attrib
description: Comando de comandos de Windows para **attrib**, que muestra, establece o quita atributos asignados a archivos o directorios.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a6f307450b06dff81180b70f864bd9e1ed5885
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851258"
---
# <a name="attrib"></a>attrib

Muestra, establece o quita atributos asignados a archivos o directorios. Si se usa sin parámetros, **attrib** muestra los atributos de todos los archivos del directorio actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{+|-}r` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo de solo lectura. |
| `{+\|-}a` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo de almacenamiento. |
| `{+\|-}s` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo del sistema. |
| `{+\|-}h` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo oculto. |
| `{+\|-}i` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo no indizado de contenido. |
| `[<Drive>:][<Path>][<FileName>]` | Especifica la ubicación y el nombre del directorio, el archivo o el grupo de archivos para los que desea mostrar o cambiar atributos. Puede **usar el** y **&#42;** caracteres comodín en el parámetro *filename* para mostrar o cambiar los atributos de un grupo de archivos. |
| /s | Aplica el **atributo attrib** y las opciones de línea de comandos a los archivos coincidentes en el directorio actual y en todos sus subdirectorios. |
| /d | Aplica el **atributo attrib** y las opciones de línea de comandos a los directorios. |
| /l | Aplica el **atributo attrib** y las opciones de línea de comandos al vínculo simbólico, en lugar del destino del vínculo simbólico. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

- Puede usar caracteres comodín ( **?** y **&#42;** ) con el parámetro *filename* para mostrar o cambiar los atributos de un grupo de archivos.

- Si un archivo tiene establecido el atributo sistema (**s**) u oculto (**h**), debe borrar el atributo para poder cambiar cualquier otro atributo de dicho archivo.

- El atributo de archivo (**a**) marca los archivos que han cambiado desde la última vez que se realizó una copia de seguridad. Tenga en cuenta que el comando **xcopy** usa atributos de archivo.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar los atributos de un archivo denominado News86 que se encuentra en el directorio actual, escriba:

```
attrib news86 
```

Para asignar el atributo de solo lectura al archivo denominado Report. txt, escriba:

```
attrib +r report.txt 
```

Para quitar el atributo de solo lectura de los archivos del directorio público y sus subdirectorios en un disco de la unidad B, escriba:

```
attrib -r b:\public\*.* /s 
```

Para establecer el atributo de archivo para todos los archivos de la unidad A y, a continuación, borrar el atributo de archivo para los archivos con la extensión. bak, escriba:

```
attrib +a a:*.* & attrib -a a:*.bak 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)