---
title: PASO 10 instalar y configurar 2 EDGE1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9810d7294a2651d4811bc5969eaf6a118db8ed56
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283267"
---
# <a name="step-10-install-and-configure-2-edge1"></a>PASO 10 instalar y configurar 2 EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Configuración de 2-EDGE1 consta de las siguientes acciones:  
  
- Instale el sistema operativo en 2 EDGE1. Instale a Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en 2 EDGE1.  
  
- Configurar las propiedades TCP/IP. Configure 2 EDGE1 con las direcciones estáticas en ambas interfaces de red.  
  
- Configurar el enrutamiento entre subredes. Para habilitar la comunicación entre las subredes de la red corporativa y 2 de la red corporativa, debe configurar el enrutamiento.  
  
- Únase a 2 EDGE1 al dominio CORP2. Únase a 2 EDGE1 al dominio corp2.corp.contoso.com.  
  
- Obtener certificados en 2 EDGE1. Los certificados son necesarios para la conexión IPsec entre los clientes de DirectAccess y el servidor de acceso remoto y para autenticar el agente de escucha de IP-HTTPS cuando los clientes se conectan a través de HTTPS.  
  
- Proporcionar acceso a CORP\User1. El usuario CORP\User1 es el Administrador de acceso remoto. Para permitir que este usuario realizar cambios en 2 EDGE1 desde EDGE1, debe conceder acceso al usuario.  
  
- Instalar el rol de acceso remoto en 2 EDGE1. Para habilitar una implementación multisitio, debe instalar el rol de acceso remoto en 2 EDGE1.  
  
2-EDGE1 debe tener dos adaptadores de red instalados.  
  
## <a name="installOS"></a>Instalar el sistema operativo en 2 EDGE1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  2-EDGE1 de conectarse a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  Conecte un adaptador de red a la subred Corpnet de 2 y el otro a la red Internet simulada.  
  
## <a name="tcpip"></a>Configurar las propiedades de TCP/IP  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En **las conexiones de red**, haga clic en la conexión de red que está conectada a la subred Corpnet de 2, haga clic en **cambiar el nombre de**, tipo **2 Corpnet**, y, a continuación, presione ENTRAR.  
  
3.  Haga clic en **2 Corpnet**y, a continuación, haga clic en **propiedades**.  
  
4.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
5.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.2.0.20**, en **máscara de subred**, tipo **255.255.255.0**.  
  
6.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, tipo **10.2.0.1**y en **servidor DNS alternativo**, tipo **10.0.0.1**.  
  
7.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
8.  En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
9. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
10. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:2::20**, en **longitud del prefijo de subred**, tipo **64**. Haga clic en **usar las siguientes direcciones de servidor DNS**y en **servidor DNS preferido**, tipo **2001:db8:2::1**, en **servidor DNS alternativo**, tipo **2001:db8:1::1**.  
  
11. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
12. En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
13. En el **2 Corpnet propiedades** cuadro de diálogo, haga clic en **cerrar**.  
  
14. En el **las conexiones de red** ventana, haga clic en la conexión de red que está conectada a la subred de Internet, haga clic en **cambiar el nombre de**, tipo **Internet**, y, a continuación, presione ENTRAR .  
  
15. Haga clic con el botón secundario en **Internet** y, a continuación, haga clic en **Propiedades**.  
  
16. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
17. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **131.107.0.20**. En **Máscara de subred**, escriba **255.255.255.0**.  
  
18. Haga clic en **Avanzada**. En la ficha **Configuración de IP**, en el área **Direcciones IP**, haga clic en **Agregar**. En el **dirección TCP/IP** cuadro de diálogo **dirección IP** tipo **131.107.0.21**, en **máscara de subred** tipo **255.255.255.0** y, a continuación, haga clic en **agregar**.  
  
19. Haga clic en la pestaña **DNS**.  
  
20. En **sufijo DNS para esta conexión**, tipo **ISP.ejemplo.com**, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
21. Cierre la ventana **Conexiones de red**.  
  
## <a name="routing"></a>Configurar el enrutamiento entre subredes  
  
1.  En el **iniciar** , escriba**cmd.exe**, y, a continuación, presione ENTRAR.  
  
