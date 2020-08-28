---
title: lpr
description: Artículo de referencia para el comando lpr, que envía un archivo a un equipo o un dispositivo de uso compartido de impresoras que ejecuta el servicio line Printer daemon (LPD) como preparación para la impresión.
ms.topic: reference
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ab663ab089c6727e7354accda5dc43b2d94c946
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036343"
---
# <a name="lpr"></a>lpr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía un archivo a un equipo o a un dispositivo de uso compartido de impresoras que ejecuta el servicio line Printer daemon (LPD) como preparación para la impresión.

## <a name="syntax"></a>Sintaxis

```
lpr [-S <servername>] -P <printername> [-C <bannercontent>] [-J <jobname>] [-o | -o l] [-x] [-d] <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -S `<servername>` | Especifica (por nombre o dirección IP) el equipo o el dispositivo de uso compartido de impresoras que hospeda la cola de impresión de LPD con el estado que desea mostrar.  Este parámetro es obligatorio y debe escribirse en mayúsculas. |
| -P `<printername> `| Especifica (por nombre) la impresora para la cola de impresión con el estado que desea mostrar. Para buscar el nombre de la impresora, abra la carpeta **impresoras** . Este parámetro es obligatorio y debe escribirse en mayúsculas. |
| -C `<bannercontent>` | Especifica el contenido que se va a imprimir en la página de encabezado del trabajo de impresión. Si no incluye este parámetro, aparece el nombre del equipo desde el que se envió el trabajo de impresión en la página de encabezado. Este parámetro debe escribirse en mayúsculas. |
| -J `<jobname>` | Especifica el nombre del trabajo de impresión que se imprimirá en la página de encabezado. Si no incluye este parámetro, el nombre del archivo que se va a imprimir aparece en la página de encabezado. Este parámetro debe escribirse en mayúsculas. |
| `[-o | -o l]` | Especifica el tipo de archivo que desea imprimir. El parámetro **-o** especifica que desea imprimir un archivo de texto. El parámetro **-o l** especifica que desea imprimir un archivo binario (por ejemplo, un archivo PostScript). |
| -d | Especifica que el archivo de datos debe enviarse antes que el archivo de control. Use este parámetro si la impresora requiere que se envíe primero el archivo de datos. Para obtener más información, consulte la documentación de la impresora. |
| -X | Especifica que el comando **LPR** debe ser compatible con el sistema operativo Sun Microsystems (denominado SunOS) para las versiones hasta la versión 4.1.4_u1. |
| `<filename>` | Especifica (por nombre) el archivo que se va a imprimir. Este parámetro es obligatorio. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para imprimir el archivo de texto *Document.txt* en la cola de impresión de *Laserprinter1* en un host de LPD en *10.0.0.45*, escriba:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt
```

Para imprimir el archivo *PostScript_file. PS* de Adobe PostScript en la cola de impresión de *Laserprinter1* en un host de LPD en *10.0.0.45*, escriba:

```
lpr -S 10.0.0.45 -P Laserprinter1 -o l PostScript_file.ps
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
