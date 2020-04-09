---
title: retain
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 891192c0d6b1687cf6bffea0f57d1853e023b776
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835768"
---
# <a name="retain"></a>retain



Prepara un volumen dinámico simple existente para usarlo como volumen de arranque o del sistema.

## <a name="syntax"></a>Sintaxis

```
retain
```

## <a name="remarks"></a>Comentarios

-   En un disco dinámico de registro de arranque maestro (MBR), este comando crea una entrada de partición en el registro de arranque maestro.
-   En un disco dinámico de tabla de particiones GUID (GPT), este comando crea una entrada de partición en la tabla de particiones GUID.

## <a name="additional-references"></a>Referencias adicionales

