---
title: Trabajar de forma remota en Windows Server Essentials
description: Obtenga información acerca de las distintas formas de obtener acceso a los recursos que se encuentran en el servidor cuando está fuera de la red.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 8b183f8f-1279-4fdf-a495-c7c801563cb0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 12e36c7d77a018dded93fd2b8d374a37423fc242
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97809992"
---
# <a name="work-remotely-in-windows-server-essentials"></a>Trabajar de forma remota en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Hay varias maneras de acceder a los recursos que se encuentran en el servidor cuando está fuera de la red si las funciones de Acceso desde cualquier lugar (Acceso web remoto, red privada virtual y DirectAccess) están configuradas en el servidor.

 Los siguientes temas pueden ayudarle a acceder a los recursos del servidor de forma remota:


-   [Administrar Acceso web remoto en Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMA_RWA)

-   [Usar VPN para conectarse a Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_3)

-   [Usar la aplicación My Server para conectarse a Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_App)

-   [Usar la aplicación My Server para Windows Phone.](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)

-   [Usar Microsoft 365 con Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_O365)

> [!NOTE]
>  Para obtener información sobre cómo configurar el acceso desde cualquier lugar en el servidor, consulte [administrar el acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md).

##  <a name="use-remote-web-access-in-windows-server-essentials"></a><a name="BKMA_RWA"></a> Usar acceso Web remoto en Windows Server Essentials

 El Acceso web remoto le permite estar conectado a la red de Windows Server Essentials cuando lo utilice. Para obtener más información, vea el tema [usar acceso Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).


##  <a name="use-vpn-to-connect-to-windows-server-essentials"></a><a name="BKMK_3"></a> Usar VPN para conectarse a Windows Server Essentials
 Si tiene un equipo cliente configurado con cuentas de red que pueden usarse para conectarse a un servidor hospedado que ejecuta Windows Server Essentials a través de una conexión VPN, todas las cuentas de usuario recién creadas en el servidor hospedado deben usar VPN para iniciar sesión en el equipo cliente por primera vez. Complete el procedimiento siguiente en el equipo cliente que está conectado al servidor.

#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Usar VPN para acceder de forma remota a los recursos del servidor

1.  Presione Ctrl + Alt + Supr en el equipo cliente.

2.  Haga clic en **Cambiar usuario** en la pantalla de inicio de sesión.

3.  Haga clic en el icono de inicio de sesión de red, en la esquina inferior derecha de la pantalla.

4.  Inicie sesión en la red de Windows Server Essentials usando su nombre de usuario y su contraseña de red.

##  <a name="use-the-my-server-app-to-connect-to-windows-server-essentials"></a><a name="BKMK_App"></a> Usar la aplicación My Server para conectarse a Windows Server Essentials
 La aplicación My Server le permite conectarse a los recursos y realizar tareas administrativas ligeras en su servidor de Windows Server Essentials desde su equipo, portátil o dispositivo Surface basado en Windows. Si el servidor ejecuta Windows Server 2012, descargue la aplicación My Server original de las [aplicaciones para Windows](https://windows.microsoft.com/windows-8/apps). Si el servidor ejecuta Windows Server Essentials, debe descargar en su lugar la aplicación My Server 2012 R2.

 Con la aplicación Mi servidor 2012 R2 expandida, puede conectarse al servidor o a los equipos cliente mediante el Escritorio remoto. Si el servidor de Windows Server Essentials está integrado con Microsoft 365 y la suscripción incluye SharePoint Online, también puede trabajar con documentos en las bibliotecas de SharePoint Online y abrir los sitios de grupo de SharePoint desde My Server 2012 R2.


 Para obtener información sobre la instalación y el uso de estas aplicaciones, consulte [usar la aplicación My Server](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).

 Para obtener información sobre la instalación y el uso de estas aplicaciones, consulte [usar la aplicación My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).


##  <a name="use-the-my-server-app-for-windows-phone"></a><a name="BKMK_2"></a> Usar la aplicación My Server para Windows Phone
 La aplicación My Server Windows para Windows Phone (para Windows Server 2012) y la aplicación My Server 2012 R2 para Windows Phone (para Windows Server Essentials) están diseñadas para ayudarle a mantenerse conectado a los servidores a través de teléfonos inteligentes mientras trabaja en ubicaciones remotas. Esta es una de las distintas formas de obtener acceso a Windows Server Essentials después de configurar el servidor para el acceso remoto.

 Puede descargar cualquier aplicación desde la Tienda de Windows Phone:

- [Instalar Mi servidor para Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)

- [Instalar Mi servidor 2012 R2 para Windows Phone](http://www.windowsphone.com/store/app/my-server-2012-r2/44f596b5-0477-4096-b96e-ddd6ef64ad6b)

  Para obtener más información acerca de la aplicación de teléfono My Server, consulte la entrada de blog sobre la [aplicación de teléfono My Server para Windows Server Essentials](/archive/blogs/sbs/my-server-phone-app-for-windows-server-2012-essentials). Para obtener más información sobre la aplicación de teléfono My Server 2012 R2, consulte la entrada de blog [Aplicaciones My Server 2012 R2 de Windows y Windows Phone](/archive/blogs/sbs/my-server-2012-r2-windows-and-windows-phone-apps).

##  <a name="use-microsoft-365-with-windows-server-essentials"></a><a name="BKMK_O365"></a> Usar Microsoft 365 con Windows Server Essentials

 Microsoft 365 es un conjunto de herramientas habilitadas para Web fácil de usar que le permiten acceder a su correo electrónico, documentos importantes, contactos y calendario desde prácticamente cualquier lugar y cualquier dispositivo. Para obtener más información, vea la [Guía de inicio rápido a usar Microsoft 365](Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).


## <a name="see-also"></a>Consulte también

-   [Administrar Acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Conéctese](Get-Connected-in-Windows-Server-Essentials.md)

-   [Usar carpetas compartidas](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Reproducir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md)