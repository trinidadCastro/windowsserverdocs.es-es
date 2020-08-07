---
title: Get-ImageFile
description: Artículo de referencia de Get-ImageFile, que recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (. wim).
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10b86929366cc8734a5ff155200fb3078f0cd5d4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879372"
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
|/ImageFile:\<WIM file path>|Especifica la ruta de acceso completa y el nombre de archivo del archivo. Wim.|
|[/Detailed]|Devuelve todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|

## <a name="examples"></a>Ejemplos

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