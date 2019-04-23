---
title: PASO 2 instalar y configurar ENRUTADOR1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a771b5eb8587d23bc67a7e7769264251afdb5bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838516"
---
# <a name="step-2-install-and-configure-router1"></a>PASO 2 instalar y configurar ENRUTADOR1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta guía de laboratorio de pruebas de multisitio, el equipo de enrutador proporciona un puente entre las subredes de la red corporativa y 2 de la red corporativa de IPv4 e IPv6 y actúa como un enrutador para el tráfico Teredo e IP-HTTPS.  
  
- Instalar el sistema operativo en ENRUTADOR1 
  
- Configure las propiedades de TCP/IP y el nombre del equipo  
  
- Desactivar el firewall
  
- Configurar el reenvío y enrutamiento
  
## <a name="install-the-operating-system-on-router1"></a>Instalar el sistema operativo en ENRUTADOR1  
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Para instalar el sistema operativo en ENRUTADOR1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa).  
  
2.  Siga las instrucciones para completar la instalación, especificando una contraseña segura para la cuenta Administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  Conecte ENRUTADOR1 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  Conectar ENRUTADOR1 a las subredes de la red corporativa y 2 de la red corporativa.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configure las propiedades de TCP/IP y el nombre del equipo  
Configuración de TCP/IP en el enrutador y el nombre del equipo a ENRUTADOR1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Para configurar las propiedades de TCP/IP y el nombre del equipo  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En el **las conexiones de red** ventana, haga clic en el adaptador de red que está conectado a la red corporativa, haga clic en **cambiar el nombre de**, tipo **Corpnet**, y presione ENTRAR.  
  
3.  Haga clic en **Corpnet**y, a continuación, haga clic en **propiedades**.  
  
4.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
5.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.0.0.254**. En **máscara de subred**, tipo **255.255.255.0**y, a continuación, haga clic en **Aceptar**.  
  
6.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
7.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:1::fe**. En **longitud del prefijo de subred**, tipo **64**y, a continuación, haga clic en **Aceptar**.  
  
8.  En el **propiedades de la red corporativa** haga clic en el cuadro de diálogo **cerrar**.  
  
9. En el **las conexiones de red** ventana, haga clic en el adaptador de red que está conectado a Corpnet-2, haga clic en **cambiar el nombre de**, tipo **2 Corpnet**, y presione ENTRAR.  
  
10. Haga clic en **2 Corpnet**y, a continuación, haga clic en **propiedades**.  
  
11. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
12. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.2.0.254**. En **máscara de subred**, tipo **255.255.255.0**y, a continuación, haga clic en **Aceptar**.  
  
13. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
14. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:2::fe**. En **longitud del prefijo de subred**, tipo **64**y, a continuación, haga clic en **Aceptar**.  
  
15. En el **2 Corpnet propiedades** haga clic en el cuadro de diálogo **cerrar**.  
  
16. Cierre la ventana **Conexiones de red**.  
  
17. En la consola de administrador del servidor, en **servidor Local**, en el **propiedades** área, junto a **nombre_equipo**, haga clic en el vínculo.  
  
18. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
19. En el **cambios de dominio o el nombre de equipo** cuadro de diálogo **nombre_equipo**, tipo **ENRUTADOR1**y, a continuación, haga clic en **Aceptar**.  
  
20. Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
21. En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
22. Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
23. Una vez reiniciado el equipo, inicie sesión con la cuenta de administrador local.  
  
## <a name="turn-off-the-firewall"></a>Desactivar el firewall  
Este equipo está configurado solo para proporcionar enrutamiento entre las subredes de la red corporativa y 2 de la red corporativa; por lo tanto, el firewall debe estar desactivado.  
  
### <a name="to-turn-off-the-firewall"></a>Para desactivar el firewall  
  
1.  En el **iniciar** , escriba**wf.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el Firewall de Windows con seguridad avanzada, en la **acciones** panel, haga clic en **propiedades**.  
  
3.  En el **Firewall de Windows con seguridad avanzada** cuadro de diálogo el **perfil de dominio** ficha **estado del Firewall**, haga clic en **desactivar**.  
  
4.  En el **Firewall de Windows con seguridad avanzada** cuadro de diálogo el **perfil privado** ficha **estado del Firewall**, haga clic en **desactivar**.  
  
5.  En el **Firewall de Windows con seguridad avanzada** cuadro de diálogo el **perfil público** ficha **estado del Firewall**, haga clic en **desactivar**y, a continuación, Haga clic en **Aceptar**.  
  
6.  Cierre Firewall de Windows con seguridad avanzada.  
  
## <a name="configure-routing-and-forwarding"></a>Configurar el reenvío y enrutamiento  
Para proporcionar servicios entre las subredes de la red corporativa y 2 Corpnet de reenvío y enrutamiento, debe habilitar el reenvío en las interfaces de red y configurar rutas estáticas entre las subredes.  
  
### <a name="to-configure-static-routes"></a>Para configurar rutas estáticas  
  
1.  En el **iniciar** , escriba**cmd.exe**, y, a continuación, presione ENTRAR.  
  
2.  Habilitar el reenvío en las interfaces IPv4 e IPv6 de los dos adaptadores de red mediante los siguientes comandos. Después de escribir cada comando, presione ENTRAR.  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  Habilitar el enrutamiento IP-HTTPS entre las subredes de la red corporativa y 2 de la red corporativa.  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  Habilitar el enrutamiento de Teredo entre las subredes de la red corporativa y 2 de la red corporativa.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Cierre la ventana del símbolo del sistema.
