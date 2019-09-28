---
title: Información general de implementación de modo de caché hospedada de BranchCache
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe55bc9971606559af652d592a91db7a89544a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356366"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Información general de implementación de modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar este tema para planear la implementación de BranchCache en modo caché hospedada.

>[!IMPORTANT]
>El servidor de caché hospedada debe ejecutar Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

Antes de implementar el servidor de caché hospedada, debe planear los siguientes elementos:

- [Planear la configuración básica del servidor](#bkmk_basic)

- [Planear el acceso a dominios](#bkmk_domain)

- [Planear la ubicación y el tamaño de la memoria caché hospedada](#bkmk_cachelocation)

- [Planear el recurso compartido en el que se van a copiar los paquetes del servidor de contenido](#bkmk_package)

- [Planear la creación de paquetes de datos y hash en los servidores de contenido](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planear la configuración básica del servidor
  
Si planea usar un servidor existente en la sucursal como su servidor de caché hospedada, no es necesario que realice este paso de planeación, ya que el equipo ya tiene el nombre y tiene una configuración de dirección IP.

Después de instalar Windows Server 2016 en el servidor de caché hospedada, debe cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.

>[!NOTE]
>En esta guía, el servidor de caché hospedada se denomina HCS1, pero debe usar un nombre de servidor que sea adecuado para su implementación.

## <a name="bkmk_domain"></a>Planear el acceso a dominios

Si planea usar un servidor existente en la sucursal como su servidor de caché hospedada, no es necesario que realice este paso de planeación, a menos que el equipo no esté unido al dominio.
  
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y la cuenta de usuario se debe crear en AD DS antes del intento de inicio de sesión. Además, debe unir el equipo al dominio con una cuenta que tenga la pertenencia al grupo adecuada.

## <a name="bkmk_cachelocation"></a>Planear la ubicación y el tamaño de la memoria caché hospedada

En HCS1, determine dónde desea ubicar la caché hospedada en el servidor de caché hospedada. Por ejemplo, decida el disco duro, el volumen y la ubicación de la carpeta donde planea almacenar la memoria caché.

Además, decida el porcentaje de espacio en disco que desea asignar a la caché hospedada.

## <a name="bkmk_package"></a>Planear el recurso compartido en el que se van a copiar los paquetes del servidor de contenido

Después de crear paquetes de datos en los servidores de contenido, debe copiarlos a través de la red en un recurso compartido en el servidor de caché hospedada.

Planee la ubicación de la carpeta y los permisos de uso compartido de la carpeta compartida. Además, si los servidores de contenido hospedan una gran cantidad de datos y los paquetes que crea serán archivos de gran tamaño, planee realizar la operación de copia fuera de las horas picos, de modo que la operación de copia no consuma el ancho de banda WAN durante una hora en la que otros usuarios deban usar  el ancho de banda para las operaciones empresariales normales.

## <a name="bkmk_prehash"></a>Planear la creación de paquetes de datos y hash en los servidores de contenido

Antes de aplicar el algoritmo hash en los servidores de contenido, debe identificar las carpetas y los archivos que contienen el contenido que desea agregar al paquete de datos. 

Además, debe planear la ubicación de la carpeta local donde puede almacenar los paquetes de datos antes de copiarlos en el servidor de caché hospedada.

Para continuar con esta guía, consulte [implementación del modo caché hospedada de BranchCache](4-Bc-Hcm-Deployment.md).
