---
title: El comando detallado
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6a655ccdbd95b2f3523babecaa713ccdf99f9ec7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827246"
---
# <a name="the-verbose-command"></a>El comando detallado



Muestra resultados detallados para un comando especificado. Puede usar **/ verbose** con otros comandos WDSUTIL que se ejecutan. Tenga en cuenta que debe especificar **/ verbose** y **/progreso** directamente después **WDSUTIL**.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Ejemplos

Para eliminar los equipos aprobados desde la base de datos de adición automática y mostrar salida detallada, escriba:
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```