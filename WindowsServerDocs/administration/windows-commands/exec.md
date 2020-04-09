---
title: exec
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d39fbf948050dd00f329e461c34c2365030cb05d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844998"
---
# <a name="exec"></a>exec



ejecuta un archivo en el equipo local. El archivo puede ser un script **cmd** .

## <a name="syntax"></a>Sintaxis

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ScriptFile. cmd >|Especifica el archivo de script que se va a ejecutar.|

## <a name="remarks"></a>Comentarios

-   Este comando se usa para duplicar o restaurar datos como parte de una secuencia de copia de seguridad o restauración.
-   Si se produce un error en el script, se devuelve un error y se cierra DiskShadow.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)