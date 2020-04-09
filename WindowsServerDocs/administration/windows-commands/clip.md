---
title: clip
description: Comando comandos de Windows para clip, que redirige la salida del comando desde la línea de comandos al portapapeles de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d997154a382cf39aa2b877d7a2b84f4ff34157d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847648"
---
# <a name="clip"></a>clip

Redirige la salida del comando desde la línea de comandos al portapapeles de Windows. Después, puede pegar esta salida de texto en otros programas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
<Command> | clip
clip < <FileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de comandos|Especifica un comando cuya salida se desea enviar al portapapeles de Windows.|
|\<nombre de archivo >|Especifica un archivo cuyo contenido desea enviar al portapapeles de Windows.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Puede usar el comando **clip** para copiar los datos directamente en cualquier aplicación que pueda recibir texto del portapapeles.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)