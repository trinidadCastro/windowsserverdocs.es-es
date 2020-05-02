---
title: diskperf
description: Tema de referencia de diskperf, que puede usarse para habilitar o deshabilitar de forma remota los contadores de rendimiento de disco físico o lógico en equipos que ejecutan Windows 2000.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8f505d924ee1de311f2f2736ff65be844c3f2ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719445"
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
|-y|Inicie todos los contadores de rendimiento de disco cuando se reinicie el equipo.|
|-YD|Habilite los contadores de rendimiento de disco para las unidades físicas cuando se reinicie el equipo.|
|-YV|Habilite los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicie el equipo.|
|-n|Deshabilite todos los contadores de rendimiento de disco cuando se reinicie el equipo.|
|-ND|Deshabilite los contadores de rendimiento de disco para las unidades físicas cuando se reinicie el equipo.|
|-NV|Deshabilite los contadores de rendimiento de disco para unidades lógicas o volúmenes de almacenamiento cuando se reinicie el equipo.|
|\\\\*\<COMPUTERNAME>*|Especifique el nombre del equipo en el que desea habilitar o deshabilitar los contadores de rendimiento del disco.|