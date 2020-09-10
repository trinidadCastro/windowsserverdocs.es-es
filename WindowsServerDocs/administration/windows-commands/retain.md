---
title: retain
description: Artículo de referencia para el comando retain, que prepara un volumen dinámico existente para su uso como volumen de sistema o de arranque.
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 201aa8b79f8ac0f89d44be7db3f84c9f3059e206
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626906"
---
# <a name="retain"></a>retain

Prepara un volumen dinámico simple existente para su uso como volumen de arranque o del sistema. Si utiliza un disco dinámico de registro de arranque maestro (MBR), este comando crea una entrada de partición en el registro de arranque maestro. Si utiliza un disco dinámico de tabla de particiones GUID (GPT), este comando crea una entrada de partición en la tabla de particiones GUID.

## <a name="syntax"></a>Syntax

```
retain
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
