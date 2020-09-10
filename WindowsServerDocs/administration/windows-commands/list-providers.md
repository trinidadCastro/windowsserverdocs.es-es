---
title: list providers
description: Artículo de referencia para el comando enumerar proveedores, en el que se enumeran los proveedores de instantáneas que están registrados actualmente en el sistema.
ms.topic: reference
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9c6f62c852ea41a9cc355afcaaaf083e68ad1d7a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639570"
---
# <a name="list-providers"></a>list providers

Enumera los proveedores de instantáneas que están registrados actualmente en el sistema.

## <a name="syntax"></a>Sintaxis

```
list providers
```

### <a name="examples"></a>Ejemplos

Para enumerar los proveedores de instantáneas registrados actualmente, escriba:

```
list providers
```

El resultado es similar al que se muestra a continuación:

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)