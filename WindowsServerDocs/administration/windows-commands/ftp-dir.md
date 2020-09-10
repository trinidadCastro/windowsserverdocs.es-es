---
title: ftp dir
description: Artículo de referencia del comando FTP dir, que muestra una lista de los archivos de directorio y los subdirectorios de un equipo remoto.
ms.topic: reference
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8641fdca55976eb5998cdfbba58eddd3e6d8f286
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621787"
---
# <a name="ftp-dir"></a>ftp dir

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de los archivos de directorio y los subdirectorios de un equipo remoto.

## <a name="syntax"></a>Sintaxis

```
dir [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| `[<remotedirectory>]` | Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se utiliza el directorio de trabajo actual en el equipo remoto. |
| `[<localfile>]` | Especifica un archivo local en el que se va a almacenar la lista de directorios. Si no se especifica un archivo local, los resultados se muestran en la pantalla. |

### <a name="examples"></a>Ejemplos

Para mostrar una lista de directorios para *dir1* en el equipo remoto, escriba:

```
dir dir1
```

Para guardar una lista del directorio actual en el equipo remoto en el *dirlist.txt*de archivo local, escriba:

```
dir . dirlist.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