2.  En la ventana de símbolo del sistema, escriba los siguientes comandos. Después de escribir cada comando, presione ENTRAR.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Para comprobar la comunicación de red entre 2-EDGE1 y DC1, escriba **ping dc1.corp.contoso.com**.  
  
4.  Compruebe que hay cuatro respuestas desde la dirección IPv4, 10.0.0.1, o desde la dirección IPv6, 2001:db8:1::1.  
  
5.  Cierre la ventana del símbolo del sistema.  
  
## <a name="Join"></a>Unir 2 EDGE1 al dominio CORP2  
  
1.  En la consola de administrador del servidor, en **servidor Local**, en el **propiedades** área, junto a **nombre_equipo**, haga clic en el vínculo.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
3.  En el **cambios de dominio o el nombre de equipo** cuadro de diálogo **nombre_equipo**, tipo **2 EDGE1**. En **miembro de**, haga clic en **dominio**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
4.  Si se le pide un nombre de usuario y contraseña, escriba **administrador** y su contraseña y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo que le da la bienvenida al dominio corp2.corp.contoso.com, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Una vez reiniciado el equipo, haga clic en **cambiar de usuario**y, a continuación, haga clic en **otro usuario** e inicie sesión en el dominio CORP2 con la cuenta de administrador.  
  
## <a name="certs"></a>Obtener certificados en 2 EDGE1  
  
1.  En el **iniciar** , escriba**mmc.exe**, y, a continuación, presione ENTRAR.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, **Finalizar** y **Aceptar**.  
  
4.  En el árbol de consola del complemento certificados, abra **\Personal certificados (equipo Local)** .  
  
5.  Haga clic en **Personal**, apunte a **todas las tareas**y, a continuación, haga clic en **solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En el **solicitar certificados** página, seleccione el **autenticación cliente-servidor** y **servidor Web** casillas de verificación y, a continuación, haga clic en **encontrará más información necesario para inscribir este certificado**.  
  
8.  En el **las propiedades del certificado** cuadro de diálogo el **asunto** ficha la **nombre de sujeto** área, en **tipo**, seleccione  **Nombre común**.  
  
9. En **valor**, tipo **2 edge1.contoso.com**y, a continuación, haga clic en **agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **valor**, escriba **2 edge1.contoso.com**y, a continuación, haga clic en **agregar**.  
  
12. En el **General** ficha **Nombre_descriptivo**, tipo **certificado IP-HTTPS**.  
  
13. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
14. En el panel de detalles del complemento certificados, compruebe que se inscribió un nuevo certificado con el nombre 2-edge1.contoso.com con diseñado con fines de autenticación de servidor, y se inscribió un nuevo certificado con el nombre 2-edge1.corp2.corp.contoso.com con Propósitos planteados de autenticación del cliente y la autenticación de servidor.  
  
15. Cierre la ventana de consola. Si se le solicite guardar la configuración, haga clic en **No**.  
  
## <a name="Access"></a>Proporcionar acceso a CORP\User1  
  
1.  En el **iniciar** , escriba**compmgmt.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo, haga clic en **usuarios y grupos locales**.  
  
3.  Haga doble clic en **grupos**y, a continuación, haga doble clic en **administradores**.  
  
4.  En el **propiedades de administradores** cuadro de diálogo, haga clic en **agregar**y en el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, haga clic en  **Ubicaciones**.  
  
5.  En el **ubicaciones** cuadro de diálogo el **ubicación** de árbol, haga clic en **corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
6.  En el **escriba los nombres de objeto para seleccionar** tipo **User1**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el **propiedades de administradores** cuadro de diálogo, haga clic en **Aceptar**.  
  
8.  Cierre la ventana de administración de equipos.  
  
## <a name="InstallDA"></a>Instalar el rol de acceso remoto en 2 EDGE1  
  
1.  En la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el diálogo **Seleccionar roles de servidor**, seleccione **Acceso remoto**, haga clic en **Agregar características** y después en **Siguiente**.  
  
4.  Haz clic cinco veces en **Siguiente**.  
  
5.  En el cuadro de diálogo **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
6.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  


