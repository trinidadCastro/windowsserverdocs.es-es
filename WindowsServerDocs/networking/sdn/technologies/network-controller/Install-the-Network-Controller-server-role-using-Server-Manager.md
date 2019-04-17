---
title: Instalar el rol de servidor de controlador de red mediante el administrador del servidor
description: Este tema proporciona instrucciones sobre cómo instalar el rol de servidor de controlador de red mediante el administrador del servidor de Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Instalar el rol de servidor de controlador de red mediante el administrador del servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona instrucciones sobre cómo instalar el rol de servidor de controlador de red mediante el administrador del servidor.

>[!IMPORTANT]
>No se implementará el rol de servidor de controlador de red en hosts físicos. Para implementar el controlador de red, debes instalar el rol de servidor de controlador de red en una máquina virtual de Hyper-V \(VM\) que está instalada en un host de Hyper-V. Después de haber instalado el controlador de red en máquinas virtuales en tres distintos hosts Hyper\-V, debe habilitar los hosts Hyper\-V para Software definido redes \(SDN\) agregando los hosts en los controladores de red mediante el comando de Windows PowerShell **nueva NetworkControllerServer**. Al hacerlo, se habilita el equilibrado de carga de Software SDN a función. Para obtener más información, consulta [nueva NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Después de instalar el controlador de red, debes usar comandos de Windows PowerShell para la configuración de controlador de red adicional. Para obtener más información, consulta [implementar controladores de red mediante Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Para instalar el controlador de red  
  
1.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el Asistente para agregar Roles y características. Haz clic en **siguiente**.  
  
2.  En **seleccionar el tipo de instalación**, mantenga la configuración predeterminada y haz clic en **siguiente**.  
  
3.  En **seleccionar el servidor de destino**, elige dónde quieres instalar el controlador de red y, a continuación, haz clic en el servidor de **siguiente**.  
  
4.  En **seleccionar Roles de servidor**, en **Roles**, haz clic en **controlador de red**.  
  
    ![Rol de servidor de controlador de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  La **agregar características que son necesarias para el controlador de red** abre el cuadro de diálogo. Haz clic en **agregar características**.  
  
    ![Agregar características para el controlador de red](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  En **seleccionar Roles de servidor**, haz clic en **siguiente**.  
  
    ![Haz clic en siguiente](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  En **seleccionar características**, haz clic en **siguiente**.  
  
8.  En **controlador de red** haga clic en **siguiente**.  
  
9. En **Confirmar selecciones de instalación**, revisa las opciones seleccionadas. Instalación del controlador de red requiere que se reinicie el equipo después de que se ejecuta el asistente. Por este motivo, haz clic en **reiniciar automáticamente el servidor de destino en caso necesario**. La **agregar Roles and Features Wizard** abre el cuadro de diálogo. Haz clic en **Sí**.  
  
    ![Agregar Roles and Features Wizard](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. En **Confirmar selecciones de instalación**, haz clic en **instalar**.  
  
11. Instala el rol de servidor de controlador de red en el servidor de destino y, a continuación, reinicia el servidor.  
  
12. Una vez reiniciado el equipo, inicia sesión el equipo y ver el administrador del servidor para comprobar la instalación del controlador de red.  
  
    ![Administrador del servidor](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Controlador de red](Network-Controller.md)  
  


