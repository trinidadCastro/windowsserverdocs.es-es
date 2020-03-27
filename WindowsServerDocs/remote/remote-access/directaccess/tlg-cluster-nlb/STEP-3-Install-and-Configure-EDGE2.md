---
title: Paso 3 instalación y configuración de EDGE2
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cb869ad1617d52562e73eb6965a9f1c2184a56a7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310783"
---
# <a name="step-3-install-and-configure-edge2"></a>Paso 3 instalación y configuración de EDGE2

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

EDGE2 es el segundo miembro de un clúster de acceso remoto. EDGE2 se instala y configura antes de habilitar la configuración del clúster.

Realice los pasos siguientes para configurar EDGE2:

## <a name="install-the-operating-system-on-edge2"></a><a name="installOS"></a>Instalación del sistema operativo en EDGE2  
  
1.  En EDGE2, inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta de administrador local.  
  
3.  Conecte EDGE2 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.  
  
4.  Conecte un adaptador de red a la subred de la red corporativa o al conmutador virtual que representa la subred corporativa y el otro a la subred de Internet o al conmutador virtual que representa la subred de Internet.  
  
## <a name="configure-tcpip-properties"></a><a name="TCP"></a>Configurar las propiedades de TCP/IP  
  
1.  En la consola de Administrador del servidor, haga clic en **servidor local**y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En la ventana **conexiones de red** , haga clic con el botón secundario en la conexión de red que está conectada a la subred corporativa o al conmutador virtual y, a continuación, haga clic en **cambiar nombre**.  
  
3.  Escriba **CorpNet**y presione Entrar.  
  
4.  Haga clic con el botón secundario en **CorpNet**y, a continuación, haga clic en **propiedades**.  
  
5.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
6.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.0.0.8**. En **Máscara de subred**, escriba **255.255.255.0**.  
  
7.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **Servidor DNS preferido**, escriba **10.0.0.1**.  
  
8.  Haga clic en  **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
9. En **sufijo DNS para esta conexión**, escriba **Corp.contoso.com**y haga clic en **Aceptar** dos veces.  
  
10. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
11. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:1:: 8**. En **longitud del prefijo de subred**, escriba **64**.  
  
12. Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, escriba **2001: db8:1:: 1**.  
  
13. Haga clic en  **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
14. En **sufijo DNS para esta conexión**, escriba **Corp.contoso.com**, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
15. En la ventana **conexiones de red** , haga clic con el botón secundario en la conexión de red que está conectada a la subred de Internet y, a continuación, haga clic en **cambiar nombre**.  
  
16. Escriba **Internet**y, a continuación, presione Entrar.  
  
17. Haga clic con el botón secundario en **Internet** y, a continuación, haga clic en **Propiedades**.  
  
18. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
19. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **131.107.0.8**. En **máscara de subred**, escriba **255.255.255.0**.  
  
20. Haga clic en la pestaña **DNS** .  
  
21. En **sufijo DNS para esta conexión**, escriba **ISP.example.com**y, a continuación, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
22. Cierre la ventana **Conexiones de red**.  
  
23. Para comprobar la comunicación de red entre EDGE2 y DC1, haga clic en **Inicio**, escriba **cmd**y, a continuación, presione Entrar.  
  
24. En la ventana del símbolo del sistema, escriba **ping DC1.Corp.contoso.com** y presione Entrar. Compruebe que hay cuatro respuestas desde 10.0.0.1 o la dirección IPv6 2001: db8:1:: 1  
  
25. Cierre la ventana del símbolo del sistema.  
  
## <a name="rename-edge2-and-join-it-to-the-domain"></a><a name="rename"></a>Cambiar el nombre de EDGE2 y unirlo al dominio  
  
1.  En la consola de Administrador del servidor, en **servidor local**, en el área **propiedades** , junto a **nombre de equipo**, haga clic en el vínculo.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
3.  En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , en el cuadro **nombre de equipo** , escriba **EDGE2**. En el área **miembro de** , haga clic en **dominio**y, en el cuadro de texto, escriba **Corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando se le pida un nombre de usuario y una contraseña, escriba **User1** y su contraseña y después haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio corp.contoso.com, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Después de reiniciar, inicie sesión como CORP\User1.  
  
## <a name="install-the-ip-https-certificate"></a><a name="IPHTTPSCert"></a>Instalación del certificado IP-HTTPS  
  
1.  En la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos** , haga clic en **certificados**, en **Agregar**, en **cuenta de equipo**, en **siguiente**, en **Finalizar**y, finalmente, en **Aceptar**.  
  
4.  En el panel izquierdo de la consola, vaya a **certificados (equipo local) \personal\certificados**. Haga clic con el botón secundario en el nodo **certificados** , seleccione **todas las tareas**y, a continuación, haga clic en **solicitar nuevo certificado**.  
  
5.  En el Asistente para inscripción de certificados, haga clic en **siguiente** dos veces.  
  
6.  En la página **solicitar certificados** , active la casilla **servidor Web** y, a continuación, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
7.  En el cuadro de diálogo **propiedades de certificado** , en la pestaña **sujeto** , en el área **nombre de sujeto** , en la lista **tipo** , haga clic en **nombre común**.  
  
8.  En **valor**, escriba **edge1.contoso.com**y, a continuación, haga clic en **Agregar**.  
  
9. En el área **nombre alternativo** , en la lista **tipo** , haga clic en **DNS**.  
  
10. En **valor**, escriba **edge1.contoso.com**y, a continuación, haga clic en **Agregar**.  
  
11. En la pestaña **General** , en **nombre descriptivo**, escriba **certificado IP-https**.  
  
12. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
13. En el panel de detalles del complemento certificados, compruebe que se inscribió un nuevo certificado con el nombre edge1.contoso.com con fines planteados de autenticación del servidor.  
  
14. Cierre la ventana de la consola. Si se le pide que guarde la configuración, haga clic en **no**.  
  
## <a name="install-the-remote-access-role-on-edge2"></a><a name="InstallDA"></a>Instalar el rol de acceso remoto en EDGE2  
  
1.  En la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el diálogo **Seleccionar roles de servidor**, seleccione **Acceso remoto**, haga clic en **Agregar características** y después en **Siguiente**.  
  
4.  Haz clic cinco veces en **Siguiente**.  
  
5.  En el cuadro de diálogo **Confirmar selecciones de instalación**, haz clic en **Instalar**.  
  
6.  En el cuadro de diálogo **Progreso de la instalación**, comprueba que la instalación se realiza correctamente y, a continuación, haz clic en **Cerrar**.  
  


