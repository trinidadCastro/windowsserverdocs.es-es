---
title: type
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 37f66d54983c002d5d09db5cb255d01635a534de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392323"
---
# <a name="type"></a>type


En el shell de comandos de Windows, **Escriba** es un comando integrado que muestra el contenido de un archivo de texto. Use el comando **Type** para ver un archivo de texto sin modificarlo.


En PowerShell, el **tipo** es un alias integrado para el cmdlet **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , que también muestra el contenido de un archivo, pero con una sintaxis diferente.


Para obtener ejemplos de cómo usar este comando en el shell de comandos de Windows (cmd. exe), vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive >:] [\<Path >] \<FileName >|Especifica la ubicación y el nombre del archivo o los archivos que desea ver. Separe varios nombres de archivo con espacios.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si *filename* contiene espacios, escríbalo entre comillas (por ejemplo, "nombre de archivo que contiene espacios. txt").
-   Si muestra un archivo binario o un archivo creado por un programa, es posible que vea caracteres extraños en la pantalla, incluidos los caracteres avance y los símbolos de secuencia de escape. Estos caracteres representan códigos de control que se usan en el archivo binario. En general, evite usar el comando **Type** para mostrar archivos binarios.

## <a name="BKMK_examples"></a>Example

Para mostrar el contenido de un archivo denominado festivo. mar, escriba:
```
type holiday.mar 
```
Para mostrar el contenido de un archivo largo denominado festividad. marzo de una pantalla a la vez, escriba:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
