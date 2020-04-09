---
title: Get-ImageFile
description: Tema de comandos de Windows para Get-ImageFile, que recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef1cf2b9eec6739690d286c32d26dd84b07e348c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830998"
---
# <a name="get-imagefile"></a>Get-ImageFile

Recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ImageFile:\<ruta de acceso del archivo WIM >|Especifica la ruta de acceso completa y el nombre de archivo del archivo. Wim.|
|[/Detailed]|Devuelve todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver información acerca de una imagen, escriba:
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Para ver información detallada, escriba:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)