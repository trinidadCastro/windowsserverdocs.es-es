---
title: Instalar el rol de servidor de controlador de red mediante el administrador del servidor
description: Este tema proporciona instrucciones sobre cómo instalar el rol de servidor de controladora de red mediante el administrador del servidor en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859066"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar el rol de servidor de controlador de red mediante el administrador del servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema proporciona instrucciones sobre cómo instalar el rol de servidor de controladora de red mediante el administrador del servidor.

>[!IMPORTANT]
>No implemente el rol de servidor de controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de controladora de red en una máquina virtual de Hyper-V \(VM\) que está instalado en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en Hyper diferentes tres\-hosts V, debe habilitar Hyper\-hosts V para redes definidas por Software \(SDN\) mediante la adición de los hosts al uso de la controladora de red el comando de Windows PowerShell **New NetworkControllerServer**. Al hacerlo, permite que el equilibrador de carga de Software de SDN a función. Para obtener más información, consulte [New NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Después de instalar el controlador de red, debe usar los comandos de Windows PowerShell para la configuración de controladora de red adicional. Para obtener más información, consulte [implementar controladora de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Para instalar el controlador de red  
  
1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar Roles y características. Haz clic en **Siguiente**.  
  
2.  En **Seleccionar tipo de instalación**, mantenga la configuración predeterminada y haga clic en **siguiente**.  
  
3.  En **Seleccionar servidor de destino**, elija el servidor donde desea instalar el controlador de red y, a continuación, haga clic en **siguiente**.  
  
4.  En **seleccionar Roles de servidor**, en **Roles**, haga clic en **controladora de red**.  
  
    ![Rol de servidor de controlador de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  El **agregar características requeridas para la controladora de red** abre el cuadro de diálogo. Haga clic en **agregar características**.  
  
    ![Agregar características de controladora de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  En **seleccionar Roles de servidor**, haga clic en **siguiente**.  
  
    ![Haga clic en siguiente](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  En **seleccionar características**, haga clic en **siguiente**.  
  
8.  En **controladora de red** haga clic en **siguiente**.  
  
9. En **Confirmar selecciones de instalación**, revise las selecciones realizadas. Instalación de controladora de red requiere reiniciar el equipo después de que se ejecuta el asistente. Por este motivo, haga clic en **reiniciar automáticamente el servidor de destino si es necesario**. El **agregar Roles y características Asistente** abre el cuadro de diálogo. Haga clic en **Sí**.  
  
    ![Asistente para agregar roles y características](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. En **Confirmar selecciones de instalación**, haga clic en **Instalar**.  
  
11. Instala el rol de servidor de controladora de red en el servidor de destino y, a continuación, se reinicia el servidor.  
  
12. Una vez reiniciado el equipo, inicie sesión en el equipo y ver el administrador del servidor para comprobar la instalación de controladora de red.  
  
    ![Administrador de servidores](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Vea también  
[Controladora de red](Network-Controller.md)  
  


