---
title: Usar el comando Progress
description: Artículo de referencia para el progreso, que muestra el progreso mientras se ejecuta un comando.
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33e0494133523748b599ab9e3673d4f786e044df
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038313"
---
# <a name="using-the-progress-command"></a>Usar el comando Progress

Muestra el progreso mientras se ejecuta un comando. Puede usar **/Progress** con cualquier otro comando WDSUtil que ejecute. Tenga en cuenta que debe especificar **/verbose** y **/Progress** directamente después de **WDSUtil**.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Ejemplos

Para inicializar el servidor y mostrar el progreso, escriba:
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```