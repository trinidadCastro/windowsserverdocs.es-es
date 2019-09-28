---
title: El comando verbose
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c7ecc2bb3578b578060694c95833fd32674db10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369915"
---
# <a name="the-verbose-command"></a>El comando verbose



Muestra la salida detallada de un comando especificado. Puede usar **/verbose** con cualquier otro comando WDSUtil que ejecute. Tenga en cuenta que debe especificar **/verbose** y **/Progress** directamente después de **WDSUtil**.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Ejemplos

Para eliminar los equipos aprobados de la base de datos de adición automática y mostrar los resultados detallados, escriba:
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```