---
title: imprimir
description: Artículo de referencia para el comando imprimir, que envía un archivo de texto a una impresora.
ms.topic: reference
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecd679a3891a073bd73c0526c395dc67c2cf0933
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035223"
---
# <a name="print"></a>imprimir

Envía un archivo de texto a una impresora. Un archivo puede imprimir en segundo plano si lo envía a una impresora conectada a un puerto serie o paralelo en el equipo local.

> [!NOTE]
> Puede realizar muchas tareas de configuración desde el símbolo del sistema mediante el [comando modo](mode.md), incluida la configuración de una impresora conectada a un puerto paralelo o serie, la visualización del estado de la impresora o la preparación de una impresora para el cambio de páginas de códigos.

## <a name="syntax"></a>Sintaxis

```
print [/d:<printername>] [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /d.`<printername>` | Especifica la impresora en la que desea imprimir el trabajo. Para imprimir en una impresora conectada localmente, especifique el puerto del equipo en el que está conectada la impresora. Los valores válidos para puertos paralelos son **LPT1**, **LPT2**y **LPT3**. Los valores válidos para los puertos serie son **COM1**, **COM2**, **COM3**y **COM4**. También puede especificar una impresora de red mediante su nombre de cola ( `\\server_name\printer_name` ). Si no especifica una impresora, el trabajo de impresión se envía a **LPT1** de forma predeterminada. |
| `<drive>`: | Especifica la unidad física o lógica en la que se encuentra el archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir se encuentra en la unidad actual. |
| `<path>` | Especifica la ubicación del archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir está ubicado en el directorio actual. |
| `<filename>[ ...]` | Necesario. Especifica el archivo que desea imprimir. Puede incluir varios archivos en un solo comando. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para enviar el archivo de **report.txt** , que se encuentra en el directorio actual, a una impresora conectada a **LPT2** en el equipo local, escriba:

```
print /d:lpt2 report.txt
```

Para enviar el archivo de **report.txt** , ubicado en el directorio **c:\accounting** , a la cola de impresión **Printer1** en el servidor **/d: \\ copyroom** , escriba:

```
print /d:\\copyroom\printer1 c:\accounting\report.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)

- [Modo (comando)](mode.md)