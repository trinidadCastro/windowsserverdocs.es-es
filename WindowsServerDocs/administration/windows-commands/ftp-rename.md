---
title: cambiar nombre de FTP
description: Tema de referencia del comando Rename de FTP, que cambia el nombre de los archivos remotos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8d3ea25e48266db6a4a282f2ea395bd8b8d5fd9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820315"
---
# <a name="ftp-rename"></a>cambiar nombre de FTP

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de los archivos remotos.

## <a name="syntax"></a>Sintaxis

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<filename>` | Especifica el archivo cuyo nombre desea cambiar. |
| `<newfilename>` | Especifica el nuevo nombre de archivo. |

### <a name="examples"></a>Ejemplos

Para cambiar el nombre del archivo remoto *example. txt* a *ejemplo1. txt*, escriba:

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
