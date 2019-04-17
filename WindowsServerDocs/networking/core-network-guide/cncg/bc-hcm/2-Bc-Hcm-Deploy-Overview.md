---
title: BranchCache hospedado caché Introducción al modo implementación
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: feab3156b89637f64d1af0250df459533b1662c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>BranchCache hospedado caché Introducción al modo implementación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar a esta guía para implementar un servidor de caché BranchCache hospedado en una sucursal donde los equipos están unidos a un dominio. Puedes usar este tema para obtener una visión general del proceso de implementación de modo de caché hospedada BranchCache.

Esta introducción incluye la infraestructura de BranchCache que necesitas, así como una sencilla introducción paso a paso de implementación.

## <a name="bkmk_components"></a>Infraestructura de implementación de servidor de la memoria caché hospedada

En esta implementación, el servidor de la memoria caché hospedada se implementa mediante el uso de los puntos de conexión de servicio en los servicios de dominio de Active Directory \(AD DS\), tienes la opción con BranchCache en Windows Server 2016, Windows Server 2012 R2, y contenidos servidores basados en Windows Server 2012, para prehash el contenido compartido en la Web y el archivo y luego cargar previamente el contenido en los servidores de la memoria caché hospedada.

La siguiente ilustración muestra la infraestructura que es necesaria para implementar un servidor de caché BranchCache hospedado.

![Introducción al modo de caché hospedada BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Aunque esta implementación muestra los servidores de contenido en un centro de datos de la nube, puedes usar a esta guía para implementar un servidor de caché BranchCache hospedado independientemente en los servidores de contenido que se implementarán en la oficina principal o en una ubicación de nube.

### <a name="hcs1-in-the-branch-office"></a>HCS1 en la oficina de rama

Este equipo debe configurar como un servidor de la memoria caché hospedada. Si decides prehash datos del servidor de contenido, de modo que puedes cargar previamente el contenido en los servidores de la memoria caché hospedada, puede importar paquetes de datos que contienen el contenido de los servidores Web y el archivo.

### <a name="web1-in-the-cloud-data-center"></a>Web1 en el centro de datos de la nube

Web1 es un servidor de contenido BranchCache\ habilitado. Si eliges prehash datos del servidor de contenido, de modo que puedes cargar previamente el contenido en los servidores de la memoria caché hospedada, puede prehash el contenido compartido en WEB1 y luego crear un paquete de datos que Copies en HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 en el centro de datos de la nube

Archivo1 es un servidor de contenido BranchCache\ habilitado. Si eliges prehash datos del servidor de contenido, de modo que puedes cargar previamente el contenido en los servidores de la memoria caché hospedada, puede prehash el contenido compartido en archivo1 y luego crear un paquete de datos que Copies en HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 en la oficina principal

DC1 es un controlador de dominio, y debes configurar la directiva de dominio predeterminada, u otra directiva que sea más adecuado para la implementación, con la configuración de directiva de grupo BranchCache para habilitar la detección de memoria caché hospedada automática al punto de conexión de servicio.

Cuando los equipos cliente en la rama tienen la directiva de grupo actualizada y se aplica esta configuración de directiva, automáticamente busca y empezar a usar el servidor de la memoria caché hospedada en las sucursales.

### <a name="client-computers-in-the-branch-office"></a>Equipos cliente en la oficina de rama

Debes actualizar la directiva de grupo en los equipos cliente para aplicar la configuración de directiva de grupo BranchCache nuevo y para permitir que los clientes buscar y usar el servidor de la memoria caché hospedada.

## <a name="bkmk_overview"></a>Introducción al proceso de implementación servidor de la memoria caché hospedada

>[!NOTE]
>Se proporcionan los detalles de cómo realizar estos pasos en la sección [implementación de modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md).

El proceso de implementación de un servidor de caché hospedada BranchCache se produce en estas fases:

>[!NOTE]
>Algunos de los pasos siguientes son opcionales, como los pasos que muestran cómo prehash y precargar contenido en los servidores de la memoria caché hospedada. Cuando implementas BranchCache en modo de caché hospedada, no es necesario prehash contenido en las Web y el archivo servidores de contenido, para crear un paquete de datos y para importar el paquete de datos para precargar los servidores de la memoria caché hospedada con el contenido. Los pasos se indican como opcionales en esta sección y en la sección [implementación de modo de caché hospedada BranchCache](4-Bc-Hcm-Deployment.md) para que se puede omitir si lo prefieres.

1. En HCS1, usa comandos de Windows PowerShell para configurar el equipo como un servidor de la memoria caché hospedada y para registrar un punto de conexión de servicio en Active Directory.

2. \(Optional\) en HCS1, si los valores predeterminados de BranchCache no coinciden con tus objetivos de implementación para el servidor y la memoria caché hospedada, configurar la cantidad de espacio en disco que quieres asignar la memoria caché hospedada. Configurar la ubicación de disco que prefieras para la memoria caché hospedada.

3. \(Optional\) contenido prehash en los servidores de contenido, crear paquetes de datos y contenido precarga en el servidor de la memoria caché hospedada.

    > [!NOTE]
    > Precarga y prehashing contenido en el servidor de la memoria caché hospedada es opcional, pero si decides prehash y precarga, debes realizar todos los pasos que son aplicable a la implementación. \ (Por ejemplo, si no tienes servidores Web, no necesitas realizar cualquiera de los pasos para prehashing y carga previa de contenido del servidor Web. \)

    1. En WEB1, prehash contenido del servidor Web y crea un paquete de datos.

    2. En archivo1, prehash el contenido del servidor de archivos y crea un paquete de datos.

    3. Desde WEB1 y archivo1, copiar los paquetes de datos en el servidor de la memoria caché hospedada HCS1.

    4. En HCS1, importa los paquetes de datos para precargar la caché de datos.

4. En DC1, configurar los equipos de cliente de office de rama de unido a un dominio para el modo de la memoria caché hospedada mediante la configuración de directiva de grupo con la configuración de directiva de BranchCache.

5. En los equipos cliente, actualizar la directiva de grupo.

Para continuar con esta guía, consulte [BranchCache hospedado caché de modo de planeación de implementación](3-Bc-Hcm-Plan.md).