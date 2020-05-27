---
title: enumerar proveedores
description: Tema de referencia del comando list Providers, que muestra los proveedores de instantáneas que están registrados actualmente en el sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98615dfa92c24b91babb55ae3545065834887e5d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817245"
---
# <a name="list-providers"></a>enumerar proveedores

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