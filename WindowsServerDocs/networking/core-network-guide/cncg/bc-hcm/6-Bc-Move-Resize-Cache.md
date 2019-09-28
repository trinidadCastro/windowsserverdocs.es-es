---
title: Mover y cambiar el tamaño de la caché hospedada (opcional)
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b0e3b6b490dead32071d99becccd9dca937f1f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406359"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Mueva y cambie el tamaño de la caché hospedada \(Optional @ no__t-1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este procedimiento para trasladar la caché hospedada a la unidad y carpeta que prefiera, y para especificar la cantidad de espacio en disco que el servidor de caché hospedada puede usar para la caché hospedada.

Este procedimiento es opcional. Si la ubicación de caché predeterminada \(% WINDIR% \\ServiceProfiles @ no__t-2NetworkService @ no__t-3AppData @ no__t-4Local @ no__t-5PeerDistPub @ no__t-6 y size (que es el 5% del espacio total en el disco duro), es adecuado para su implementación, no es necesario cambiarlos.

Para realizar este procedimiento debe ser miembro del grupo de administradores.

### <a name="to-move-and-resize-the-hosted-cache"></a>Para cambiar el tamaño de la caché hospedada

1. Abra Windows PowerShell con privilegios de administrador.

2. Escriba el siguiente comando para trasladar la caché hospedada a otra ubicación en el equipo local y, a continuación, presione Entrar.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como – path y – moveTo, por los valores adecuados para su implementación.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Escriba el siguiente comando para cambiar el tamaño de la memoria caché hospedada, específicamente la memoria caché \- en el equipo local. Presione ENTRAR.

    > [!IMPORTANT]
    > Antes de ejecutar el siguiente comando, reemplace los valores de parámetro, como \-Percentage, por los valores adecuados para su implementación.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Para comprobar la configuración del servidor de caché hospedada, escriba el siguiente comando y presione Entrar.

    ``` 
    Get-BCStatus
    ``` 

    Los resultados del comando muestran el estado de todos los aspectos de la instalación de BranchCache. A continuación se muestran algunos de los valores de BranchCache y el valor correcto para cada elemento:

    -   Caché de los | CacheFileDirectoryPath: Muestra la ubicación del disco duro que coincide con el valor que proporcionó con el parámetro – moveTo del comando SetBCCache. Por ejemplo, si proporcionó el valor D: \\datacache, ese valor se muestra en el resultado del comando.

    -   Caché de los | MaxCacheSizeAsPercentageOfDiskVolume: Muestra el número que coincide con el valor que proporcionó con el parámetro – Percentage del comando SetBCCache. Por ejemplo, si proporcionó el valor 20, ese valor se muestra en el resultado del comando.

Para continuar con esta guía, consulte [hash y precargar contenido en el servidor &#40;de caché&#41;hospedada opcional](7-Bc-Prehash-Preload.md).