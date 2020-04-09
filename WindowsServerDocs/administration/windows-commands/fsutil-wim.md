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
ms.openlocfilehash: d4a8f2c008c1a28e498edb7726a8c209e91f41af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843928"
---
# <a name="fsutil-wim"></a>Fsutil Wim
>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

Proporciona funciones para detectar y administrar archivos respaldados por la imagen de Windows (WIM).

## <a name="syntax"></a>Sintaxis

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|enumfiles (|Enumera los archivos de copia de seguridad de WIM.|
|\<nombre de la unidad >|Especifica el nombre de la unidad.|
|\<origen de datos >|Especifica el origen de datos.|
|enumwims|Enumera los archivos WIM de respaldo.|
|queryfile|Consulta si el archivo está respaldado por WIM y, en caso afirmativo, muestra detalles sobre el archivo WIM.|
|\<nombre de archivo >|Especifica el nombre de archivo.|
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)