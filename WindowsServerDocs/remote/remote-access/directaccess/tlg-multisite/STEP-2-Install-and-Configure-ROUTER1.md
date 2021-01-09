---
title: Paso 2 Instalación y configuración de ENRUTADOR1
description: Aprenda a instalar y configurar ENRUTADOR1.
manager: brianlic
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: d505d40af963645bff711956c83e239668668716
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040515"
---
# <a name="step-2-install-and-configure-router1"></a>Paso 2 Instalación y configuración de ENRUTADOR1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta guía del laboratorio de pruebas multisitio, el equipo del enrutador proporciona un puente IPv4 e IPv6 entre las subredes corporativas y 2-CorpNet, y actúa como enrutador para el tráfico de IP-HTTPS y Teredo.

- Instalación del sistema operativo en ENRUTADOR1

- Configurar las propiedades de TCP/IP y cambiar el nombre del equipo

- Desactivar el Firewall

- Configurar el enrutamiento y el reenvío

## <a name="install-the-operating-system-on-router1"></a>Instalación del sistema operativo en ENRUTADOR1
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

### <a name="to-install-the-operating-system-on-router1"></a>Para instalar el sistema operativo en ENRUTADOR1

1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa).

2.  Siga las instrucciones para completar la instalación, especificando una contraseña segura para la cuenta Administrador local. Inicie sesión con la cuenta Administrador local.

3.  Conecte ENRUTADOR1 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.

4.  Conecte ENRUTADOR1 a las subredes corporativas y 2-CorpNet.

## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurar las propiedades de TCP/IP y cambiar el nombre del equipo
Configure los valores de TCP/IP en el enrutador y cambie el nombre del equipo a ENRUTADOR1.

### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Para configurar las propiedades de TCP/IP y cambiar el nombre del equipo

1.  En la consola de Administrador del servidor, haga clic en **servidor local** y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.

2.  En la ventana **conexiones de red** , haga clic con el botón secundario en el adaptador de red que está conectado a CorpNet, haga clic en **cambiar nombre**, escriba **CorpNet** y presione Entrar.

3.  Haga clic con el botón secundario en **CorpNet** y, a continuación, haga clic en **propiedades**.

4.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.

5.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.0.0.254**. En **máscara de subred**, escriba **255.255.255.0** y, a continuación, haga clic en **Aceptar**.

6.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.

7.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:1:: fe**. En **longitud del prefijo de subred**, escriba **64** y, a continuación, haga clic en **Aceptar**.

8.  En el cuadro de diálogo **propiedades de CorpNet** , haga clic en **cerrar**.

9. En la ventana **conexiones de red** , haga clic con el botón secundario en el adaptador de red que está conectado a 2-CorpNet, haga clic en **cambiar nombre**, escriba **2-CorpNet** y presione Entrar.

10. Haga clic con el botón secundario en **2-CorpNet** y, a continuación, haga clic en **propiedades**.

11. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.

12. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.2.0.254**. En **máscara de subred**, escriba **255.255.255.0** y, a continuación, haga clic en **Aceptar**.

13. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.

14. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:2:: fe**. En **longitud del prefijo de subred**, escriba **64** y, a continuación, haga clic en **Aceptar**.

15. En el cuadro de diálogo **2-propiedades de la red corporativa,** haga clic en **cerrar**.

16. Cierre la ventana **Conexiones de red**.

17. En la consola de Administrador del servidor, en **servidor local**, en el área **propiedades** , junto a **nombre de equipo**, haga clic en el vínculo.

18. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.

19. En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , en **nombre de equipo**, escriba **ENRUTADOR1** y, a continuación, haga clic en **Aceptar**.

20. Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.

21. En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.

22. Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.

23. Una vez reiniciado el equipo, inicie sesión con la cuenta de administrador local.

## <a name="turn-off-the-firewall"></a>Desactivar el Firewall
Este equipo está configurado solo para proporcionar enrutamiento entre las subredes corporativas y 2-CorpNet; por lo tanto, el Firewall debe estar desactivado.

### <a name="to-turn-off-the-firewall"></a>Para desactivar el Firewall

1.  En la pantalla **Inicio** , escriba **WF. msc** y, a continuación, presione Entrar.

2.  En firewall de Windows con seguridad avanzada, en el panel **acciones** , haga clic en **propiedades**.

3.  En el cuadro de diálogo **firewall de Windows con seguridad avanzada** , en la pestaña **Perfil de dominio** , en **Estado de Firewall**, haga clic en **desactivado**.

4.  En el cuadro de diálogo **firewall de Windows con seguridad avanzada** , en la pestaña **perfil privado** , en **Estado de Firewall**, haga clic en **desactivado**.

5.  En el cuadro de diálogo **firewall de Windows con seguridad avanzada** , en la ficha **perfil público** , en **Estado de Firewall**, haga clic en **desactivado** y, a continuación, haga clic en **Aceptar**.

6.  Cierre Firewall de Windows con seguridad avanzada.

## <a name="configure-routing-and-forwarding"></a>Configurar el enrutamiento y el reenvío
Para proporcionar servicios de enrutamiento y reenvío entre las subredes corporativas y 2-CorpNet, debe habilitar el reenvío en las interfaces de red y configurar rutas estáticas entre las subredes.

### <a name="to-configure-static-routes"></a>Para configurar rutas estáticas

1.  En la pantalla **Inicio** , escriba **cmd.exe** y, a continuación, presione Entrar.

2.  Habilite el reenvío en las interfaces IPv4 e IPv6 de ambos adaptadores de red mediante los comandos siguientes. Después de escribir cada comando, presione Entrar.

    ```
    netsh interface IPv4 set interface Corpnet forwarding=enabled
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled
    netsh interface IPv6 set interface Corpnet forwarding=enabled
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled
    ```

3.  Habilite el enrutamiento IP-HTTPS entre las subredes corporativas y 2-CorpNet.

    ```
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20
    ```

4.  Habilite el enrutamiento Teredo entre las subredes corporativas y 2-CorpNet.

    ```
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20
    ```

5.  Cierre la ventana del símbolo del sistema.
