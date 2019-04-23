---
title: diskperf
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852586"
---
# <a name="diskperf"></a>diskperf



En Windows 2000, los contadores de rendimiento de disco físico y lógico no están habilitados de forma predeterminada.

**Diskperf** se incluye en Windows XP, Windows Server 2003, Windows Server 2008, Windows Vista, Windows Server 2008 R2 y Windows 7 para que se puede usar para habilitar o deshabilitar los contadores de rendimiento de disco lógico o físico en equipos que ejecutan de forma remota Windows 2000.

## <a name="syntax"></a>Sintaxis

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opciones

|Opción|Descripción|
|------|-----------|
|-?|Sensible al contexto muestra la Ayuda.|
|-Y|Iniciar todos los contadores de rendimiento de disco cuando se reinicia el equipo.|
|-YD|Habilitar los contadores de rendimiento de disco para unidades físicas al reiniciar el equipo.|
|-YV|Habilitar los contadores de rendimiento de disco para las unidades lógicas o volúmenes de almacenamiento cuando se reinicia el equipo.|
|-N|Deshabilitar todos los contadores de rendimiento de disco cuando se reinicia el equipo.|
|-ND|Deshabilitar los contadores de rendimiento de disco para unidades físicas al reiniciar el equipo.|
|-NV|Deshabilitar los contadores de rendimiento de disco para las unidades lógicas o volúmenes de almacenamiento cuando se reinicia el equipo.|
|\\\\*\<computername>*|Especifique el nombre del equipo donde desea habilitar o deshabilitar los contadores de rendimiento de disco.|