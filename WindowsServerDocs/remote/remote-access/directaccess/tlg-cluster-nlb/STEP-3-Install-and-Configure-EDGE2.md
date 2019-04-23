---
title: PASO 3 instalar y configurar EDGE2
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0692d47d50d84a66b5c3cc41d2ba2fca1004cafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870886"
---
# <a name="step-3-install-and-configure-edge2"></a>PASO 3 instalar y configurar EDGE2

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

EDGE2 es el segundo miembro de un clúster de acceso remoto. EDGE2 está instalado y configurado antes de habilitar la configuración del clúster.

Realice los pasos siguientes para configurar EDGE2:

## <a name="installOS"></a>Instalar el sistema operativo en EDGE2  
  
1.  En EDGE2, inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Siga las instrucciones para completar la instalación, especificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa) y una contraseña segura para la cuenta de administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  Conecte EDGE2 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  Conecte un adaptador de red a la subred Corpnet o el conmutador virtual que representa la subred de la red corporativa y el otro a la subred de Internet o un conmutador virtual que representa la subred de Internet.  
  
## <a name="TCP"></a>Configurar las propiedades de TCP/IP  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En el **las conexiones de red** , haga clic en la conexión de red que está conectada a la subred de la red corporativa o un conmutador virtual y, a continuación, haga clic en **cambiar el nombre**.  
  
3.  Tipo **Corpnet**, y, a continuación, presione ENTRAR.  
  
4.  Haga clic en **Corpnet**y, a continuación, haga clic en **propiedades**.  
  
5.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
6.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.0.0.8**. En **Máscara de subred**, escriba **255.255.255.0**.  
  
7.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **Servidor DNS preferido**, escriba **10.0.0.1**.  
  
8.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
9. En **sufijo DNS para esta conexión**, tipo **corp.contoso.com**, haga clic en **Aceptar** dos veces.  
  
10. Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
11. Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:1::8**. En **longitud del prefijo de subred**, tipo **64**.  
  
12. Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, tipo **2001:db8:1::1**.  
  
13. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
14. En **sufijo DNS para esta conexión**, tipo **corp.contoso.com**, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
15. En el **las conexiones de red** , haga clic en la conexión de red que está conectada a la subred de Internet y, a continuación, haga clic en **cambiar el nombre**.  
  
16. Tipo **Internet**, y, a continuación, presione ENTRAR.  
  
17. Haga clic con el botón secundario en **Internet** y, a continuación, haga clic en **Propiedades**.  
  
18. Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
19. Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **131.107.0.8**. En **máscara de subred**, escriba **255.255.255.0**.  
  
20. Haga clic en el **DNS** ficha  
  
21. En **sufijo DNS para esta conexión**, tipo **ISP.ejemplo.com**y, a continuación, haga clic en **Aceptar** dos veces y, a continuación, haga clic en **cerrar**.  
  
22. Cierre la ventana **Conexiones de red**.  
  
23. Para comprobar la comunicación de red entre EDGE2 y DC1, haga clic en **iniciar**, tipo **cmd**, y, a continuación, presione ENTRAR.  
  
24. En la ventana de símbolo del sistema, escriba **ping dc1.corp.contoso.com** y presione ENTRAR. Compruebe que hay cuatro respuestas desde 10.0.0.1 o 2001:db8:1::1 de direcciones de IPv6  
  
25. Cierre la ventana del símbolo del sistema.  
  
## <a name="rename"></a>Cambie el nombre EDGE2 y unirlo al dominio  
  
1.  En la consola de administrador del servidor, en **servidor Local**, en el **propiedades** área, junto a **nombre_equipo**, haga clic en el vínculo.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
3.  En el **cambios de dominio o el nombre de equipo** cuadro de diálogo el **nombre_equipo** , escriba **EDGE2**. En el **miembro de** área, haga clic en **dominio**y en el cuadro de texto, escriba **corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando se le pida un nombre de usuario y una contraseña, escriba **User1** y su contraseña, y haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio corp.contoso.com, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Después de reiniciar, inicie sesión como corp\usuario1.  
  
## <a name="IPHTTPSCert"></a>Instale el certificado IP-HTTPS  
  
1.  En el **iniciar** , escriba**mmc.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el **agregar o quitar complementos** cuadro de diálogo, haga clic en **certificados**, haga clic en **agregar**, haga clic en **cuenta de equipo**, haga clic en  **Siguiente**, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  En el panel izquierdo de la consola, vaya a **\Personal\Certificates certificados (equipo Local)**. Haga clic en el **certificados** nodo, seleccione **todas las tareas**y, a continuación, haga clic en **solicitar un nuevo certificado**.  
  
5.  En el Asistente para inscripción de certificados, haga clic en **siguiente** dos veces.  
  
6.  En el **solicitar certificados** página, seleccione el **servidor Web** casilla de verificación y, a continuación, haga clic en **se necesita más información para inscribir este certificado**.  
  
7.  En el **las propiedades del certificado** cuadro de diálogo el **asunto** ficha la **nombre de sujeto** área, en el **tipo** lista, haga clic en **Nombre común**.  
  
8.  En **valor**, tipo **edge1.contoso.com**y, a continuación, haga clic en **agregar**.  
  
9. En el **nombre alternativo** área, en el **tipo** lista, haga clic en **DNS**.  
  
10. En **valor**, tipo **edge1.contoso.com**y, a continuación, haga clic en **agregar**.  
  
11. En el **General** ficha **Nombre_descriptivo**, tipo **certificado IP-HTTPS**.  
  
12. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
13. En el panel de detalles del complemento certificados, compruebe que se inscribió un nuevo certificado con el nombre de edge1.contoso.com con diseñado con fines de autenticación de servidor.  
  
14. Cierre la ventana de consola. Si se le solicite guardar la configuración, haga clic en **No**.  
  
## <a name="InstallDA"></a>Instalar el rol de acceso remoto en EDGE2  
  
1.  En la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el diálogo **Seleccionar roles de servidor**, seleccione **Acceso remoto**, haga clic en **Agregar características** y después en **Siguiente**.  
  
4.  Haz clic cinco veces en **Siguiente**.  
  
5.  En el cuadro de diálogo **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
6.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  


