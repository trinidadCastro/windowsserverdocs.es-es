---
title: exec
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f10e28a8da96bc7228af4561fb36824899f2d7a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725743"
---
# <a name="exec"></a>exec



Ejecuta un archivo en el equipo local. El archivo puede ser un script **cmd** .

## <a name="syntax"></a>Sintaxis

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ScriptFile. cmd>|Especifica el archivo de script que se va a ejecutar.|

## <a name="remarks"></a>Observaciones

-   Este comando se usa para duplicar o restaurar datos como parte de una secuencia de copia de seguridad o restauración.
-   Si se produce un error en el script, se devuelve un error y se cierra DiskShadow.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)