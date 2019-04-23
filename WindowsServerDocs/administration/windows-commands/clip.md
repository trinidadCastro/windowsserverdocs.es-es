---
title: clip
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b10876e115c1f0dcac3448948003852449012087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862396"
---
# <a name="clip"></a>clip



Redirige la salida del comando desde la línea de comandos en el Portapapeles de Windows. A continuación, puede pegar este texto de salida en otros programas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Command>|Especifica un comando cuya salida desea enviar en el Portapapeles de Windows.|
|\<FileName>|Especifica un archivo cuyo contenido desea enviar en el Portapapeles de Windows.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Puede usar el **clip** comando para copiar datos directamente en cualquier aplicación que pueda recibir texto desde el Portapapeles.

## <a name="BKMK_examples"></a>Ejemplos

Para copiar el directorio actual listado en el Portapapeles de Windows, escriba:
```
dir | clip
```
Para copiar la salida de un programa llamado Generic.awk en el Portapapeles de Windows, escriba:
```
awk -f generic.awk input.txt | clip
```
Para copiar el contenido de un archivo denominado Readme.txt en el Portapapeles de Windows, escriba:
```
clip < readme.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)