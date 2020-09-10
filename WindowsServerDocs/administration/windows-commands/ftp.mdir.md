---
title: ftp mdir
description: Artículo de referencia para el comando FTP MDIR, que muestra una lista de directorios de archivos y subdirectorios de un directorio remoto.
ms.topic: reference
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 40c64d82f07a763dec3dd690780438eb2db35753
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636661"
---
# <a name="ftp-mdir"></a>ftp mdir

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de directorios de archivos y subdirectorios de un directorio remoto.

## <a name="syntax"></a>Sintaxis

```
mdir <remotefile>[...] <localfile>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el directorio o el archivo para el que desea ver una lista. Puede especificar varios *remotefiles*. Escriba un guión (-) para usar el directorio de trabajo actual en el equipo remoto. |
| `<localfile>` | Especifica un archivo local para almacenar la lista. Este parámetro es obligatorio. Escriba un guión (-) para mostrar la lista en pantalla. |

### <a name="examples"></a>Ejemplos

Para mostrar una lista de directorios de *dir1* y *dir2* en la pantalla, escriba:

```
mdir dir1 dir2 -
```

Para guardar la lista de directorios combinada de *dir1* y *dir2* en un archivo local denominado *dirlist.txt*, escriba:

```
mdir dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
