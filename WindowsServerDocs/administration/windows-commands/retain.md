---
title: retain
description: Artículo de referencia para el comando retain, que prepara un volumen dinámico existente para su uso como volumen de sistema o de arranque.
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98c27c62ab7e0ac3320986dde6049be40d10db57
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036994"
---
# <a name="retain"></a>retain

Prepara un volumen dinámico simple existente para su uso como volumen de arranque o del sistema. Si utiliza un disco dinámico de registro de arranque maestro (MBR), este comando crea una entrada de partición en el registro de arranque maestro. Si utiliza un disco dinámico de tabla de particiones GUID (GPT), este comando crea una entrada de partición en la tabla de particiones GUID.

## <a name="syntax"></a>Sintaxis

```
retain
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
