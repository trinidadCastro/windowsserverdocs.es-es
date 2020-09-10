---
title: dfsdiag
description: Artículo de referencia para el comando dfsdiag, que proporciona información de diagnóstico para los espacios de nombres DFS.
ms.topic: reference
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05ca89a2ad2984ec7e47002489f26968e7d2d69b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635038"
---
# <a name="dfsdiag"></a>dfsdiag

Proporciona información de diagnóstico para los espacios de nombres DFS.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /testdcs [/domain:<domain name>]
dfsdiag /testsites </machine:<server name>| /DFSPath:<namespace root or DFS folder> [/recurse]> [/full]
dfsdiag /testdfsconfig /DFSRoot:<namespace>
dfsdiag /testdfsintegrity /DFSRoot:<DFS root path> [/recurse] [/full]
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [dfsdiag testdcs](dfsdiag-testdcs.md) | Comprueba la configuración del controlador de dominio. |
| [dfsdiag testsites](dfsdiag-testsites.md) | Comprueba las asociaciones de sitio. |
| [dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md) | Comprueba la configuración del espacio de nombres DFS. |
| [dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md) | Comprueba la integridad del espacio de nombres DFS. |
| [dfsdiag testreferral](dfsdiag-testreferral.md) | Comprueba las respuestas de referencia. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
