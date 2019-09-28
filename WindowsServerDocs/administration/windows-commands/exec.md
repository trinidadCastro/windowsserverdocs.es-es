---
title: ejec
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377435"
---
# <a name="exec"></a>ejec



Ejecuta un archivo en el equipo local. El archivo puede ser un script **cmd** .

## <a name="syntax"></a>Sintaxis

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t -0ScriptFile. cmd >|Especifica el archivo de script que se va a ejecutar.|

## <a name="remarks"></a>Comentarios

-   Este comando se usa para duplicar o restaurar datos como parte de una secuencia de copia de seguridad o restauración.
-   Si se produce un error en el script, se devuelve un error y se cierra DiskShadow.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)