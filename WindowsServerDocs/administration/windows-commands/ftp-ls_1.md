---
title: FTP LS
description: Tema de referencia del comando FTP LS, que muestra una lista abreviada de archivos y subdirectorios del equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae913f001c3ddffce9ff81c9c5c5fd32f436da5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820175"
---
# <a name="ftp-ls"></a>FTP LS

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios del equipo remoto.

## <a name="syntax"></a>Sintaxis

```
ls [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| `[<remotedirectory>]` | Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se utiliza el directorio de trabajo actual en el equipo remoto. |
| `[<localfile>]` | Especifica un archivo local en el que se va a almacenar la lista. Si no se especifica un archivo local, los resultados se muestran en la pantalla. |

### <a name="examples"></a>Ejemplos

Para mostrar una lista abreviada de archivos y subdirectorios del equipo remoto, escriba:

```
ls
```

Para obtener una lista abreviada de directorios de *dir1* en el equipo remoto y guardarlo en un archivo local denominado *dirlist. txt*, escriba:

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
