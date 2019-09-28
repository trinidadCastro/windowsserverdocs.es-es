---
title: Usar el comando Get-ImageFile
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c8136585e04caca02ab16c7b4ca11a825cf400d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392137"
---
# <a name="using-the-get-imagefile-command"></a>Usar el comando Get-ImageFile



Recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ImageFile: ruta de acceso del archivo \<WIM >|Especifica la ruta de acceso completa y el nombre de archivo del archivo. Wim.|
|[/Detailed]|Devuelve todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|

## <a name="BKMK_examples"></a>Example

Para ver información acerca de una imagen, escriba:
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Para ver información detallada, escriba:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)