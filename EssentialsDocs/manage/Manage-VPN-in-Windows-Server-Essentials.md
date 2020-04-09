---
title: Administrar VPN en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 38c0b0e7bfc29f0b7717cd18a103ae068bbb259b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852698"
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Administrar VPN en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Las conexiones de red privada virtual (VPN) permiten a los usuarios móviles o que trabajan en casa acceder a un servidor en una red privada usando la infraestructura proporcionada por una red pública, por ejemplo, Internet. Para usar una red VPN para acceder a los recursos de un servidor, debe realizar las siguientes acciones:  
  
-   [Habilitar VPN para el acceso remoto en el servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Establecer permisos de VPN para los usuarios de red](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Conectar equipos cliente al servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Usar VPN para conectarse a Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="enable-vpn-for-remote-access-on-the-server"></a><a name="BKMK_1"></a>Habilitar VPN para el acceso remoto en el servidor  
 Complete el procedimiento siguiente para configurar VPN en Windows Server Essentials para habilitar el acceso remoto.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Para habilitar VPN en Windows Server Essentials  
  
1.  Abra el Panel.  
  
2.  Haga clic en **Configuración** y, a continuación, en la pestaña **Acceso desde cualquier lugar**.  
  
3.  Haga clic en **Configurar**. Se muestra el asistente para configuración de Acceso desde cualquier lugar.  
  
4.  En la página **Elegir las características de Acceso desde cualquier lugar que desea habilitar**, active la casilla **Red privada virtual**.  
  
5.  Siga las instrucciones para completar el asistente.  
  
##  <a name="set-vpn-permissions-for-network-users"></a><a name="BKMK_2"></a>Establecer permisos de VPN para los usuarios de red  
 Puede conectarse a Windows Server Essentials mediante VPN y acceder a todos los recursos que se almacenan en el servidor. Esto resulta especialmente útil si un equipo cliente está configurado con las cuentas de red que pueden usarse para conectar con un servidor de Windows Server Essentials hospedado, a través de una conexión VPN. Todas las cuentas de usuario nuevas que se creen en el servidor de Windows Server Essentials hospedado deben usar VPN cuando inicien sesión en el equipo cliente por primera vez.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Para establecer los permisos de VPN de los usuarios de red  
  
1.  Abra el Panel.  
  
2.  En la barra de navegación, haga clic en **USUARIOS**.  
  
3.  En la lista de cuentas de usuario, seleccione aquella a la que desea conceder permisos de acceso remoto al escritorio.  
  
4.  En el panel tareas de la **cuenta de usuario de <\>** , haga clic en **propiedades**.  
  
5.  En la **< cuenta de usuario\> propiedades**, haga clic en la pestaña **acceso desde cualquier lugar** .  
  
6.  En la pestaña **Acceso desde cualquier lugar** , seleccione la casilla **Permitir red privada virtual (VPN)**  para permitir a un usuario conectarse al servidor mediante VPN.  
  
7.  Haga clic en **Aplicar** y en **Aceptar**.  
  
##  <a name="connect-client-computers-to-the-server"></a><a name="BKMK_Connect"></a>Conectar equipos cliente al servidor  
 Una vez habilitada la red VPN en un servidor que ejecuta Windows Server Essentials para el acceso remoto, puede usar una conexión VPN para conectarse y tener acceso a todos los recursos almacenados en el servidor. Sin embargo, primero debe conectar el equipo al servidor. Cuando se conecta un equipo al servidor mediante el asistente para conectar un equipo al servidor, se genera automáticamente una conexión de red VPN en el equipo cliente que se puede usar para acceder a los recursos del servidor mientras trabaja desde casa o se desplaza. Para obtener instrucciones paso a paso acerca de cómo conectar un equipo al servidor, consulte [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="use-vpn-to-connect-to-windows-server-essentials"></a><a name="BKMK_3"></a>Usar VPN para conectarse a Windows Server Essentials  
 Si tiene un equipo cliente configurado con cuentas de red que pueden usarse para conectarse a un servidor hospedado que ejecuta Windows Server Essentials a través de una conexión VPN, todas las cuentas de usuario recién creadas en el servidor hospedado deben usar VPN para iniciar sesión en el equipo cliente por primera vez. Complete el procedimiento siguiente en el equipo cliente que está conectado al servidor.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Usar VPN para acceder de forma remota a los recursos del servidor  
  
1.  Presione Ctrl + Alt + Supr en el equipo cliente.  
  
2.  Haga clic en **Cambiar usuario** en la pantalla de inicio de sesión.  
  
3.  Haga clic en el icono de inicio de sesión de red, en la esquina inferior derecha de la pantalla.  
  
4.  Inicie sesión en la red de Windows Server Essentials usando su nombre de usuario y su contraseña de red.  
  
## <a name="see-also"></a>Vea también  
  
-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
