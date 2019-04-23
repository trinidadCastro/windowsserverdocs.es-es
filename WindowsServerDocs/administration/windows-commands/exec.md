---
title: exec
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882926"
---
# <a name="exec"></a>exec



ejecuta un archivo en el equipo local. El archivo puede ser un **cmd** secuencia de comandos.

## <a name="syntax"></a>Sintaxis

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ScriptFile.cmd>|Especifica el archivo de script para ejecutar.|

## <a name="remarks"></a>Comentarios

-   Este comando se usa para duplicar o restaurar los datos como parte de una copia de seguridad o la secuencia de restauración.
-   Si se produce un error en la secuencia de comandos, se devuelve un error y sale de DiskShadow.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)