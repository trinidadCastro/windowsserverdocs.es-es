---
title: retain
description: Artículo de referencia de * * * *-
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a0205f3b67bd99ca590c7ffc6fbd04b0eefd94f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883643"
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

