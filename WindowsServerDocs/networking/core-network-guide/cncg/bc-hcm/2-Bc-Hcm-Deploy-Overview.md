---
title: Información general de implementación de modo de caché hospedada de BranchCache
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 011c0e2f30dad914cc9fde4b2f81fdea2899bf11
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956042"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Información general de implementación de modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar esta guía para implementar un servidor de caché hospedada de BranchCache en una sucursal en la que los equipos están Unidos a un dominio. Puede usar este tema para obtener información general sobre el proceso de implementación del modo de caché hospedada de BranchCache.

En esta información general se incluye la infraestructura de BranchCache que necesita, así como una sencilla introducción paso a paso de la implementación.

## <a name="hosted-cache-server-deployment-infrastructure"></a><a name="bkmk_components"></a>Infraestructura de implementación de servidor de caché hospedada

En esta implementación, el servidor de caché hospedada se implementa mediante puntos de conexión de servicio en Active Directory Domain Services \( AD DS \) y tiene la opción con BranchCache en windows Server 2016, windows Server 2012 R2 y windows Server 2012, para aplicar previamente el algoritmo hash al contenido compartido en servidores web y de contenido basado en archivos y, a continuación, cargar previamente el contenido en servidores de caché hospedada.

En la ilustración siguiente se muestra la infraestructura necesaria para implementar un servidor de caché hospedada de BranchCache.

![Información general del modo caché hospedada de BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Aunque esta implementación describe los servidores de contenido en un centro de datos en la nube, puede usar esta guía para implementar un servidor de caché hospedada de BranchCache independientemente de dónde implemente los servidores de contenido: en la oficina central o en una ubicación en la nube.

### <a name="hcs1-in-the-branch-office"></a>HCS1 en la sucursal

Debe configurar este equipo como un servidor de caché hospedada. Si opta por prehacer hash de los datos del servidor de contenido para que pueda cargar previamente el contenido en los servidores de caché hospedada, puede importar los paquetes de datos que contienen el contenido de los servidores web y de archivos.

### <a name="web1-in-the-cloud-data-center"></a>WEB1 en el centro de datos en la nube

WEB1 es un \- servidor de contenido habilitado para BranchCache. Si opta por prehacer hash de los datos del servidor de contenido para que pueda cargar previamente el contenido en los servidores de caché hospedada, puede aplicar un algoritmo hash al contenido compartido en WEB1 y luego crear un paquete de datos que copie en HCS1.

### <a name="file1-in-the-cloud-data-center"></a>ARCHIVO1 en el centro de datos en la nube

ARCHIVO1 es un \- servidor de contenido habilitado para BranchCache. Si opta por prehacer hash de los datos del servidor de contenido para que pueda cargar previamente el contenido en los servidores de caché hospedada, puede aplicar un algoritmo hash al contenido compartido en ARCHIVO1 y, a continuación, crear un paquete de datos que copie en HCS1.

### <a name="dc1-in-the-main-office"></a>DC1 en la oficina central

DC1 es un controlador de dominio y debe configurar la Directiva de dominio predeterminada, u otra directiva más adecuada para su implementación, con la configuración de BranchCache directiva de grupo para habilitar la detección automática de caché hospedada por punto de conexión de servicio.

Cuando directiva de grupo actualizan los equipos cliente de la rama y se aplica esta configuración de Directiva, localizan y comienzan a usar automáticamente el servidor de caché hospedada en la sucursal.

### <a name="client-computers-in-the-branch-office"></a>Equipos cliente de la sucursal

Debe actualizar directiva de grupo en los equipos cliente para aplicar la nueva configuración de directiva de grupo de BranchCache y permitir que los clientes busquen y utilicen el servidor de caché hospedada.

## <a name="hosted-cache-server-deployment-process-overview"></a><a name="bkmk_overview"></a>Información general del proceso de implementación de servidor de caché hospedada

>[!NOTE]
>Los detalles de cómo realizar estos pasos se proporcionan en la sección [implementación de modo de caché hospedada de BranchCache](4-Bc-Hcm-Deployment.md).

El proceso de implementación de un servidor de caché hospedada de BranchCache se produce en estas fases:

>[!NOTE]
>Algunos de los pasos siguientes son opcionales, como los pasos que muestran cómo aplicar un algoritmo hash y precargar contenido en servidores de caché hospedada. Al implementar BranchCache en modo caché hospedada, no es necesario aplicar un algoritmo hash al contenido en los servidores web y de contenido de archivo, para crear un paquete de datos y para importar el paquete de datos con el fin de cargar previamente los servidores de caché hospedada en el contenido. Los pasos se indican como opcionales en esta sección y en la sección [implementación del modo caché hospedada de BranchCache](4-Bc-Hcm-Deployment.md) para que pueda omitirlos si lo prefiere.

1. En HCS1, use los comandos de Windows PowerShell para configurar el equipo como un servidor de caché hospedada y para registrar un punto de conexión de servicio en Active Directory.

2. \(Opcional \) en HCS1, si los valores predeterminados de BranchCache no coinciden con los objetivos de implementación para el servidor y la caché hospedada, configure la cantidad de espacio en disco que desea asignar a la caché hospedada. Configure también la ubicación del disco que prefiera para la caché hospedada.

3. \(\)Contenido hash opcional en los servidores de contenido, cree paquetes de datos y cargue previamente el contenido en el servidor de caché hospedada.

    > [!NOTE]
    > El hash y la carga previa del contenido en el servidor de caché hospedada es opcional; sin embargo, si elige el prehash y la precarga, debe realizar todos los pasos siguientes que se aplican a la implementación. \(Por ejemplo, si no tiene servidores Web, no es necesario realizar ninguno de los pasos relacionados con el hash y la carga previa del contenido del servidor Web.\)

    1. En WEB1, aplica un algoritmo hash al contenido del servidor Web y crea un paquete de datos.

    2. En ARCHIVO1, prehash del contenido del servidor de archivos y creación de un paquete de datos.

    3. Desde WEB1 y ARCHIVO1, copie los paquetes de datos en el servidor de caché hospedada HCS1.

    4. En HCS1, importe los paquetes de datos para cargar previamente la memoria caché de datos.

4. En DC1, configure los equipos cliente de la sucursal unida a un dominio para el modo caché hospedada mediante la configuración de directiva de grupo con la configuración de directivas de BranchCache.

5. En los equipos cliente, actualice directiva de grupo.

Para continuar con esta guía, consulte [planeamiento de la implementación del modo caché hospedada de BranchCache](3-Bc-Hcm-Plan.md).