---
title: Usar el comando Progress
description: Artículo de referencia para el progreso, que muestra el progreso mientras se ejecuta un comando.
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a970e5f547bd4b4f64dbae757ae7b61ca9aaf2b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881512"
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