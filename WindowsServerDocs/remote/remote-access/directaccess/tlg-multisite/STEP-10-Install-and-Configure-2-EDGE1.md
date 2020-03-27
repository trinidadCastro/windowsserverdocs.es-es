---
title: 'PASO 10: instalación y configuración de 2-EDGE1'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7d21d80f4970a501e31a053483c37268bdddb811
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314612"
---
# <a name="step-10-install-and-configure-2-edge1"></a>PASO 10: instalación y configuración de 2-EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

2-EDGE1 la configuración consta de lo siguiente:  
  
- Instale el sistema operativo en 2-EDGE1. Instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en 2-EDGE1.  
  
- Configurar las propiedades TCP/IP. Configure 2-EDGE1 con direcciones estáticas en ambas interfaces de red.  
  
- Configurar el enrutamiento entre subredes. Para habilitar la comunicación entre las subredes corporativas y 2-CorpNet, debe configurar el enrutamiento.  
  
- Join 2-EDGE1 al dominio CORP2. Join 2-EDGE1 al dominio corp2.corp.contoso.com.  
  
- Obtener certificados en 2-EDGE1. Los certificados son necesarios para la conexión IPsec entre los clientes de DirectAccess y el servidor de acceso remoto, y para autenticar el agente de escucha IP-HTTPS cuando los clientes se conectan a través de HTTPS.  
  
- Proporcionar acceso a CORP\User1. El usuario CORP\User1 es el administrador de acceso remoto. Para permitir que este usuario realice cambios en 2-EDGE1 desde EDGE1, debe conceder acceso al usuario.  
  
- Instale el rol de acceso remoto en 2-EDGE1. Para habilitar una implementación multisitio, debe instalar el rol de acceso remoto en 2-EDGE1.  
  
2-EDGE1 debe tener instalados dos adaptadores de red.  
  
## <a name="install-the-operating-system-on-2-edge1"></a><a name="installOS"></a>Instalación del sistema operativo en 2-EDGE1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta de administrador local.  
  
3.  Conecte 2-EDGE1 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.  
  
4.  Conecte un adaptador de red a la subred 2-CorpNet y el otro a la red Internet simulada.  
  
## <a name="configure-tcpip-properties"></a><a name="tcpip"></a>Configurar las propiedades de TCP/IP  
  
1.  En la consola de Administrador del servidor, haga clic en **servidor local**y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En **conexiones de red**, haga clic con el botón secundario en la conexión de red que está conectada a la subred 2-CorpNet, haga clic en **cambiar nombre**, escriba **2-CorpNet**y, a continuación, presione Entrar.  
  
3.  Haga clic con el botón secundario en **2-CorpNet**y, a continuación, haga clic en **propiedades**.  
  
4.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
5.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.2.0.20**, en **máscara de subred**, escriba **255.255.255.0**.  
  
6.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, escriba **10.2.0.1**y, en **servidor DNS alternativo**, escriba **10.0.0.1**.  
  
7.  Haga clic en  **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
8.  En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
9. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
10. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:2:: 20**, en **longitud de prefijo de subred**, escriba **64**. Haga clic en **usar las siguientes direcciones de servidor DNS**y, en **servidor DNS preferido**, escriba **2001: db8:2:: 1**, en **servidor DNS alternativo**, escriba **2001: db8:1:: 1**.  
  
11. Haga clic en  **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
12. En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
13. En el cuadro de diálogo **2-propiedades de la red corporativa** , haga clic en **cerrar**.  
  
14. En la ventana **conexiones de red** , haga clic con el botón secundario en la conexión de red que está conectada a la subred de Internet, haga clic en **cambiar nombre**, escriba **Internet**y, a continuación, presione Entrar.  
  
15. Haga clic con el botón secundario en **Internet** y, a continuación, haga clic en **Propiedades**.  
  
16. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
17. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **131.107.0.20**. En **Máscara de subred**, escriba **255.255.255.0**.  
  
18. Haga clic en **Opciones avanzadas**. En la ficha **Configuración de IP**, en el área **Direcciones IP**, haga clic en **Agregar**. En el cuadro de diálogo **Dirección TCP/IP** , en tipo de **dirección IP** **131.107.0.21**, en **máscara de subred** , escriba **255.255.255.0**y, a continuación, haga clic en **Agregar**.  
  
19. Haga clic en la pestaña **DNS**.  
  
20. En **sufijo DNS para esta conexión**, escriba **ISP.example.com**, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
21. Cierre la ventana **Conexiones de red**.  
  
