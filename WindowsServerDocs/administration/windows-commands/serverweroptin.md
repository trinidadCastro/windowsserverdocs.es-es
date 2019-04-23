---
title: serverweroptin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 29545be99b14042d16a6f3a4118e0746f18b14ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869646"
---
# <a name="serverweroptin"></a>serverweroptin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite habilitar informes de errores.
## <a name="syntax"></a>Sintaxis
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/query|comprueba la configuración actual.|
|/detailed|Envía automáticamente informes detallados.|
|/ summary|Envía automáticamente informes de resumen.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="BKMK_Examples"></a>Ejemplos
Para comprobar la configuración actual, escriba:
```
serverweroptin /query
```
Para enviar automáticamente informes detallados, escriba:
```
serverweroptin /detailed
```
Para enviar automáticamente informes de resumen, escriba
```
serverweroptin /summary
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

