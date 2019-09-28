---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: de9a04850555b11a42d74826685c249bea15710c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376876"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crea un vínculo físico entre un archivo existente y un archivo nuevo.

## <a name="syntax"></a>Sintaxis

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|crear|Establece un vínculo físico NTFS entre un archivo existente y un archivo nuevo. (Un vínculo físico NTFS es similar a un vínculo físico de POSIX).|
|@no__t 0NewFileName >|Especifica el archivo al que desea crear un vínculo físico.|
|@no__t 0ExistingFileName >|Especifica el archivo desde el que desea crear un vínculo físico.|
|list|Muestra el hardlinks al *nombre de archivo*.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|

## <a name="remarks"></a>Comentarios

-   Un vínculo físico es una entrada de directorio para un archivo. Se puede considerar que cada archivo tiene al menos un vínculo físico. En volúmenes NTFS, cada archivo puede tener varios vínculos físicos, por lo que un único archivo puede aparecer en muchos directorios (o incluso en el mismo directorio con nombres diferentes). Dado que todos los vínculos hacen referencia al mismo archivo, los programas pueden abrir cualquiera de los vínculos y modificar el archivo. Un archivo se elimina del sistema de archivos solo después de que se hayan eliminado todos los vínculos a él. Después de crear un vínculo físico, los programas pueden usarlo como cualquier otro nombre de archivo.

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


