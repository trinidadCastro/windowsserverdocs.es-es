---
title: Instalar un nuevo servidor de archivos como servidor de contenido
description: Obtenga información acerca de cómo instalar el rol de servidor servicios de archivo y el servicio de rol BranchCache para archivos de red en un equipo que ejecuta Windows Server 2016.
manager: brianlic
ms.topic: how-to
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 29687cd09795b2b2a3b65b6d659fc9d047c80cf0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949711"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>Instalar un nuevo servidor de archivos como servidor de contenido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar el rol de servidor servicios de archivo y el servicio **de rol BranchCache para archivos de red** en un equipo que ejecute Windows Server 2016.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.
>
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`
>
> `Restart-Computer`
>
> Para instalar el servicio de rol desduplicación de datos, escriba el siguiente comando y, a continuación, presione Entrar.
>
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`

### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>Para instalar los Servicios de archivo y el servicio de rol BranchCache para archivos de red

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características. En **Antes de comenzar**, haga clic en **Siguiente**.

2.  En **Seleccionar tipo de instalación**, asegúrese de que esté seleccionada la opción instalación basada en **características o en roles** y, a continuación, haga clic en **siguiente**.

3.  En **Seleccionar servidor de destino**, asegúrese de que está seleccionado el servidor correcto y, a continuación, haga clic en **siguiente**.

4.  En **Seleccionar roles de servidor**, en **roles**, tenga en cuenta que el rol **servicios de archivos y almacenamiento** ya está instalado. Haga clic en la flecha situada a la izquierda del nombre del rol para expandir la selección de servicios de función y, a continuación, haga clic en la flecha situada a la izquierda de **servicios de archivos e iSCSI**.

5.  Active las casillas de verificación de **servidor de archivos** y **BranchCache para archivos de red**.

    > [!TIP]
    > Se recomienda que active también la casilla de **desduplicación de datos**.

    Haga clic en **Next**.

6.  En **seleccionar características**, haga clic en **siguiente**.

7.  En **confirmar selecciones de instalación**, revise las selecciones y, a continuación, haga clic en **instalar**. El panel progreso de la **instalación** se muestra durante la instalación. Una vez finalizada la instalación, haga clic en **cerrar**.
