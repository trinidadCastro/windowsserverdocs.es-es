---
title: Información general de implementación de modo de caché hospedada de BranchCache
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7232f8732e7476b955115741b5582a585dc6068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890686"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Información general de implementación de modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede utilizar este tema para planear la implementación de BranchCache en modo caché hospedada.

>[!IMPORTANT]
>El servidor de caché hospedada debe ejecutar Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

Antes de implementar el servidor de caché hospedada, debe planear los siguientes elementos:

- [Planear la configuración básica del servidor](#bkmk_basic)

- [Planeación de acceso de dominio](#bkmk_domain)

- [Planear la ubicación y tamaño de la memoria caché hospedada](#bkmk_cachelocation)

- [Planear el recurso compartido a la que los paquetes del servidor de contenido se van a copiarse](#bkmk_package)

- [Plan de hash previos y los datos de la creación en servidores de contenido del paquete](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planear la configuración básica del servidor
  
Si planea utilizar un servidor existente en la sucursal como el servidor de caché hospedada, no es necesario realizar este paso de planeación, porque el equipo ya tiene el nombre y tiene una configuración de direcciones IP.

Después de instalar Windows Server 2016 en el servidor de caché hospedada, debe cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.

>[!NOTE]
>En esta guía, el servidor de caché hospedada se denomina HCS1, sin embargo, debe usar un nombre de servidor que sea adecuado para su implementación.

## <a name="bkmk_domain"></a>Planeación de acceso de dominio

Si planea utilizar un servidor existente en la sucursal como el servidor de caché hospedada, no es necesario realizar este paso de planeación, a menos que el equipo actualmente no está unido al dominio.
  
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y se debe crear la cuenta de usuario en AD DS antes del intento de inicio de sesión. Además, debe unir el equipo al dominio con una cuenta que tenga la pertenencia al grupo adecuado.

## <a name="bkmk_cachelocation"></a>Planear la ubicación y tamaño de la memoria caché hospedada

En HCS1, determinar donde en el servidor de caché hospedada que desea colocar la memoria caché hospedada. Por ejemplo, decidir el disco duro, el volumen y la ubicación de la carpeta donde va a almacenar la memoria caché.

Asimismo, decida qué porcentaje de espacio en disco desea asignar para la caché hospedada.

## <a name="bkmk_package"></a>Planear el recurso compartido a la que los paquetes del servidor de contenido se van a copiarse

Después de crear paquetes de datos en los servidores de contenido, debe copiarlos a través de la red a un recurso compartido en el servidor de caché hospedada.

Planear la ubicación de carpeta y permisos para la carpeta compartida de uso compartido. Además, si sus servidores de contenido hospedan una gran cantidad de datos y los paquetes que cree será archivos de gran tamaño, planea realizar la operación de copia durante las horas punta: desactivar\ para que la operación de copia durante un tiempo cuando otros usuarios necesitan utilizar no consume ancho de banda WAN  el ancho de banda para las operaciones empresariales habituales.

## <a name="bkmk_prehash"></a>Plan de hash previos y los datos de la creación en servidores de contenido del paquete

Antes de hacer un hash previo contenido en los servidores de contenido, debe identificar las carpetas y archivos que contienen el contenido que desea agregar al paquete de datos. 

Además, debe planear la ubicación de carpeta local donde se pueden almacenar los paquetes de datos antes de copiarlos en el servidor de caché hospedada.

Para continuar con esta guía, consulte [implementación en modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md).