## <a name="configure-routing-between-subnets"></a><a name="routing"></a>Configurar el enrutamiento entre subredes  
  
1.  En la pantalla **Inicio** , escriba**cmd. exe**y, a continuación, presione Entrar.  
  
2.  En la ventana del símbolo del sistema, escriba los siguientes comandos. Después de escribir cada comando, presione Entrar.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Para comprobar la comunicación de red entre 2 y EDGE1 y DC1, escriba **ping DC1.Corp.contoso.com**.  
  
4.  Compruebe que hay cuatro respuestas de la dirección IPv4, 10.0.0.1 o de la dirección IPv6, 2001: db8:1:: 1.  
  
5.  Cierre la ventana del símbolo del sistema.  
  
## <a name="join-2-edge1-to-the-corp2-domain"></a><a name="Join"></a>Unión 2-EDGE1 al dominio CORP2  
  
1.  En la consola de Administrador del servidor, en **servidor local**, en el área **propiedades** , junto a **nombre de equipo**, haga clic en el vínculo.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
3.  En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , en **nombre de equipo**, escriba **2-EDGE1**. En **miembro de**, haga clic en **dominio**, escriba **Corp2.Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
4.  Si se le pide un nombre de usuario y una contraseña, escriba **Administrador** y su contraseña y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo que le muestra el dominio corp2.corp.contoso.com, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Una vez reiniciado el equipo, haga clic en **cambiar de usuario**y, a continuación, haga clic en **otro usuario** e inicie sesión en el dominio CORP2 con la cuenta de administrador.  
  
## <a name="obtain-certificates-on-2-edge1"></a><a name="certs"></a>Obtención de certificados en 2 EDGE1  
  
1.  En la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, **Finalizar** y **Aceptar**.  
  
4.  En el árbol de consola del complemento certificados, Abra **certificados (equipo local) \Personal**.  
  
5.  Haga clic con el botón secundario en **personal**, seleccione **todas las tareas**y, a continuación, haga clic en **solicitar nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , active las casillas **autenticación cliente-servidor** y **servidor Web** y, a continuación, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
8.  En el cuadro de diálogo **propiedades de certificado** , en la pestaña **sujeto** , en el área **nombre de sujeto** , en **tipo**, seleccione **nombre común**.  
  
9. En **valor**, escriba **2-edge1.contoso.com**y, a continuación, haga clic en **Agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **valor**, escriba **2-edge1.contoso.com**y, a continuación, haga clic en **Agregar**.  
  
12. En la pestaña **General** , en **nombre descriptivo**, escriba **certificado IP-https**.  
  
13. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
14. En el panel de detalles del complemento certificados, compruebe que se inscribió un nuevo certificado con el nombre 2-edge1.contoso.com con los propósitos planteados de autenticación del servidor y que se inscribió un nuevo certificado con el nombre 2-edge1.corp2.corp.contoso.com con Propósitos planteados de autenticación de cliente y autenticación de servidor.  
  
15. Cierre la ventana de la consola. Si se le pide que guarde la configuración, haga clic en **no**.  
  
## <a name="provide-access-to-corpuser1"></a><a name="Access"></a>Proporcionar acceso a CORP\User1  
  
1.  En la pantalla **Inicio** , escriba**compmgmt. msc**y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo, haga clic en **usuarios y grupos locales**.  
  
3.  Haga doble clic en **grupos**y, a continuación, haga doble clic en **administradores**.  
  
4.  En el cuadro de diálogo **propiedades de administradores** , haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , haga clic en **ubicaciones**.  
  
5.  En el cuadro de diálogo **ubicaciones** , en el árbol **Ubicación** , haga clic en **Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
6.  En **Escriba los nombres de objeto que desea seleccionar** , escriba **user1**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **propiedades de administradores** , haga clic en **Aceptar**.  
  
8.  Cierre la ventana Administración de equipos.  
  
## <a name="install-the-remote-access-role-on-2-edge1"></a><a name="InstallDA"></a>Instalar el rol de acceso remoto en 2-EDGE1  
  
1.  En la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el diálogo **Seleccionar roles de servidor**, seleccione **Acceso remoto**, haga clic en **Agregar características** y después en **Siguiente**.  
  
4.  Haz clic cinco veces en **Siguiente**.  
  
5.  En el cuadro de diálogo **Confirmar selecciones de instalación**, haz clic en **Instalar**.  
  
6.  En el cuadro de diálogo **Progreso de la instalación**, comprueba que la instalación se realiza correctamente y, a continuación, haz clic en **Cerrar**.  
  


