---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil Wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fc79b70e8dedb9ecad5e8c6e89f51ece3279faa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376658"
---
# <a name="fsutil-wim"></a>Fsutil Wim
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Proporciona funciones para detectar y administrar archivos respaldados por la imagen de Windows (WIM).

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
|enumfiles (|Enumera los archivos de copia de seguridad de WIM.|
|nombre de \<drive >|Especifica el nombre de la unidad.|
|@no__t > de origen de 0data|Especifica el origen de datos.|
|enumwims|Enumera los archivos WIM de respaldo.|
|queryfile|Consulta si el archivo está respaldado por WIM y, en caso afirmativo, muestra detalles sobre el archivo WIM.|
|@no__t 0filename >|Especifica el nombre de archivo.|
|removewim|Quita un WIM de los archivos de copia de seguridad.|




### <a name="examples"></a>Ejemplos

Para enumerar los archivos de la unidad C: desde el origen de datos 0, escriba:

```
fsutil wim enumfiles C: 0
```

Para enumerar los archivos WIM de respaldo de la unidad C:, escriba:

```
fsutil wim enumwims C:
```

Para ver si un archivo está respaldado por WIM, escriba:

```
fsutil wim C:\Windows\Notepad.exe
```

Para quitar WIM de archivos de copia de seguridad para el volumen C: y el origen de datos 2, escriba:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)