---
title: Mover y cambiar el tamaño de la caché hospedada (opcional)
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cb75e06b5da8ff95fcf763b22c5160ea200035f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853536"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Mover y cambiar el tamaño de la memoria caché hospedada \(opcional\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para mover la caché hospedada a la unidad y carpeta que prefiera y para especificar la cantidad de espacio en disco que el servidor de caché hospedada puede usar para la caché hospedada.

Este procedimiento es opcional. Si el valor predeterminado de ubicación de caché \(% windir %\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) y tamaño, que es del 5% del disco duro total espacio: son adecuadas para la implementación, no es necesario cambiarlas.

Para realizar este procedimiento debe ser miembro del grupo de administradores.

### <a name="to-move-and-resize-the-hosted-cache"></a>Para mover y cambiar el tamaño de la memoria caché hospedada

1. Abra Windows PowerShell con privilegios de administrador.

2. Escriba el siguiente comando para mover la caché hospedada a otra ubicación en el equipo local y, a continuación, presione ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como – Path y – MoveTo, con los valores adecuados para su implementación.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Escriba el siguiente comando para cambiar el tamaño hospedado almacenar en caché: específicamente la datacache \- en el equipo local. Presione ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como \-porcentaje, con los valores adecuados para su implementación.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Para comprobar la configuración del servidor de caché hospedada, escriba el siguiente comando y presione ENTRAR.

    ``` 
    Get-BCStatus
    ``` 

    Los resultados del comando mostrarán estado de todos los aspectos de la instalación de BranchCache. Siguientes son algunas de la configuración de BranchCache y el valor correcto para cada elemento:

    -   DataCache | CacheFileDirectoryPath: Muestra la ubicación de disco duro que coincida con el valor proporcionado con el parámetro – MoveTo del comando SetBCCache. Por ejemplo, si se proporciona el valor D:\\datacache, que se muestra el valor en la salida del comando.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Muestra el número que coincida con el valor proporcionado con el parámetro de porcentaje del comando SetBCCache. Por ejemplo, si proporciona el valor de 20, ese valor se muestra en la salida del comando.

Para continuar con esta guía, consulte [Prehash y precarga de contenido en el servidor de caché hospedada &#40;opcional&#41;](7-Bc-Prehash-Preload.md).