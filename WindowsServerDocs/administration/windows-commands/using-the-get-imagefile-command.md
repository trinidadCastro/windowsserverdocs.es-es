---
title: Mediante el comando get-ImageFile
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827626"
---
# <a name="using-the-get-imagefile-command"></a>Mediante el comando get-ImageFile



Recupera información sobre las imágenes contenidas en un archivo de imagen de Windows (.wim).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ ImageFile:\<ruta del archivo WIM >|Especifica la ruta de acceso y el nombre completo del archivo .wim.|
|[/ Detallada]|Devuelve todos los metadatos de imagen de cada imagen. Si no se utiliza esta opción, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo.|

## <a name="BKMK_examples"></a>Ejemplos

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