---
title: progreso
description: Comandos de Windows tema de progreso, que muestra el progreso mientras se ejecuta un comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9957174203df8f2f5c02bf3ab8a4f3406701a8da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833128"
---
# <a name="progress"></a>progreso

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