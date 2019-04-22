---
title: Instalar un nuevo servidor de archivos como servidor de contenido
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e3a4dbe5339685b385b0157756379e9e545f1964
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812026"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>Instalar un nuevo servidor de archivos como servidor de contenido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar el rol de servidor Servicios de archivo y la **BranchCache para archivos de red** servicio de rol en un equipo que ejecuta Windows Server 2016.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
> [!NOTE]  
> Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> Para instalar el servicio de rol desduplicación de datos, escriba el siguiente comando y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>Para instalar los Servicios de archivo y el servicio de rol BranchCache para archivos de red  
  
1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características. En **Antes de comenzar**, haga clic en **Siguiente**.  
  
2.  En **Seleccionar tipo de instalación**, asegúrese de que **instalación basada en roles o basada en características** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
3.  En **Seleccionar servidor de destino**, asegúrese de que el servidor correcto está seleccionado y, a continuación, haga clic en **siguiente**.  
  
4.  En **seleccionar roles de servidor**, en **Roles**, tenga en cuenta que el **servicios de archivos y almacenamiento** ya está instalado el rol; haga clic en la flecha situada a la izquierda del nombre de rol para expandir el selección de servicios de rol y, a continuación, haga clic en la flecha situada a la izquierda del **iSCSI y archivo servicios**.  
  
5.  Seleccione las casillas de verificación para **servidor de archivos** y **BranchCache para archivos de red**.  
  
    > [!TIP]  
    > Se recomienda que seleccione también la casilla de verificación **desduplicación de datos**.
  
    Haz clic en **Siguiente**.  
  
6.  En **seleccionar características**, haga clic en **siguiente**.  
  
7.  En **Confirmar selecciones de instalación**, revise las selecciones y, a continuación, haga clic en **instalar**. El **progreso de la instalación** panel se muestra durante la instalación. Cuando se complete la instalación, haga clic en **cerrar**.
