---
title: ftp rename
description: Artículo de referencia del comando Rename de FTP, que cambia el nombre de los archivos remotos.
ms.topic: reference
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ea7862a759779a5f767b8e18cdd5a0b36db2a6a1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627638"
---
# <a name="ftp-rename"></a>ftp rename

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

Para cambiar el nombre del archivo remoto *example.txt* a *example1.txt*, escriba:

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
