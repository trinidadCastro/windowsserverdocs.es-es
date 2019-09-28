---
title: diskperf
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377813"
---
# <a name="diskperf"></a>diskperf



En Windows 2000, los contadores de rendimiento de disco físico y lógico no están habilitados de forma predeterminada.

**Diskperf** se incluye en Windows XP, windows Server 2003, windows Server 2008, Windows Vista, windows Server 2008 R2 y Windows 7 para que se pueda usar para habilitar o deshabilitar de forma remota los contadores de rendimiento de disco físico o lógico en equipos que ejecutan Windows 2000.

## <a name="syntax"></a>Sintaxis

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opciones

|Opción|Descripción|
|------|-----------|
|-?|Muestra la ayuda contextual.|
|-Y|Inicie todos los contadores de rendimiento de disco cuando se reinicie el equipo.|
|-YD|Habilite los contadores de rendimiento de disco para las unidades físicas cuando se reinicie el equipo.|
|-YV|Habilite los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicie el equipo.|
|-N|Deshabilite todos los contadores de rendimiento de disco cuando se reinicie el equipo.|
|-ND|Deshabilite los contadores de rendimiento de disco para las unidades físicas cuando se reinicie el equipo.|
|-NV|Deshabilite los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicie el equipo.|
|\\ @ no__t-1 *\<computername >*|Especifique el nombre del equipo en el que desea habilitar o deshabilitar los contadores de rendimiento del disco.|