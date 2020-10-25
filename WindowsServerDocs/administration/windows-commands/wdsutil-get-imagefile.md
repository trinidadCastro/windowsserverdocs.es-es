---
title: Get-ImageFile
description: Artículo de referencia de Get-ImageFile, que recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).
ms.topic: reference
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 41183f92d75736afc32dfbbee9d31871b3ef24f9
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524340"
---
# <a name="get-imagefile"></a>Get-ImageFile

Recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ImageFile:\<WIM file path>|Especifica la ruta de acceso completa y el nombre de archivo del archivo. Wim.|
|[/Detailed]|Devuelve todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|

## <a name="examples"></a>Ejemplos

Para ver información acerca de una imagen, escriba:
```
wdsutil /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Para ver información detallada, escriba:
```
wdsutil /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)