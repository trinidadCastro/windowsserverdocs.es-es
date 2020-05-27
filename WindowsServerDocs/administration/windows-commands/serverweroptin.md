---
title: serverweroptin
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6329552d3525a1330286e04c6400378b14039fbf
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821275"
---
# <a name="serverweroptin"></a>serverweroptin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite habilitar el informe de errores.
## <a name="syntax"></a>Sintaxis
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Query|comprueba la configuración actual.|
|/detailed|Envía informes detallados automáticamente.|
|/Summary|Envía informes de Resumen de forma automática.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="examples"></a>Ejemplos
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

