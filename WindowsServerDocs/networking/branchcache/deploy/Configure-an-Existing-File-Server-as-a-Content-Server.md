---
title: Configurar un servidor de archivos existente como servidor de contenido
description: Obtenga información acerca de cómo instalar el servicio de rol BranchCache para archivos de red del rol de servidor servicios de archivo en un equipo que ejecuta Windows Server 2016.
manager: brianlic
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 83223ab3f565b3b00fc77accf0da67e82ecce630
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904740"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurar un servidor de archivos existente como servidor de contenido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar el servicio de rol **BranchCache para archivos de red** del rol de servidor servicios de archivo en un equipo que ejecuta Windows Server 2016.

> [!IMPORTANT]
> Si el rol de servidor Servicios de archivo no está instalado todavía, no siga este procedimiento. En su lugar, vea [instalar un nuevo servidor de archivos como servidor de contenido](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.
>
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`
>
> Para instalar el servicio de rol desduplicación de datos, escriba el siguiente comando y, a continuación, presione Entrar.
>
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`

### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Para instalar el servicio de rol BranchCache para archivos de red

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características. Haga clic en **Next**.

2.  En **Seleccionar tipo de instalación**, asegúrese de que esté seleccionada la opción instalación basada en **características o en roles** y, a continuación, haga clic en **siguiente**.

3.  En **Seleccionar servidor de destino**, asegúrese de que está seleccionado el servidor correcto y, a continuación, haga clic en **siguiente**.

4.  En **Seleccionar roles de servidor**, en **roles**, tenga en cuenta que el rol **servicios de archivos y almacenamiento** ya está instalado. Haga clic en la flecha situada a la izquierda del nombre del rol para expandir la selección de servicios de función y, a continuación, haga clic en la flecha situada a la izquierda de **servicios de archivos e iSCSI**.

5.  Active la casilla de verificación de **BranchCache para archivos de red**.

    > [!TIP]
    > Si todavía no lo ha hecho, se recomienda que active también la casilla de **desduplicación de datos**.

    Haga clic en **Next**.

6.  En **seleccionar características**, haga clic en **siguiente**.

7.  En **confirmar selecciones de instalación**, revise las selecciones y, a continuación, haga clic en **instalar**. El panel progreso de la **instalación** se muestra durante la instalación. Una vez finalizada la instalación, haga clic en **cerrar**.



