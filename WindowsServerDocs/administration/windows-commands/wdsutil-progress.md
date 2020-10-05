---
title: progreso de WDSUtil
description: Artículo de referencia para el progreso de WDSUtil, que muestra el progreso mientras se ejecuta un comando.
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a7ddc18db35b110c8b5c513f798e3408aafd93f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730694"
---
# <a name="wdsutil-progress"></a>WDSUtil/Progress

Muestra el progreso mientras se ejecuta un comando. Puede usar **/Progress** con cualquier otro comando WDSUtil que ejecute. Si desea activar el registro detallado para este comando, debe especificar **/verbose** y **/Progress** directamente después de **WDSUtil**.

## <a name="syntax"></a>Sintaxis

```
wdsutil /progress <commands>
```

## <a name="examples"></a>Ejemplos

Para inicializar el servidor y mostrar el progreso, escriba:

```
wdsutil /verbose /progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)