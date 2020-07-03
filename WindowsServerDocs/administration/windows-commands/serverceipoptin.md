---
title: serverceipoptin
description: Artículo de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2430907237fd82dc6788c8b68f4de35629f5f35
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935920"
---
# <a name="serverceipoptin"></a>serverceipoptin

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="examples"></a>Ejemplos
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

