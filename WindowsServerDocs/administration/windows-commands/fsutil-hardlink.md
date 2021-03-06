---
title: fsutil hardlink
description: Artículo de referencia para el comando fsutil hardlink, que crea un vínculo físico entre un archivo existente y un archivo nuevo.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 9d3782736686106b892f3b35e74573e4e162ae80
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037363"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Crea un vínculo físico entre un archivo existente y un archivo nuevo. Un vínculo físico es una entrada de directorio para un archivo. Se puede considerar que cada archivo tiene al menos un vínculo físico.

En volúmenes NTFS, cada archivo puede tener varios vínculos físicos, por lo que un único archivo puede aparecer en muchos directorios (o incluso en el mismo directorio con nombres diferentes). Dado que todos los vínculos hacen referencia al mismo archivo, los programas pueden abrir cualquiera de los vínculos y modificar el archivo. Un archivo se elimina del sistema de archivos solo después de que se hayan eliminado todos los vínculos a él. Después de crear un vínculo físico, los programas pueden usarlo como cualquier otro nombre de archivo.

## <a name="syntax"></a>Sintaxis

```
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| create | Establece un vínculo físico NTFS entre un archivo existente y un archivo nuevo. (Un vínculo físico NTFS es similar a un vínculo físico de POSIX). |
| \<newfilename> | Especifica el archivo al que desea crear un vínculo físico. |
| \<existingfilename> | Especifica el archivo desde el que desea crear un vínculo físico. |
| list | Muestra los vínculos físicos al *nombre de archivo*. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
