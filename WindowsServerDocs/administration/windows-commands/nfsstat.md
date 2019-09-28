---
title: nfsstat
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373181"
---
# <a name="nfsstat"></a>nfsstat



Puede usar **nfsstat** para mostrar o restablecer el número de llamadas realizadas a servidor para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsstat [-z]
```

## <a name="description"></a>Descripción

Cuando se usa sin la opción **-z** , la utilidad de línea de comandos **nfsstat** muestra el número de llamadas NFS V2, NFS V3 y Mount V3 realizadas al servidor desde que los contadores se establecieron en 0, cuando se inició el servicio o cuando se restablecieron los contadores con  **nfsstat-z**.