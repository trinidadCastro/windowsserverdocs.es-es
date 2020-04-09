---
title: serverweroptin
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18b4a56888b3f23bf3bac4a12b2dba7079b50923
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834638"
---
# <a name="serverweroptin"></a>serverweroptin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|/?|Muestra la Ayuda en el símbolo del sistema.|
## <a name="examples"></a><a name=BKMK_Examples></a>Example
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
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

