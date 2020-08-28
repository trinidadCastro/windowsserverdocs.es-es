---
title: Usar el comando verbose
description: Artículo de referencia para detallado, que muestra la salida detallada de un comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1432656a89188755d63df974fa2732702a1a1ae
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029993"
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