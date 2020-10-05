---
title: sxstrace
description: Artículo de referencia para el comando systrace, que ayuda a diagnosticar problemas en paralelo.
ms.topic: reference
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 95f52993db56e61b3a1cd4d26a0ef36626421a15
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718212"
---
# <a name="sxstrace"></a>sxstrace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Diagnostica problemas en paralelo.

## <a name="syntax"></a>Sintaxis

```
sxstrace [{[trace -logfile:<filename> [-nostop]|[parse -logfile:<filename> -outfile:<parsedfile>  [-filter:<appname>]}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| seguimiento | Habilita el seguimiento para en paralelo. |
| -logfile | Especifica el archivo de registro sin procesar. |
| `<filename>` | Guarda el registro de seguimiento en `<filename` . |
| -NoStop | Especifica que no se debe recibir un aviso para detener el seguimiento. |
| parse | Traduce el archivo de seguimiento sin procesar. |
| -OUTFILE | Especifica el nombre de archivo de salida. |
| `<parsedfile>` | Especifica el nombre de archivo del archivo analizado. |
| -filter | Permite filtrar la salida. |
| `<appname>` | Especifica el nombre de la aplicación. |
| stoptrace | Detiene el seguimiento, en caso de que no se haya detenido antes. |
| -? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para habilitar el seguimiento y guardar el archivo de seguimiento en *sxstrace. ETL*, escriba:

```
sxstrace trace -logfile:sxstrace.etl
```

Para convertir el archivo de seguimiento sin procesar en un formato legible y guardar el resultado en *sxstrace.txt*, escriba:

```
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
