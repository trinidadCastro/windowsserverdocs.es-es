---
title: anexar FTP
description: Tema de referencia del comando APPEND de FTP, que anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1b6ab4a6ae0c1654d4335d24f135b2893bdcb7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819145"
---
# <a name="ftp-append"></a>anexar FTP

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.

## <a name="syntax"></a>Sintaxis

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<localfile>` | Especifica el archivo local que se va a agregar. |
| archivoremoto | Especifica el archivo en el equipo remoto al que <localfile> se agrega. Si no usa este parámetro, `<localfile>` se usa el nombre en lugar del nombre de archivo remoto. |

### <a name="examples"></a>Ejemplos

Para anexar *archivo1. txt* a *archivo2. txt* en el equipo remoto, escriba:

```
append file1.txt file2.txt
```

Para anexar el *archivo1. txt* local a un archivo denominado *archivo1. txt* en el equipo remoto.

```
append file1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
