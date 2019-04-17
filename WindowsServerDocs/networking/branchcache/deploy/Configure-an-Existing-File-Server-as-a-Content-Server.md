---
title: Configurar un servidor de archivos existente como un servidor de contenido
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da4c38b6209dc10704aee8c79344ee2da98da272
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurar un servidor de archivos existente como un servidor de contenido

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para instalar la **BranchCache para archivos de red** servicio de rol del rol de servidor de servicios de archivos en un equipo que ejecute Windows Server 2016.  
  
> [!IMPORTANT]  
> Si ya no está instalado el rol de servidor de servicios de archivo, sigue este procedimiento. En su lugar, consulta [instalar un nuevo servidor de archivos como un servidor de contenido](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Para realizar este procedimiento mediante Windows PowerShell, ejecuta Windows PowerShell como administrador, escribe los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Para instalar el servicio de rol desduplicación de datos, escribe el siguiente comando y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Para instalar la BranchCache para el servicio de rol de archivos de red  
  
1.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el Asistente para agregar Roles y características. Haz clic en **siguiente**.  
  
2.  En **selecciona el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  En **servidor de destino selecciona**, asegúrate de que el servidor correcto está seleccionado y, a continuación, haz clic en **siguiente**.  
  
4.  En **seleccionar roles de servidor**, en **Roles**, ten en cuenta que la **servicios de almacenamiento de archivos y** ya está instalado el rol; Haz clic en la flecha situada a la izquierda del nombre de rol para ampliar la selección de servicios de rol y, a continuación, haz clic en la flecha situada a la izquierda de **iSCSI y archivo servicios**.  
  
5.  Selecciona la casilla de verificación **BranchCache para archivos de red**.  
  
    > [!TIP]  
    > Si aún no lo has hecho, se recomienda que tienes que seleccionar también la casilla de verificación **desduplicación datos**.  
  
    Haz clic en **siguiente**.  
  
6.  En **Select features**, haz clic en **siguiente**.  
  
7.  En **Confirmar selecciones de instalación**, revise las opciones seleccionadas y, a continuación, haz clic en **instalar**. La **progreso de la instalación** panel se muestre durante la instalación. Una vez finalizada la instalación, haz clic en **cerrar**.  
  


