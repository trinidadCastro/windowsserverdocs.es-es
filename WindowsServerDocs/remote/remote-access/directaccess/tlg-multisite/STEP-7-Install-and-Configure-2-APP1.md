---
title: 'Paso 7: instalación y configuración 2-APP1'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 36973d822f607ac6a62fffc956f1dacee68be60a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947981"
---
# <a name="step-7-install-and-configure-2-app1"></a>Paso 7: instalación y configuración 2-APP1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

2-APP1 proporciona servicios de uso compartido de archivos y Web. 2-la configuración de APP1 consta de lo siguiente:

- Instalar el sistema operativo en 2-APP1

- Configurar las propiedades TCP/IP

- Unir 2-APP1 al dominio CORP2

- Instalar el rol de servidor Web (IIS) en 2-APP1

- Crear una carpeta compartida en 2-APP1

## <a name="install-the-operating-system-on-2-app1"></a><a name="bkmk_InstallOS"></a>Instalar el sistema operativo en 2-APP1
En primer lugar, instale Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

#### <a name="to-install-the-operating-system-on-2-app1"></a>Para instalar el sistema operativo en 2-APP1

1.  Inicie la instalación de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (instalación completa).

2.  Siga las instrucciones para completar la instalación, especificando una contraseña segura para la cuenta Administrador local. Inicie sesión con la cuenta Administrador local.

3.  Conecte 2-APP1 a una red que tenga acceso a Internet y ejecute Windows Update para instalar las actualizaciones más recientes para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y, a continuación, desconéctese de Internet.

4.  Conecte 2-APP1 a la subred 2-CorpNet.

## <a name="configure-tcpip-properties"></a><a name="bkmk_TCP"></a>Configurar las propiedades TCP/IP
Configure las propiedades TCP/IP en 2-APP1.

#### <a name="to-configure-tcpip-properties"></a>Para configurar las propiedades TCP/IP

1.  En la consola de Administrador del servidor, haga clic en **servidor local** y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.

2.  En la ventana **conexiones de red** , haga clic con el botón secundario en **conexión cableada Ethernet** y, a continuación, haga clic en **propiedades**.

3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.

4.  Haga clic en **Usar la siguiente dirección IP**. En **dirección IP**, escriba **10.2.0.3**. En **Máscara de subred**, escriba **255.255.255.0**. En **puerta de enlace predeterminada**, escriba **10.2.0.254**.

5.  Haga clic en **Usar las siguientes direcciones de servidor DNS**. En **servidor DNS preferido**, escriba **10.2.0.1**.

6.  Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS** . En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com** y haga clic en **Aceptar** dos veces.

7.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.

8.  Haga clic en **usar la siguiente dirección IPv6**. En **dirección IPv6**, escriba **2001: db8:2:: 3**. En **longitud del prefijo de subred**, escriba **64**. En **puerta de enlace predeterminada**, escriba **2001: db8:2:: fe**. Haga clic en **usar las siguientes direcciones de servidor DNS** y, en **servidor DNS preferido**, escriba **2001: db8:2:: 1**.

9. Haga clic en **Opciones avanzadas** y, a continuación, haga clic en la pestaña **DNS**.

10. En **sufijo DNS para esta conexión**, escriba **Corp2.Corp.contoso.com** y, a continuación, haga clic en **Aceptar** dos veces.

11. En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **cerrar**.

12. Cierre la ventana **Conexiones de red**.

## <a name="join-2-app1-to-the-corp2-domain"></a><a name="bkmk_JoinDomain"></a>Unir 2-APP1 al dominio CORP2
Únase 2-APP1 al dominio corp2.corp.contoso.com.

#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Para unir 2-APP1 al dominio CORP2

1.  En la consola de Administrador del servidor, en **servidor local**, en el área **propiedades** , junto a **nombre de equipo**, haga clic en el vínculo.

2.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.

3.  En **nombre de equipo**, escriba **2-app1**. En **miembro de**, haga clic en **dominio**, escriba **Corp2.Corp.contoso.com** y, a continuación, haga clic en **Aceptar**.

4.  Cuando se le pida un nombre de usuario y una contraseña, escriba **Administrador** y su contraseña y, a continuación, haga clic en **Aceptar**.

5.  Cuando vea un cuadro de diálogo que le muestra el dominio corp2.corp.contoso.com, haga clic en **Aceptar**.

6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.

7.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.

8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.

9. Una vez reiniciado el equipo, haga clic en **cambiar de usuario** y, a continuación, haga clic en **otro usuario** e inicie sesión en el dominio CORP2 con la cuenta de administrador.

## <a name="install-the-web-server-iis-role-on-2-app1"></a><a name="bkmk_IIS"></a>Instalar el rol de servidor Web (IIS) en 2-APP1
Instale el rol de servidor Web (IIS) para hacer que 2-APP1 sea un servidor Web.

#### <a name="to-install-the-web-server-iis-role"></a>Para instalar el rol servidor Web (IIS)

1.  En la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.

2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor

3.  En la página **Seleccionar roles de servidor** , seleccione **servidor Web (IIS)** y, a continuación, haga clic en **siguiente** cuatro veces.

4.  En la página **Confirmar selecciones de instalación**, haga clic en **Instalar**.

5.  Compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **cerrar**.

## <a name="create-a-shared-folder-on-2-app1"></a><a name="bkmk_Share"></a>Crear una carpeta compartida en 2-APP1
Cree una carpeta compartida y un archivo de texto dentro de la carpeta en 2-APP1.

#### <a name="to-create-a-shared-folder"></a>Para crear una carpeta compartida

1.  En la pantalla **Inicio** , escriba **explorer.exe** y, a continuación, presione Entrar.

2.  Haga clic en **equipo** y, a continuación, haga doble clic en **disco local (C:)**.

3.  Haga clic en **nueva carpeta**, escriba **archivos** y, a continuación, presione Entrar. Deje abierta la ventana **disco local** .

4.  En la pantalla **Inicio** , escriba **notepad.exe**, haga clic con el botón secundario en **Bloc de notas**, haga clic en **Opciones avanzadas** y, a continuación, haga clic en **Ejecutar como administrador**.

5.  En la ventana sin **Título: Bloc de notas** , escriba **este es un archivo compartido en 2-app1**.

6.  Haga clic en **archivo**, haga clic en **Guardar**, haga clic en **equipo**, haga doble clic en **disco local (C:)** y, a continuación, haga doble clic en la carpeta **archivos** .

7.  En **nombre de archivo**, escriba **example.txt** y, a continuación, haga clic en **Guardar**. Cierre el Bloc de notas.

8.  En la ventana **disco local** , haga clic con el botón secundario en la carpeta **archivos** , seleccione **compartir con** y, a continuación, haga clic en **personas específicas**.

9. En el cuadro de diálogo **uso compartido de archivos** , en la lista desplegable, haga clic en **todos** y, a continuación, haga clic en **Agregar**. En **nivel de permiso** para **todos**, haga clic en **lectura/escritura**.

10. Haga clic en **compartir** y, a continuación, en **listo**.

11. Cierre la ventana **disco local** .



