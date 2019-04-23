---
title: print
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831556"
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
|/d:\<PrinterName>|Especifica la impresora que desea que el trabajo de impresión. Para imprimir en una impresora conectada localmente, especifique el puerto en el equipo donde está conectada la impresora.</br>-Los valores válidos para los puertos paralelos son LPT1, LPT2 y LPT3.</br>-Los valores válidos para los puertos serie son COM1, COM2, COM3 y COM4.</br>También puede especificar una impresora de red mediante su nombre de cola (\\\\*ServerName*\*nombreImpresora *). Si no especifica una impresora, el trabajo de impresión se envía al puerto LPT1 de forma predeterminada.|
|\<Drive>:|Especifica la unidad lógica o física donde se encuentra el archivo que desea imprimir. Este parámetro no es necesario si se encuentra el archivo que desea imprimir en la unidad actual.|
|\<Path>|Especifica la ubicación del archivo que desea imprimir. Este parámetro no es necesario si se encuentra el archivo que desea imprimir en el directorio actual.|
|\<FileName>[ ...]|Obligatorio. Especifica el archivo que desea imprimir. Puede incluir varios archivos en un solo comando.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Puede imprimir un archivo en segundo plano si se envía a una impresora conectada a un puerto serie o en paralelo en el equipo local.
-   Puede realizar muchas tareas de configuración desde el símbolo del sistema mediante la **modo** comando.

    Consulte [modo](mode.md) para obtener más información acerca de:  
    -   Configuración de una impresora conectada a un puerto paralelo
    -   Configuración de una impresora conectada a un puerto serie
    -   Mostrar el estado de una impresora
    -   Preparar una impresora para la modificación de la página de código

## <a name="BKMK_examples"></a>Ejemplos

Para enviar el archivo informe.txt en el directorio actual a una impresora conectada al puerto LPT2 en el equipo local, escriba:
```
print /d:lpt2 report.txt
```
Para enviar el archivo informe.txt en el directorio c:\Accounting a la cola de impresión Printer1 el \\ \\copias servidor, escriba:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Referencia de comandos de impresión](print-command-reference.md)

[Modo](mode.md)