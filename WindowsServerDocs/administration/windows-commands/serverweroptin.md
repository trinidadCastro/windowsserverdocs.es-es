---
title: serverweroptin
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7d5791e059d31e416f848f6e8df648c8f9bd27d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371019"
---
# <a name="serverweroptin"></a>serverweroptin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite habilitar el informe de errores.
## <a name="syntax"></a>Sintaxis
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Query|comprueba la configuración actual.|
|/detailed|Envía informes detallados automáticamente.|
|/Summary|Envía informes de Resumen de forma automática.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="BKMK_Examples"></a>Example
Para comprobar la configuración actual, escriba:
```
serverweroptin /query
```
Para enviar automáticamente informes detallados, escriba:
```
serverweroptin /detailed
```
Para enviar automáticamente informes de Resumen, escriba
```
serverweroptin /summary
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

