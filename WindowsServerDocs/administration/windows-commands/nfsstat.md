---
title: nfsstat
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94eb389fdd694c08dcd1d77325f145a81e59ea0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838888"
---
# <a name="nfsstat"></a>nfsstat



Puede usar **nfsstat** para mostrar o restablecer el número de llamadas realizadas a servidor para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsstat [-z]
```

## <a name="description"></a>Descripción

Cuando se usa sin la opción **-z** , la utilidad de línea de comandos **nfsstat** muestra el número de llamadas NFS V2, NFS V3 y Mount V3 realizadas al servidor desde que los contadores se establecieron en 0, cuando se inició el servicio o cuando se restablecieron los contadores con **nfsstat-z**.