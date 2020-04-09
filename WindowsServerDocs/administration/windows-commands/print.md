---
title: print
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36966d8d3beb032ee0dcee50d9bd5bc0111bf4f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837378"
---
# <a name="print"></a>print



Envía un archivo de texto a una impresora.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d:\<Nombredeimpresora >|Especifica la impresora en la que desea imprimir el trabajo. Para imprimir en una impresora conectada localmente, especifique el puerto del equipo en el que está conectada la impresora.</br>-Los valores válidos para puertos paralelos son LPT1, LPT2 y LPT3.</br>-Los valores válidos para los puertos serie son COM1, COM2, COM3 y COM4.</br>También puede especificar una impresora de red mediante su nombre de cola (\\\\*ServerName*\*nombredeimpresora *). Si no especifica una impresora, el trabajo de impresión se envía a LPT1 de forma predeterminada.|
|> de \<unidad:|Especifica la unidad física o lógica en la que se encuentra el archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir se encuentra en la unidad actual.|
|\<ruta de acceso >|Especifica la ubicación del archivo que desea imprimir. Este parámetro no es necesario si el archivo que desea imprimir está ubicado en el directorio actual.|
|\<nombre de archivo > [...]|Obligatorio. Especifica el archivo que desea imprimir. Puede incluir varios archivos en un solo comando.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Un archivo puede imprimir en segundo plano si lo envía a una impresora conectada a un puerto serie o paralelo en el equipo local.
-   Puede realizar muchas tareas de configuración desde el símbolo del sistema mediante el comando **modo** .

    Vea el [modo](mode.md) para obtener más información acerca de:  
    -   Configuración de una impresora conectada a un puerto paralelo
    -   Configurar una impresora conectada a un puerto serie
    -   Mostrar el estado de una impresora
    -   Preparar una impresora para el cambio de páginas de códigos

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para enviar el archivo report. txt en el directorio actual a una impresora conectada a LPT2 en el equipo local, escriba:
```
print /d:lpt2 report.txt
```
Para enviar el archivo report. txt en el directorio c:\Accounting a la cola de impresión Printer1 en el \\\\servidor CopyRoom, escriba:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Referencia de comandos de impresión](print-command-reference.md)

[Modo](mode.md)