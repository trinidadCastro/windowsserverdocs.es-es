---
title: Información general de implementación de modo de caché hospedada de BranchCache
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 930a9b4872a7a79351055841a5d716dd99df0fa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829516"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Información general de implementación de modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar a esta guía para implementar un servidor de caché hospedada de BranchCache en una sucursal donde los equipos están unidos a un dominio. Puede utilizar este tema para obtener información general sobre el proceso de implementación de modo caché hospedada de BranchCache.

Esta información general incluye la infraestructura de BranchCache que necesita, así como una sencilla introducción paso a paso de implementación.

## <a name="bkmk_components"></a>Infraestructura de implementación de servidor de caché hospedada

En esta implementación, el servidor de caché hospedada se implementa mediante el uso de puntos de conexión de servicio en Active Directory Domain Services \(AD DS\), y tiene la opción con BranchCache en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, para hacer un hash previo del contenido compartido de Web y de archivos basado en servidores de contenido, a continuación, precargar el contenido en servidores de caché hospedada.

La siguiente ilustración muestra la infraestructura necesaria para implementar un servidor de caché hospedada de BranchCache.

![Información general del modo de caché hospedada de BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Aunque esta implementación muestra los servidores de contenido de un centro de datos en la nube, puede usar a esta guía para implementar un servidor de caché hospedada de BranchCache, independientemente de dónde implementar los servidores de contenido en la oficina central o en una ubicación en la nube.

### <a name="hcs1-in-the-branch-office"></a>HCS1 en la sucursal

Debe configurar este equipo como servidor de caché hospedada. Si decide hacer un hash previo datos del servidor de contenido para que pueda precargar el contenido en los servidores de caché hospedada, puede importar paquetes de datos que contienen el contenido de los servidores de archivos y Web.

### <a name="web1-in-the-cloud-data-center"></a>Web1 en el centro de datos en la nube

Web1 es un BranchCache\-habilitado el servidor de contenido. Si decide hacer un hash previo datos del servidor de contenido para que pueda precargar el contenido en los servidores de caché hospedada, puede hacer un hash previo del contenido compartido en WEB1, luego crear un paquete de datos que se copia en HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 en el centro de datos en la nube

File1 es un BranchCache\-habilitado el servidor de contenido. Si decide hacer un hash previo datos del servidor de contenido para que pueda precargar el contenido en los servidores de caché hospedada, puede hacer un hash previo del contenido compartido en FILE1 y luego crear un paquete de datos que se copia en HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 en la oficina central

DC1 es un controlador de dominio y debe configurar la directiva predeterminada de dominio o en otra directiva que sea más adecuado para la implementación, con la configuración de directiva de grupo para habilitar la detección de caché hospedada automática mediante el punto de conexión de servicio.

Cuando los equipos cliente de la sucursal tienen actualizar directiva de grupo y se aplica esta configuración de directiva, automáticamente buscar y comenzar a usar el servidor de caché hospedada en la sucursal.

### <a name="client-computers-in-the-branch-office"></a>Equipos cliente en la sucursal

Debe actualizar la directiva de grupo en los equipos cliente para aplicar la nueva configuración de directiva de grupo de BranchCache y permite a los clientes buscar y usar el servidor de caché hospedada.

## <a name="bkmk_overview"></a>Hospedado información general del proceso de implementación de servidor de caché

>[!NOTE]
>Se proporcionan los detalles de cómo realizar estos pasos en la sección [implementación en modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md).

Se produce el proceso de implementación de un servidor de caché hospedada de BranchCache en estas fases:

>[!NOTE]
>Algunos de los pasos siguientes son opcionales, como los pasos que se muestran cómo hacer un hash previo y precargar contenido en servidores de caché hospedada. Cuando se implementa BranchCache en modo caché hospedada, no es necesario prehash contenido en las Web y el archivo de servidores de contenido, para crear un paquete de datos e importar el paquete de datos para cargar previamente los servidores de caché hospedada con contenido. Los pasos se indican como opcionales en esta sección y en la sección [implementación en modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md) para que se puede omitir si lo prefiere.

1. En HCS1, usar comandos de Windows PowerShell para configurar el equipo como servidor de caché hospedada y para registrar un punto de conexión de servicio en Active Directory.

2. \(Opcional\) en HCS1, si los valores predeterminados de BranchCache no coinciden con los objetivos de implementación para el servidor y la memoria caché hospedada, configurar la cantidad de espacio en disco que desea asignar para la caché hospedada. Configure también la ubicación del disco que prefiera para la caché hospedada.

3. \(Opcional\) hacer un hash previo contenido en servidores de contenido, crear paquetes de datos y cargar previamente contenido en el servidor de caché hospedada.

    > [!NOTE]
    > Aplicación de hash previo y cargar previamente contenido en el servidor de caché hospedada es opcional, pero si decide hacer un hash previo y precarga, debe realizar todos los pasos siguientes son aplicable a su implementación. \(Por ejemplo, si no tiene servidores Web, es necesario realizar cualquiera de los pasos relacionados con la aplicación de hash previo y carga previa de contenido del servidor Web.\)

    1. En WEB1, hacer un hash previo contenido del servidor Web y crear un paquete de datos.

    2. En FILE1, hacer un hash previo contenido del servidor de archivos y crear un paquete de datos.

    3. En WEB1 y FILE1, copie los paquetes de datos en el servidor de caché hospedada HCS1.

    4. En HCS1, importe los paquetes de datos para cargar previamente la caché de datos.

4. En DC1, configurar la rama de equipos cliente Unidos al dominio office para el modo caché hospedada mediante la configuración de directiva de grupo con la configuración de directiva de BranchCache.

5. En los equipos cliente, actualice la directiva de grupo.

Para continuar con esta guía, consulte [BranchCache hospedado caché de modo de planeamiento de la implementación](3-Bc-Hcm-Plan.md).