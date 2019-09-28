---
title: print
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ada0657e2f17754e55e97e6488aac99fb0025afb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372154"
---
# <a name="print"></a>print



Envía un archivo de texto a una impresora.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d: @no__t 0PrinterName >|Especifica la impresora en la que desea imprimir el trabajo. Para imprimir en una impresora conectada localmente, especifique el puerto del equipo en el que está conectada la impresora.</br>-Los valores válidos para puertos paralelos son LPT1, LPT2 y LPT3.</br>-Los valores válidos para los puertos serie son COM1, COM2, COM3 y COM4.</br>También puede especificar una impresora de red mediante su nombre de cola (\\ @ no__t-1*ServerName*\*PrinterName *). Si no especifica una impresora, el trabajo de impresión se envía a LPT1 de forma predeterminada.|
|@no__t 0Drive >:|Especifica la unidad física o lógica en la que se encuentra el archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir se encuentra en la unidad actual.|
|@no__t 0Path >|Especifica la ubicación del archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir está ubicado en el directorio actual.|
|\<FileName > [...]|Obligatorio. Especifica el archivo que desea imprimir. Puede incluir varios archivos en un solo comando.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Un archivo puede imprimir en segundo plano si lo envía a una impresora conectada a un puerto serie o paralelo en el equipo local.
-   Puede realizar muchas tareas de configuración desde el símbolo del sistema mediante el comando **modo** .

    Vea el [modo](mode.md) para obtener más información acerca de:  
    -   Configuración de una impresora conectada a un puerto paralelo
    -   Configurar una impresora conectada a un puerto serie
    -   Mostrar el estado de una impresora
    -   Preparar una impresora para el cambio de páginas de códigos

## <a name="BKMK_examples"></a>Example

Para enviar el archivo report. txt en el directorio actual a una impresora conectada a LPT2 en el equipo local, escriba:
```
print /d:lpt2 report.txt
```
Para enviar el archivo report. txt en el directorio c:\Accounting a la cola de impresión Printer1 del servidor \\ @ no__t-1CopyRoom, escriba:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Referencia de comandos de impresión](print-command-reference.md)

[Modo](mode.md)