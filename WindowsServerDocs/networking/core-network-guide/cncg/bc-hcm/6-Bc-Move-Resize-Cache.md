---
title: Mover y cambiar el tamaño de la memoria caché hospedada (opcional)
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a3ed1d6de1281575b33cdf75a5970d2ed033085
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Mover y cambiar el tamaño de la memoria caché hospedada de \(Optional\)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar este procedimiento para mover la memoria caché hospedada en la unidad y la carpeta que prefieras, así como para especificar la cantidad de espacio en disco que el servidor de la memoria caché hospedada puede usar para la memoria caché hospedada.

Este procedimiento es opcional. Si la predeterminada caché ubicación \(%windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) y el tamaño, que es el 5% del espacio total del disco duro, son adecuados para la implementación, no es necesario cambiarlos.

Debe ser miembro del grupo Administradores para realizar este procedimiento.

### <a name="to-move-and-resize-the-hosted-cache"></a>Para mover y cambiar el tamaño de la memoria caché hospedada

1. Abre Windows PowerShell con privilegios de administrador.

2. Escribe el siguiente comando para mover la memoria caché hospedada en otra ubicación en el equipo local y, a continuación, presione ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el comando siguiente, reemplaza los valores de parámetro, como: ruta de acceso y – MoveTo, con los valores que son adecuados para la implementación.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Escribe el siguiente comando para cambiar el tamaño hospedado en caché: específicamente la datacache \: en el equipo local. Presiona ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el comando siguiente, reemplaza los valores de parámetro, como \-Percentage, con los valores que son adecuados para la implementación.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Para comprobar la configuración del servidor de memoria caché hospedada, escribe el siguiente comando y presione ENTRAR.

    ``` 
    Get-BCStatus
    ``` 

    Los resultados del comando mostrarán estado de todos los aspectos de la instalación BranchCache. Siguientes son algunas de las opciones de configuración BranchCache y el valor correcto para cada elemento:

    -   DataCache | CacheFileDirectoryPath: Muestra la ubicación de disco duro que coincida con el valor proporcionado con el parámetro: MoveTo del comando SetBCCache. Por ejemplo, si se proporciona el valor D:\\datacache, ese valor se muestra en el resultado del comando.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Muestra el número que coincida con el valor proporcionado con el parámetro – porcentaje del comando SetBCCache. Por ejemplo, si se proporciona el valor de 20, ese valor se muestra en el resultado del comando.

Para continuar con esta guía, consulte [Prehash y precarga contenido en el servidor de la memoria caché hospedada & #40; opcional & #41;](7-Bc-Prehash-Preload.md).