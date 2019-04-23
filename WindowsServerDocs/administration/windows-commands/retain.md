---
title: Conservar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852176"
---
# <a name="retain"></a>Conservar



Prepara un volumen simple dinámico existente que se usará como un arranque o volumen del sistema.

## <a name="syntax"></a>Sintaxis

```
retain
```

## <a name="remarks"></a>Comentarios

-   En un disco dinámico de arranque maestro (MBR) del registro, este comando crea una entrada de partición en el registro de arranque maestro.
-   En un disco dinámico de la tabla (GPT) de particiones GUID, este comando crea una entrada de partición en la tabla de particiones GUID.

#### <a name="additional-references"></a>Referencias adicionales

