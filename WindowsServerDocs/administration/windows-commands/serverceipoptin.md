---
title: serverceipoptin
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0625694053a2d673cd86e12d6f6d54662a6c81d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834684"
---
# <a name="serverceipoptin"></a>serverceipoptin

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite participar en el Programa para la mejora de la experiencia del usuario (CEIP).
## <a name="syntax"></a>Sintaxis
```
serverceipoptin [/query] [/enable] [/disable]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Query|comprueba la configuración actual.|
|/Enable|Habilita la participación.|
|/Disable|Deshabilita la participación.|
|/?|Muestra la Ayuda en el símbolo del sistema.|
## <a name="examples"></a><a name=BKMK_Examples></a>Example
Para comprobar la configuración actual, escriba:
```
serverceipoptin /query
```
Para habilitar la participación, escriba:
```
serverceipoptin /enable
```
Para deshabilitar la participación, escriba:
```
serverceipoptin /disable
```
## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

