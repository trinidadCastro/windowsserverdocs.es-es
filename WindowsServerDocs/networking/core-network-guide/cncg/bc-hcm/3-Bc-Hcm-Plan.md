---
title: BranchCache hospedado modo de caché de planeación de implementación
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e645dd96ec85e3a23df6717cfa43d7627cb938e7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>BranchCache hospedado modo de caché de planeación de implementación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar este tema para planear la implementación de BranchCache en modo de caché hospedada.

>[!IMPORTANT]
>El servidor de memoria caché hospedada debe estar ejecutando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

Antes de implementar el servidor de la memoria caché hospedada, debes planear los siguientes elementos:

- [Planear la configuración básica del servidor](#bkmk_basic)

- [Planear el acceso de dominio](#bkmk_domain)

- [Planear la ubicación y tamaño de la memoria caché hospedada](#bkmk_cachelocation)

- [Planear el recurso compartido al que los paquetes de servidor de contenido se copie](#bkmk_package)

- [Creación en servidores de contenido del paquete prehashing plan y datos](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planear la configuración básica del servidor
  
Si vas a usar un servidor existente de tu sucursal como el servidor de la memoria caché hospedada, no es necesario realizar este paso de planeación, dado que el equipo ya tiene el nombre y tiene una configuración de la dirección IP.

Después de instalar Windows Server 2016 en el servidor de la memoria caché hospedada, debes cambiar el nombre del equipo y asignar y configurar una dirección IP estática para el equipo local.

>[!NOTE]
>En esta guía, el servidor de la memoria caché hospedada se denomina HCS1, sin embargo, debes usar un nombre de servidor que sea apropiado para la implementación.

## <a name="bkmk_domain"></a>Planear el acceso de dominio

Si vas a usar un servidor existente de tu sucursal como el servidor de la memoria caché hospedada, no es necesario realizar este paso de planeación, a menos que el equipo no está actualmente unido al dominio.
  
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y la cuenta de usuario debe crearse en AD DS antes del intento de inicio de sesión. Además, debes unir el equipo al dominio con una cuenta que tenga la pertenencia al grupo apropiado.

## <a name="bkmk_cachelocation"></a>Planear la ubicación y tamaño de la memoria caché hospedada

En HCS1, determinar dónde en el servidor de la memoria caché hospedada que quieras buscar en la memoria caché hospedada. Por ejemplo, decidir el disco duro, el volumen y la ubicación de carpeta donde planea la memoria caché de la tienda.

Además, decide qué porcentaje de espacio en disco que quieres asignar la memoria caché hospedada.

## <a name="bkmk_package"></a>Planear el recurso compartido al que los paquetes de servidor de contenido se copie

Después de crear paquetes de datos en los servidores de contenido, debe copiarlos en la red a un recurso compartido en el servidor de la memoria caché hospedada.

Planear la ubicación de carpeta y permisos para la carpeta compartida de uso compartido. Además, si los servidores de contenido hospedar una gran cantidad de datos y los paquetes que crees estarán archivos grandes, planea realizar la operación de copia durante las horas pico: desactivar\ para que no se consume el ancho de banda WAN por la operación de copia durante un tiempo cuando otras personas necesitan usar el ancho de banda para las operaciones empresariales normales.

## <a name="bkmk_prehash"></a>Creación en servidores de contenido del paquete prehashing plan y datos

Antes de prehash contenido en los servidores de contenido, debe identificar las carpetas y archivos que contienen contenido que quieres agregar al paquete de datos. 

Además, debes planear en la ubicación de la carpeta local donde puedes almacenar los paquetes de datos antes de copiarlos al servidor de la memoria caché hospedada.

Para continuar con esta guía, consulte [implementación de modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md).
