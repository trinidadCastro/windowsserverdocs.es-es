---
title: MLS de FTP
description: Tema de referencia del comando de FTP MLS, que muestra una lista abreviada de archivos y subdirectorios de un directorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 372c0b9a42fdfb8600083a301b71c37ada43c014
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820398"
---
# <a name="ftp-mls"></a>MLS de FTP

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios de un directorio remoto.

## <a name="syntax"></a>Sintaxis

```
mls <remotefile>[ ] <localfile>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el archivo para el que desea ver una lista. Al especificar *remotefiles*, use un guion para representar el directorio de trabajo actual en el equipo remoto. |
| `<localfile>` | Especifica un archivo local en el que se va a almacenar la lista. Al especificar *archivolocal*, use un guion para mostrar la lista en pantalla. |

### <a name="examples"></a>Ejemplos

Para mostrar una lista abreviada de archivos y subdirectorios para *dir1* y *dir2*, escriba:

```
mls dir1 dir2 -
```

Para guardar una lista abreviada de archivos y subdirectorios para *dir1* y *dir2* en el archivo local *dirlist. txt*, escriba:

```
mls dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
