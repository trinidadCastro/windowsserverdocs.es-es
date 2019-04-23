---
title: nfsstat
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853296"
---
# <a name="nfsstat"></a>nfsstat



Puede usar **nfsstat** para mostrar o restablecer recuentos de llamadas realizadas al servidor para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsstat [-z]
```

## <a name="description"></a>Descripción

Cuando se usa sin la **- z** opción, el **nfsstat** utilidad de línea de comandos muestra el número de NFS V2, NFS V3 y llamadas de montaje V3 realizadas en el servidor desde los contadores se establecen en 0, cuando el servicio inicia o cuando se restablecen los contadores mediante **nfsstat - z**.