---
title: Mover y cambiar el tamaño de la caché hospedada (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 276ab47bc6f4f906aaeafc7779c4e2afdb80b260
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947891"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Movimiento y cambio de tamaño de la caché hospedada \( opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para trasladar la caché hospedada a la unidad y carpeta que prefiera, y para especificar la cantidad de espacio en disco que el servidor de caché hospedada puede usar para la caché hospedada.

Este procedimiento es opcional. Si la ubicación de caché predeterminada \( % WINDIR% \\ ServiceProfiles \\ NetworkService \\ AppData \\ local \\ PeerDistPub \) and size (que es el 5% del espacio total del disco duro) es adecuado para la implementación, no es necesario cambiarla.

Para realizar este procedimiento debe ser miembro del grupo de administradores.

### <a name="to-move-and-resize-the-hosted-cache"></a>Para cambiar el tamaño de la caché hospedada

1. Abra Windows PowerShell con privilegios de administrador.

2. Escriba el siguiente comando para trasladar la caché hospedada a otra ubicación en el equipo local y, a continuación, presione Entrar.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como – path y – moveTo, por los valores adecuados para su implementación.

    ```
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ```

3.  Escriba el siguiente comando para cambiar el tamaño de la memoria caché hospedada, en concreto, la memoria caché del \- equipo local. Presione ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como \- porcentaje, por los valores adecuados para su implementación.

    ```
    Set-BCCache -Percentage 20
    ```

4.  Para comprobar la configuración del servidor de caché hospedada, escriba el siguiente comando y presione Entrar.

    ```
    Get-BCStatus
    ```

    Los resultados del comando muestran el estado de todos los aspectos de la instalación de BranchCache. A continuación se muestran algunos de los valores de BranchCache y el valor correcto para cada elemento:

    -   Caché de los | CacheFileDirectoryPath: muestra la ubicación del disco duro que coincide con el valor que proporcionó con el parámetro – moveTo del comando SetBCCache. Por ejemplo, si proporcionó el valor D: \\ Cache, ese valor se muestra en el resultado del comando.

    -   Caché de los | MaxCacheSizeAsPercentageOfDiskVolume: muestra el número que coincide con el valor que proporcionó con el parámetro – Percentage del comando SetBCCache. Por ejemplo, si proporcionó el valor 20, ese valor se muestra en el resultado del comando.

Para continuar con esta guía, consulte [hash y precargar contenido en el servidor de caché hospedada &#40;&#41;opcional ](7-Bc-Prehash-Preload.md).