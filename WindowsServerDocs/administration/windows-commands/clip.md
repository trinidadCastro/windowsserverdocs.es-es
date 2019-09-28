---
title: clip
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379354"
---
# <a name="clip"></a>clip



Redirige la salida del comando desde la línea de comandos al portapapeles de Windows. Después, puede pegar esta salida de texto en otros programas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Command >|Especifica un comando cuya salida se desea enviar al portapapeles de Windows.|
|\<Nombre de archivo >|Especifica un archivo cuyo contenido desea enviar al portapapeles de Windows.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Puede usar el comando **clip** para copiar los datos directamente en cualquier aplicación que pueda recibir texto del portapapeles.

## <a name="BKMK_examples"></a>Example

Para copiar la lista de directorios actual en el portapapeles de Windows, escriba:
```
dir | clip
```
Para copiar la salida de un programa llamado Generic. awk en el portapapeles de Windows, escriba:
```
awk -f generic.awk input.txt | clip
```
Para copiar el contenido de un archivo denominado README. txt en el portapapeles de Windows, escriba:
```
clip < readme.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)