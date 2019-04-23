---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: fsutil dirty
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c308b0497a5a39a25384b22441b733143df8727b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852136"
---
# <a name="fsutil-dirty"></a>fsutil dirty
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Las consultas o establece el bit de integridad de un volumen. Cuando un volumen del desfasadas bit está establecido, **autochk** comprueba automáticamente si hay errores en el volumen la próxima vez que se reinicie el equipo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|query|Consulta el bit de integridad del volumen especificado.|
|set|Establece el bit de integridad del volumen especificado.|
|\<VolumePath>|Especifica el nombre de la unidad seguido por un signo de dos puntos o un GUID en el formato siguiente: **Volume{***GUID***}**.|

## <a name="remarks"></a>Comentarios

-   Bit de integridad de un volumen indica que el sistema de archivos puede estar en un estado incoherente. El bit de integridad se puede establecer porque:

    -   El volumen está conectado y tiene cambios pendientes.

    -   Se realizaron cambios en el volumen y el equipo se cerró antes de que los cambios se confirmaron en el disco.

    -   Se detectaron daños en el volumen.

-   Si se establece el bit de integridad cuando se reinicia el equipo, **chkdsk** se ejecuta para comprobar la integridad del sistema de archivos y para intentar corregir cualquier problema con el volumen.

## <a name="BKMK_examples"></a>Ejemplos
Para consultar el bit de integridad de la unidad C, escriba:

```
fsutil dirty query c:
```

-   Si el volumen está dañado, se muestra el siguiente resultado:

    `Volume C: is dirty`

-   Si el volumen no contiene errores, se muestra el siguiente resultado:

    `Volume C: is not dirty`

Para establecer el bit de integridad de la unidad C, escriba:

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


