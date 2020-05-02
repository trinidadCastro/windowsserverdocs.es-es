---
title: nfsstat
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e2c02fdfeb9923993a1d4471862a6c8487c9d86
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723753"
---
# <a name="nfsstat"></a>nfsstat



Puede usar **nfsstat** para mostrar o restablecer el número de llamadas realizadas a servidor para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsstat [-z]
```

## <a name="description"></a>Descripción

Cuando se usa sin la opción **-z** , la utilidad de línea de comandos **nfsstat** muestra el número de llamadas NFS V2, NFS V3 y Mount V3 realizadas al servidor desde que los contadores se establecieron en 0, cuando se inició el servicio o cuando se restablecieron los contadores con **nfsstat-z**.