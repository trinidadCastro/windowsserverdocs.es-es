---
title: Tain
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b076e12c833645833f53a06476e62bbf44f2690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384494"
---
# <a name="retain"></a>Tain



Prepara un volumen dinámico simple existente para usarlo como volumen de arranque o del sistema.

## <a name="syntax"></a>Sintaxis

```
retain
```

## <a name="remarks"></a>Comentarios

-   En un disco dinámico de registro de arranque maestro (MBR), este comando crea una entrada de partición en el registro de arranque maestro.
-   En un disco dinámico de tabla de particiones GUID (GPT), este comando crea una entrada de partición en la tabla de particiones GUID.

#### <a name="additional-references"></a>Referencias adicionales

