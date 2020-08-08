---
title: Instale el rol de servidor de la controladora de red mediante Administrador del servidor
description: En este tema se proporcionan instrucciones sobre cómo instalar el rol de servidor de la controladora de red mediante Administrador del servidor en Windows Server 2016.
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: c5736656fc61f303b360b33ca29de9319a299435
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969572"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instale el rol de servidor de la controladora de red mediante Administrador del servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporcionan instrucciones sobre cómo instalar el rol de servidor de la controladora de red mediante Administrador del servidor.

>[!IMPORTANT]
>No implemente el rol de servidor de la controladora de red en hosts físicos. Para implementar la controladora de red, debe instalar el rol de servidor de la controladora de red en una máquina virtual de Hyper-V \( \) que esté instalada en un host de Hyper-v. Una vez que haya instalado el controlador de red en las máquinas virtuales en tres hosts de Hyper- \- v diferentes, debe habilitar los \- hosts de Hyper v para las redes definidas por software de \( Sdn \) agregando los hosts a la controladora de red mediante el comando de Windows PowerShell **New-NetworkControllerServer**. Al hacerlo, habilita el Load Balancer de software de SDN para que funcione. Para obtener más información, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Después de instalar el controlador de red, debe usar comandos de Windows PowerShell para la configuración adicional de la controladora de red. Para obtener más información, consulte implementación de la [controladora de red con Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).

### <a name="to-install-network-controller"></a>Para instalar el controlador de red

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características. Haga clic en **Next**.

2.  En **Seleccionar tipo de instalación**, mantenga la configuración predeterminada y haga clic en **siguiente**.

3.  En **Seleccionar servidor de destino**, elija el servidor en el que desea instalar el controlador de red y, a continuación, haga clic en **siguiente**.

4.  En **Seleccionar roles de servidor**, en **roles**, haga clic en **controladora de red**.

    ![Función de servidor de controladora de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)

5.  Se abre el cuadro **de diálogo Agregar características necesarias para la controladora de red** . Haga clic en **Agregar características**.

    ![Agregar características para la controladora de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)

6.  En **Seleccionar roles de servidor**, haga clic en **siguiente**.

    ![Haga clic en Next (Siguiente).](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)

7.  En **seleccionar características**, haga clic en **siguiente**.

8.  En **controlador de red** , haga clic en **siguiente**.

9. En **confirmar selecciones de instalación**, revise las opciones. La instalación de la controladora de red requiere que reinicie el equipo después de que se ejecute el asistente. Debido a esto, haga clic en **reiniciar el servidor de destino automáticamente si es necesario**. Se abre el cuadro de diálogo **Asistente para agregar roles y características** . Haga clic en **Sí**.

    ![Asistente para agregar roles y características](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)

10. En **Confirmar selecciones de instalación**, haga clic en **Instalar**.

11. El rol de servidor de la controladora de red se instala en el servidor de destino y, a continuación, se reinicia el servidor.

12. Una vez reiniciado el equipo, inicie sesión en el equipo y Compruebe la instalación de la controladora de red. para ello, consulte Administrador del servidor.

    ![Administrador de servidores](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)

## <a name="see-also"></a>Consulte también
[Controladora de red](Network-Controller.md)



