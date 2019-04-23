---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840056"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crea un vínculo entre un archivo existente y un nuevo archivo de disco duro.

## <a name="syntax"></a>Sintaxis

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|crear|Establece un vínculo físico de NTFS entre un archivo existente y un nuevo archivo. (Un vínculo físico NTFS es similar a un vínculo físico POSIX).|
|\<NewFileName>|Especifica el archivo que desea crear un vínculo físico al.|
|\<ExistingFileName>|Especifica el archivo que desea crear un vínculo físico desde.|
|list|Enumera los vínculos físicos para *Filename*.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|

## <a name="remarks"></a>Comentarios

-   Un vínculo físico es una entrada de directorio para un archivo. Todos los archivos se pueden considerar tener al menos un vínculo físico. En los volúmenes NTFS, cada archivo puede tener varios vínculos físicos, por lo que un único archivo puede aparecer en muchos directorios (o incluso en el mismo directorio con nombres diferentes). Dado que todos los vínculos de referencia al mismo archivo, programas pueden abrir cualquiera de los vínculos y modificar el archivo. Después de que se haya eliminado todos los vínculos a él, se elimina un archivo del sistema de archivos. Después de crear un vínculo físico, programas pueden utilizar como cualquier otro nombre de archivo.

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


