---
title: Administrar VPN en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Administrar VPN en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Conexiones de red privada virtual (VPN) permiten a los usuarios que trabajan en casa o en la carretera acceder a un servidor en una red privada mediante la infraestructura proporcionada por una red pública, como Internet. Para usar VPN para tener acceso a recursos del servidor, debes realizar lo siguiente:  
  
-   [Permitir VPN para el acceso remoto en el servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Establecer permisos de VPN para los usuarios de red](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Conectar los equipos cliente al servidor](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Usar VPN para conectarte a Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>Permitir VPN para el acceso remoto en el servidor  
 Completa el siguiente procedimiento para configurar VPN en Windows Server Essentials para habilitar el acceso remoto.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Para habilitar VPN en Windows Server Essentials  
  
1.  Abra el panel.  
  
2.  Haz clic en **configuración**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
3.  Haz clic en **configurar**. Aparece el desde cualquier lugar acceso asistente Configurar.  
  
4.  En la **características elige acceso desde cualquier lugar para habilitar** página, seleccione la **red privada Virtual** casilla de verificación.  
  
5.  Sigue las instrucciones para completar al asistente.  
  
##  <a name="BKMK_2"></a>Establecer permisos de VPN para los usuarios de red  
 Puedes usar VPN para conectarte a Windows Server Essentials y obtener acceso a todos los recursos que se almacenan en el servidor. Esto es especialmente útil si tienes un equipo cliente que está configurado con cuentas de red que pueden usarse para conectarse a un servidor de Windows Server Essentials hospedado a través de una conexión VPN. Todas las cuentas de usuario recién creado en el servidor de Windows Server Essentials hospedado deben usar VPN para iniciar sesión en el equipo cliente por primera vez.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Para establecer permisos de VPN para los usuarios de red  
  
1.  Abra el panel.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieras para conceder permisos para obtener acceso al escritorio de forma remota.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **propiedades**.  
  
5.  En la **< usuario Account\ > propiedades**, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
6.  En la **acceso desde cualquier lugar** ficha, para permitir que un usuario que se conecte al servidor mediante VPN, selecciona el **permitir red privada Virtual (VPN)** casilla de verificación.  
  
7.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_Connect"></a>Conectar los equipos cliente al servidor  
 Después de VPN está habilitada en un servidor que ejecuta Windows Server Essentials para el acceso remoto, puedes usar una conexión VPN para conectarse y tener acceso a todos los recursos que se almacenan en el servidor. Sin embargo, primero debes conectar el equipo al servidor. Cuando un equipo se conecta al servidor mediante conectar mi PC para el Asistente de servidor, una conexión de red VPN se genera automáticamente en el equipo cliente y puede usarse para acceder a recursos del servidor mientras trabajan en casa o en la carretera. Para obtener instrucciones paso a paso sobre cómo conectar el equipo al servidor, consulte [conectar equipos con el servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_3"></a>Usar VPN para conectarte a Windows Server Essentials  
 Si tienes un equipo cliente que está configurado con cuentas de red que pueden usarse para conectarse a un servidor hospedado que ejecutan Windows Server Essentials a través de una conexión VPN, todas las cuentas de usuario recién creado en el servidor hospedado debe usar VPN para iniciar sesión en el equipo cliente por primera vez. Completar el procedimiento siguiente desde el equipo cliente que está conectado al servidor.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Para usar una VPN para tener acceso remoto a los recursos del servidor  
  
1.  Presiona Ctrl + Alt + Supr en el equipo cliente.  
  
2.  Haz clic en **cambiar de usuario** en la pantalla de inicio de sesión.  
  
3.  Haz clic en el icono de inicio de sesión de red en la esquina inferior derecha de la pantalla.  
  
4.  Iniciar sesión en la red de Windows Server Essentials mediante su nombre de usuario de la red y la contraseña.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
