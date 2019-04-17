---
title: Instalar a un servidor de archivos nuevos como un servidor de contenido
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 53937cfc139efc6df5facfa872e63609229a548c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-a-new-file-server-as-a-content-server"></a>Instalar a un servidor de archivos nuevos como un servidor de contenido

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para instalar el rol de servidor de servicios de archivo y la **BranchCache para archivos de red** servicio de rol en un equipo que ejecute Windows Server 2016.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Para realizar este procedimiento mediante Windows PowerShell, ejecuta Windows PowerShell como administrador, escribe los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> Para instalar el servicio de rol desduplicación de datos, escribe el siguiente comando y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>Para instalar servicios de archivo y la BranchCache para el servicio de rol de archivos de red  
  
1.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard. En **antes de comenzar**, haz clic en **siguiente**.  
  
2.  En **selecciona el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  En **servidor de destino selecciona**, asegúrate de que el servidor correcto está seleccionado y, a continuación, haz clic en **siguiente**.  
  
4.  En **seleccionar roles de servidor**, en **Roles**, ten en cuenta que la **servicios de almacenamiento de archivos y** ya está instalado el rol; Haz clic en la flecha situada a la izquierda del nombre de rol para ampliar la selección de servicios de rol y, a continuación, haz clic en la flecha situada a la izquierda de **iSCSI y archivo servicios**.  
  
5.  Selecciona las casillas de verificación para **servidor de archivos** y **BranchCache para archivos de red**.  
  
    > [!TIP]  
    > Se recomienda que tienes que seleccionar también la casilla de verificación **desduplicación datos**.
  
    Haz clic en **siguiente**.  
  
6.  En **Select features**, haz clic en **siguiente**.  
  
7.  En **Confirmar selecciones de instalación**, revise las opciones seleccionadas y, a continuación, haz clic en **instalar**. La **progreso de la instalación** panel se muestre durante la instalación. Una vez finalizada la instalación, haz clic en **cerrar**.
