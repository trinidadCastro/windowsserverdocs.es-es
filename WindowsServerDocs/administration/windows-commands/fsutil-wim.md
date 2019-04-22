---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Wim de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826046"
---
# <a name="fsutil-wim"></a>Wim de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Proporciona funciones para detectar y administrar archivos WIM de imagen de Windows de seguridad.

## <a name="syntax"></a>Sintaxis

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|EnumFiles|Enumera los archivos WIM de seguridad.|
|\<nombre de la unidad >|Especifica el nombre de la unidad.|
|\<origen de datos >|Especifica el origen de datos.|
|enumwims|Enumera los archivos WIM de respaldo.|
|queryfile|Las consultas si el archivo está respaldado por WIM y si es así, muestra detalles acerca del archivo WIM.|
|\<filename>|Especifica el nombre de archivo.|
|removewim|Quita un archivo WIM de seguridad de archivos.|




### <a name="examples"></a>Ejemplos

Para enumerar los archivos de la unidad C: de origen de datos 0, escriba:

```
fsutil wim enumfiles C: 0
```

Para enumerar los archivos WIM de respaldo para la unidad C:, escriba:

```
fsutil wim enumwims C:
```

Para ver si un archivo está respaldado por WIM, escriba:

```
fsutil wim C:\Windows\Notepad.exe
```

Para quitar el archivo WIM de seguridad de archivos para el origen de datos 2 y el volumen C:, escriba:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)