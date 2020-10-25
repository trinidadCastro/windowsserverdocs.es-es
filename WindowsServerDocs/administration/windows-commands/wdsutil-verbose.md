---
title: Usar el comando verbose
description: Artículo de referencia para detallado, que muestra la salida detallada de un comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2335ef8bf3e3b231851d424f99f0a7e878218c4b
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524880"
---
# <a name="using-the-verbose-command"></a>Usar el comando verbose

Muestra la salida detallada de un comando especificado. Puede usar **/verbose** con cualquier otro comando WDSUtil que ejecute. Tenga en cuenta que debe especificar **/verbose** y **/Progress** directamente después de **WDSUtil**.

## <a name="syntax"></a>Sintaxis

```
wdsutil /verbose <commands>
```

## <a name="examples"></a>Ejemplos

Para eliminar los equipos aprobados de la base de datos de adición automática y mostrar los resultados detallados, escriba:

```
wdsutil /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```