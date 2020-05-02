---
title: retain
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7391c0b60d07cc2eb5a6230b283ac180cc028508
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722339"
---
# <a name="retain"></a>retain



Prepara un volumen dinámico simple existente para usarlo como volumen de arranque o del sistema.

## <a name="syntax"></a>Sintaxis

```
retain
```

## <a name="remarks"></a>Observaciones

-   En un disco dinámico de registro de arranque maestro (MBR), este comando crea una entrada de partición en el registro de arranque maestro.
-   En un disco dinámico de tabla de particiones GUID (GPT), este comando crea una entrada de partición en la tabla de particiones GUID.

## <a name="additional-references"></a>Referencias adicionales

