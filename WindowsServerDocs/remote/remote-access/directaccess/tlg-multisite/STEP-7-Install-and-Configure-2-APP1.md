---
title: PASO 7 instalar y configurar APP1 de 2
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
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b0f91b4d2b876cb7b22dc8614e7ea5dcce6da2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833566"
---
# <a name="step-7-install-and-configure-2-app1"></a>PASO 7 instalar y configurar APP1 de 2

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

2-APP1 proporciona servicios de uso compartido de archivos y web. Configuración de 2-APP1 consta de las siguientes acciones:  
  
- Instalar el sistema operativo en 2-APP1  
  
- Configurar las propiedades TCP/IP  
  
- Una 2-APP1 al dominio CORP2  
  
- Instalar el rol de servidor Web (IIS) en 2-APP1  
  
- Cree una carpeta compartida en 2-APP1 
  
## <a name="bkmk_InstallOS"></a>Instalar el sistema operativo en 2-APP1  
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Para instalar el sistema operativo en 2-APP1  
  
1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa).  
  
2.  Siga las instrucciones para completar la instalación, especificando una contraseña segura para la cuenta Administrador local. Inicie sesión con la cuenta Administrador local.  
  
3.  2-APP1 de conectarse a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconectarse de Internet.  
  
4.  Conectar 2-APP1 a la subred Corpnet de 2.  
  
## <a name="bkmk_TCP"></a>Configurar las propiedades de TCP/IP  
Configurar las propiedades de TCP/IP en 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Para configurar las propiedades TCP/IP  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En el **las conexiones de red** ventana, haga clic en **conexión cableada Ethernet**y, a continuación, haga clic en **propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, tipo **10.2.0.3**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, tipo **10.2.0.254**.  
  
5.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, tipo **10.2.0.1**.  
  
6.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**. En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y haga clic en **Aceptar** dos veces.  
  
7.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
8.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, tipo **2001:db8:2::3**. En **longitud del prefijo de subred**, tipo **64**. En **puerta de enlace predeterminada**, tipo **2001:db8:2::fe**. Haga clic en **usar las siguientes direcciones de servidor DNS**y en **servidor DNS preferido**, tipo **2001:db8:2::1**.  
  
9. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.  
  
10. En **sufijo DNS para esta conexión**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar** dos veces.  
  
11. En el **propiedades de conexión cableada Ethernet** haga clic en el cuadro de diálogo **cerrar**.  
  
12. Cierre la ventana **Conexiones de red**.  
  
## <a name="bkmk_JoinDomain"></a>Una 2-APP1 al dominio CORP2  
Únase a 2-APP1 al dominio corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Para unir 2-APP1 al dominio CORP2  
  
1.  En la consola de administrador del servidor, en **servidor Local**, en el **propiedades** área, junto a **nombre_equipo**, haga clic en el vínculo.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
3.  En **nombre_equipo**, tipo **2-APP1**. En **miembro de**, haga clic en **dominio**, tipo **corp2.corp.contoso.com**y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando se le pida un nombre de usuario y contraseña, escriba **administrador** y su contraseña y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo que le da la bienvenida al dominio corp2.corp.contoso.com, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
9. Una vez reiniciado el equipo, haga clic en **cambiar de usuario**y, a continuación, haga clic en **otro usuario** e inicie sesión en el dominio CORP2 con la cuenta de administrador.  
  
## <a name="bkmk_IIS"></a>Instalar el rol de servidor Web (IIS) en 2-APP1  
Instalar el rol servidor Web (IIS) para que un servidor de 2-APP1.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Para instalar el rol servidor Web (IIS)  
  
1.  En la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor  
  
3.  En el **seleccionar roles de servidor** , seleccione **servidor Web (IIS)** y, a continuación, haga clic en **siguiente** cuatro veces.  
  
4.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
5.  Compruebe que la instalación se realizó correctamente y, a continuación, haga clic en **cerrar**.  
  
## <a name="bkmk_Share"></a>Cree una carpeta compartida en 2-APP1  
Cree una carpeta compartida y un archivo de texto dentro de la carpeta en APP1 de 2.  
  
#### <a name="to-create-a-shared-folder"></a>Para crear una carpeta compartida  
  
1.  En el **iniciar** , escriba**explorer.exe**, y, a continuación, presione ENTRAR.  
  
2.  Haga clic en **equipo**, a continuación, haga doble clic en **disco Local (C:)**.  
  
3.  Haga clic en **nueva carpeta**, tipo **archivos**, y, a continuación, presione ENTRAR. Deje el **disco Local** abierta la ventana.  
  
4.  En el **iniciar** , escriba**notepad.exe**, haga clic en **el Bloc de notas**, haga clic en **avanzadas**y, a continuación, haga clic en **ejecutar como administrador**.  
  
5.  En el **sin título - Bloc de notas** ventana, escriba **se trata de un archivo compartido en 2-APP1**.  
  
6.  Haga clic en **archivo**, haga clic en **guardar**, haga clic en **equipo**, haga doble clic en **disco Local (C:)** y, a continuación, haga doble clic en el **archivos**  carpeta.  
  
7.  En **nombre de archivo**, tipo **example.txt**y, a continuación, haga clic en **guardar**. Cierre el Bloc de notas.  
  
8.  En el **disco Local** ventana, haga clic en el **archivos** carpeta, seleccione **compartir con**y, a continuación, haga clic en **personas específicas**.  
  
9. En el **uso compartido de archivos** cuadro de diálogo, en la lista desplegable, haga clic en **todo el mundo**y, a continuación, haga clic en **agregar**. En **nivel de permiso** para **todo el mundo**, haga clic en **lectura/escritura**.  
  
10. Haga clic en **Share**y, a continuación, haga clic en **realiza**.  
  
11. Cerrar la **disco Local** ventana.  
  


