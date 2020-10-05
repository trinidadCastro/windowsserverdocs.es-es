---
title: Usar el comando verbose
description: Artículo de referencia para detallado, que muestra la salida detallada de un comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a5c05590bbbb3f1b185a64d6b0081a3d230d6b41
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730622"
---
# <a name="using-the-verbose-command"></a>Usar el comando verbose

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