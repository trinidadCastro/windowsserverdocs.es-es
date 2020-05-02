---
title: enumerar proveedores
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761099e3b399aeb9e6a3fe1ddd53ed1a667a4ccb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724502"
---
# <a name="list-providers"></a>enumerar proveedores



Enumera los proveedores de instantáneas que están registrados actualmente en el sistema.



## <a name="syntax"></a>Sintaxis

```
list providers
```

## <a name="examples"></a>Ejemplos

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