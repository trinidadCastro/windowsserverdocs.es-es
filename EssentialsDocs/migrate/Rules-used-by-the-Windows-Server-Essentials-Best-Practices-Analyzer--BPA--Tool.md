---
title: Reglas que usa la herramienta Analizador de procedimientos recomendados (BPA) de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850696"
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Reglas que usa la herramienta Analizador de procedimientos recomendados (BPA) de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este artículo se describe las reglas utilizadas por el Windows Server Essentials Best Practices Analyzer (BPA). El BPA examina un servidor que ejecuta Windows Server Essentials y presenta un informe que se describe los problemas y proporciona recomendaciones para resolverlos. Las recomendaciones las desarrolla la organización de soporte técnico para Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Uso de la herramienta  
 Es una práctica estándar, al migrar a Windows Server Essentials desde Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials o Windows Home Server 2011, para ejecutar el BPA en el servidor de destino después de migrar su configuración y los datos. Puede ejecutar la herramienta desde el panel en cualquier momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Para ejecutar el BPA de Windows Server Essentials en el servidor  
  
1.  Inicie sesión en el servidor como administrador y, después, abra el panel.  
  
2.  En el panel, haga clic en la pestaña **Dispositivos**.  
  
3.  En el panel **Tareas de servidor**, haga clic en **Analizador de procedimientos recomendados**.  
  
4.  Revise cada mensaje del BPA y siga las instrucciones para resolver problemas si fuera necesario.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Reglas usadas por el Analizador de procedimientos recomendados  
  
### <a name="disable-ip-filtering"></a>Deshabilitar el filtrado de IP  
 **Problema:** El filtrado de IP está habilitado actualmente en el servidor. Debe deshabilitar el filtrado de IP.  
  
 **Impacto:** Si está habilitado el filtrado de IP, el tráfico de red podría bloquearse.  
  
 **Solución:**  
  
##### <a name="to-disable-ip-filtering"></a>Para deshabilitar el filtrado de IP  
  
1.  Abra regedit.exe en el servidor.  
  
2.  Vaya a HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Haga clic con el botón derecho en **EnableSecurityFilters** y, a continuación, haga clic en **Modificar**.  
  
4.  En la ventana **Editar valor DWORD (32 bits)**, cambie el campo **Datos del valor** a cero y, a continuación, haga clic en **Aceptar**.  
  
5.  Para aplicar el cambio, reinicie el servidor.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>El servicio Coordinador de transacciones distribuidas (MSDTC) debe establecerse para que se inicie automáticamente de forma predeterminada  
 **Problema:** El servicio MSDTC no está configurado para iniciarse automáticamente.  
  
 **Impacto:** El servicio MSDTC podría no iniciarse automáticamente cuando se inicia el servidor. Si el servicio está detenido, algunas funciones de SQL Server o COM pueden producir un error. Como resultado, las aplicaciones que usan las funciones de Microsoft SQL Server o COM podrían no funcionar correctamente.  
  
 **Solución:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Para configurar el servicio MSDTC de modo que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Coordinador de transacciones distribuidas** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General**, cambie **Tipo de inicio** a **Automático (inicio retrasado)** y haga clic en **Aceptar**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de Net Logon debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** El servicio de Net Logon no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  El servicio de Net Logon podría no iniciarse automáticamente cuando se inicia el servidor. Si el servicio está detenido, el servidor podría no autenticar a los usuarios y los servicios.  
  
 **Solución:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Para configurar el servicio de Net Logon para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio de **Net Logon** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>El servicio Cliente DNS debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio Cliente DNS no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  El servicio Cliente DNS podría no iniciarse automáticamente cuando se inicia el servidor. Si este servicio está detenido, el servidor podría no resolver nombres DNS.  
  
 **Solución:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Para configurar el servicio Cliente DNS para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DNS** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>El servicio Servidor DNS debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio Servidor DNS no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  El servicio Servidor DNS podría no iniciarse automáticamente cuando se inicia el servidor. Si este servicio está detenido, no se producirán actualizaciones de DNS.  
  
 **Solución:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Para configurar el servicio Servidor DNS para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Servidor DNS** y seleccione **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Servicios web de Active Directory no está establecido en el modo de inicio predeterminado  
 **Problema:**  Servicios web de Active Directory no está establecido en el modo de inicio predeterminado automático.  
  
 **Impacto:**  Servicios web de Active Directory (ADWS) no está establecido en el modo de inicio predeterminado automático. Si ADWS está detenido o deshabilitado en el servidor, las aplicaciones cliente, como el módulo de Active Directory para Windows PowerShell o el Centro de administración de Active Directory, no pueden acceder a las instancias del servicio de directorio que se ejecutan en este servidor, ni tampoco administrarlas. Para obtener más información, consulte [Novedades en AD DS: Servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Solución:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Para configurar Servicios web de Active Directory para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Servicios web de Active Directory** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>El servicio Cliente DHCP debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio Cliente DHCP no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  El servicio Cliente DHCP no se iniciará automáticamente cuando se inicia el servidor. Si este servicio está detenido, los equipos cliente no pueden recibir una dirección IP del servidor.  
  
 **Solución:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Para configurar el servicio Cliente DHCP para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DHCP** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>El Servicio de administración IIS debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** El Servicio de administración IIS no está configurado para iniciarse automáticamente.  
  
 **Impacto:** El Servicio de administración IIS no se iniciará automáticamente cuando se inicia el servidor. Si este servicio está detenido, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Para configurar el Servicio de administración IIS para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de administración IIS** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de publicación World Wide Web debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio de publicación World Wide Web no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  El servicio de publicación World Wide Web podría no iniciarse automáticamente cuando se inicia el servidor. Si este servicio está detenido, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Para configurar el servicio de publicación World Wide Web para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de publicación World Wide Web** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de Registro remoto debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio de Registro remoto no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  
  
 El servicio de Registro remoto podría no iniciarse automáticamente cuando se inicia el servidor. Si este servicio está detenido, es posible que no pueda realizar algunas operaciones de red de forma remota.  
  
 **Solución:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Para configurar el servicio de Registro remoto para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio de **Registro remoto** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>El servicio Puerta de enlace de Escritorio remoto debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio Puerta de enlace de Escritorio remoto no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  Si este servicio está detenido, los usuarios podrían no tener acceso a los equipos mediante el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Para configurar el servicio Puerta de enlace de Escritorio remoto para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Puerta de enlace de Escritorio remoto** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General**, cambie **Tipo de inicio** a **Automático (inicio retrasado)** y haga clic en **Aceptar**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>El servicio Hora de Windows debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:**  El servicio Hora de Windows no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  Si este servicio está detenido, la sincronización de fecha y hora no estará disponible.  
  
 **Solución:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Para configurar el servicio Hora de Windows para que se inicie automáticamente  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Hora de Windows** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **General** , cambie **Tipo de inicio** a **Automático**y haga clic en **Aceptar**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Debe iniciarse el servicio Coordinador de transacciones distribuidas (MSDTC)  
 **Problema:**  El servicio MSDTC no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, algunas funciones de SQL Server o COM pueden producir un error. Como resultado, las aplicaciones que usan las funciones de Microsoft SQL Server o COM podrían no funcionar correctamente.  
  
 **Solución:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar el servicio Coordinador de transacciones distribuidas  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Coordinador de transacciones distribuidas** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Debe iniciarse el servicio de Net Logon  
 **Problema:**  El servicio de Net Logon no se está ejecutando en el servidor.  
  
 **Impacto:**  Si no se inicia el servicio, el servidor podría no autenticar a los usuarios y los servicios.  
  
 **Solución:**  
  
##### <a name="to-start-the-netlogon-service"></a>Para iniciar el servicio de Net Logon  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio de **Net Logon** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Debe iniciarse el servicio Cliente DNS  
 **Problema:**  El servicio Cliente DNS no se está ejecutando en el servidor.  
  
 **Impacto:**  Si no se inicia este servicio, el servidor podría no resolver nombres DNS.  
  
 **Solución:**  
  
##### <a name="to-start-the-dns-client-service"></a>Para iniciar el servicio Cliente DNS  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DNS** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Debe iniciarse el servicio Servidor DNS  
 **Problema:**  El servicio Servidor DNS no se está ejecutando en el servidor.  
  
 **Impacto:**  Si no se inicia el servicio Servidor DNS, podrían no producirse las actualizaciones de DNS.  
  
 **Solución:**  
  
##### <a name="to-start-the-dns-server-service"></a>Para iniciar el servicio Servidor DNS  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Servidor DNS** y seleccione **Inicio**.  
  
### <a name="active-directory-web-services-is-not-started"></a>No se inició Servicios web de Active Directory  
 **Problema:**  No se inició Servicios web de Active Directory.  
  
 **Impacto:**  No se inició Servicios web de Active Directory (ADWS). Si ADWS está detenido o deshabilitado en el servidor, las aplicaciones cliente, como el módulo de Active Directory para Windows PowerShell o el Centro de administración de Active Directory, no pueden acceder a las instancias del servicio de directorio que se ejecutan en este servidor, ni tampoco administrarlas. Para obtener más información, consulte [Novedades en AD DS: Servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Solución:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Para iniciar el servicio Servicios web de Active Directory  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicios web de Active Directory** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Debe iniciarse el servicio Cliente DHCP  
 **Problema:**  El servicio Cliente DHCP no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, los equipos cliente no pueden recibir una dirección IP del servidor.  
  
 **Solución:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Para iniciar el servicio Cliente DHCP  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DHCP** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Debe iniciarse el Servicio de administración IIS  
 **Problema:**  El Servicio de administración IIS no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Para iniciar el Servicio de administración IIS  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de administración IIS** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Debe iniciarse el servicio de publicación World Wide Web  
 **Problema:**  El servicio de publicación World Wide Web no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Para iniciar el servicio de publicación World Wide Web  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de publicación World Wide Web** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Debe iniciarse el servicio Puerta de enlace de Escritorio remoto  
 **Problema:**  El servicio Puerta de enlace de Escritorio remoto no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, los usuarios podrían no tener acceso a los equipos mediante el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Para iniciar el servicio de puerta de enlace de escritorio remoto  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Puerta de enlace de Escritorio remoto** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Debe iniciarse el servicio Hora de Windows  
 **Problema:**  El servicio Hora de Windows no se está ejecutando en el servidor.  
  
 **Impacto:**  Si este servicio está detenido, la sincronización de fecha y hora no estará disponible.  
  
 **Solución:**  
  
##### <a name="to-start-the-windows-time-service"></a>Para iniciar el servicio de hora de Windows  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Hora de Windows** y, a continuación, haga clic en **Inicio**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>La cuenta de inicio de sesión del servicio Coordinador de transacciones distribuidas (MSDTC) debe ser NT AUTHORITY\Network Service  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Coordinador de transacciones distribuidas (MSDTC).  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, las aplicaciones que usan las funciones de SQL Server o COM no funcionen correctamente.  
  
 **Solución:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Para cambiar la cuenta de inicio de sesión del servicio  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Coordinador de transacciones distribuidas** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Esta cuenta**, escriba **NT AUTHORITY\Network Service** y haga clic en **Aceptar**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de Net Logon debe usar la cuenta de sistema local como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio de Net Logon.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, el servidor podría no autenticar a los usuarios y los servicios.  
  
 **Solución:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de Net Logon  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio de **Net Logon** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Cuenta de sistema local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio Cliente DNS debe usar la cuenta NT AUTHORITY\Network Service como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Cliente DNS.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, el servidor podría no resolver nombres DNS.  
  
 **Solución:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Cliente DNS  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DNS** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Esta cuenta** y escriba **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio servidor DNS debe usar la cuenta de sistema Local como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Servidor DNS.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, podrían no producirse las actualizaciones de DNS.  
  
 **Solución:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Servidor DNS  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Servidor DNS** y seleccione **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Cuenta de sistema local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Servicios web de Active Directory no es la cuenta de inicio de sesión predeterminada  
 **Problema:**  Servicios web de Active Directory no es la cuenta de inicio de sesión predeterminada. De forma predeterminada, la cuenta de inicio de sesión se establece en **Cuenta de sistema local**.  
  
 **Impacto:**  No se inició Servicios web de Active Directory (ADWS). Si ADWS está detenido o deshabilitado en el servidor, las aplicaciones cliente, como el módulo de Active Directory para Windows PowerShell o el Centro de administración de Active Directory, no pueden acceder a las instancias del servicio de directorio que se ejecutan en este servidor, ni tampoco administrarlas. Para obtener más información, consulte [Novedades en AD DS: Servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Solución:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Para cambiar la cuenta de inicio de sesión de Servicios web de Active Directory  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicios web de Active Directory** y, a continuación, haga clic en **Propiedades**.  
  
3.  Cambie **Tipo de inicio** a **Automático** y haga clic en **Aceptar**.  
  
4.  En **Propiedades** de Servicios web de Active Directory, haga clic en la pestaña **Iniciar sesión**.  
  
5.  Seleccione la opción **Cuenta de sistema local** y haga clic en **Aceptar**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio Windows Update debe usar la cuenta de sistema local como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio de Actualizaciones automáticas.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, es posible que el servidor no reciba actualizaciones automáticas.  
  
 **Solución:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Windows Update  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Windows Update** y, a continuación, haga clic en Propiedades.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Cuenta de sistema local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>El servicio Cliente DHCP debe usar la cuenta NT AUTHORITY\LocalService como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Cliente DHCP.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, el equipo cliente no recibirá direcciones IP del servidor.  
  
 **Solución:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Cliente DHCP  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Cliente DHCP** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Esta cuenta** y escriba **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>El Servicio de administración IIS debe usar la cuenta de sistema local como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del Servicio de administración IIS.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-change-the-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de administración IIS** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Cuenta de sistema local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de publicación World Wide Web debe usar la cuenta de sistema local como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio de publicación World Wide Web.  
  
 **Impacto:**  El servicio podría no tener los permisos necesarios para que funcione según lo previsto. Como resultado, podría no tener acceso a sitios web que se ejecutan en el servidor, como el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de publicación World Wide Web  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en **Servicio de publicación World Wide Web** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Cuenta de sistema local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio Puerta de enlace de Escritorio remoto debe usar la cuenta NT AUTHORITY\Network Service como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Puerta de enlace de Escritorio remoto.  
  
 **Impacto:**  El servicio podría no tener los permisos apropiados para que funcione según lo previsto. Como resultado, los usuarios podrían no tener acceso a los equipos mediante el acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Puerta de enlace de Escritorio remoto  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Puerta de enlace de Escritorio remoto** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Esta cuenta** y escriba **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio Hora de Windows debe usar la cuenta NT AUTHORITY\Network Service como cuenta de inicio de sesión  
 **Problema:**  Se cambió la cuenta de inicio de sesión predeterminada del servicio Hora de Windows.  
  
 **Impacto:**  El servicio podría no tener los permisos apropiados para que funcione según lo previsto. Como resultado, la sincronización de fecha y hora podría no estar disponible.  
  
 **Solución:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio Hora de Windows  
  
1.  Abra services.msc en el servidor.  
  
2.  Haga clic con el botón derecho en el servicio **Hora de Windows** y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Iniciar sesión**, seleccione **Esta cuenta** y escriba **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>El grupo Administradores integrado no tiene derecho a iniciar sesión como un trabajo por lotes  
 **Problema:**  El grupo Administradores integrado no tiene derecho a iniciar sesión como un trabajo por lotes.  
  
 **Impacto:**  Si el administrador crea una alerta y la configura para que se ejecute cuando el administrador no haya iniciado sesión, la alerta producirá un error con un código de error 2147943785.  
  
 **Solución:**  Para obtener información acerca de cómo proporcionar a los administradores integrados grupo permiso para iniciar sesión como trabajo por lotes, vea [conceder al grupo Administradores integrado el derecho a iniciar sesión como trabajo por lotes](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>Firewall de Windows está desactivado  
 **Problema:**  Firewall de Windows está desactivado. El valor predeterminado es activado.  
  
 **Impacto:**  En función de su configuración, Firewall de Windows puede ayudar a proteger el servidor y la red frente a actividades malintencionadas, al impedir que cierta información pase a través del servidor.  
  
 **Solución:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Para activar Firewall de Windows en el servidor  
  
1.  Abra el Panel de control en el servidor.  
  
2.  En el Panel de control, haga clic en **Sistema y seguridad** y después haga clic en **Firewall de Windows**.  
  
3.  En Firewall de Windows, haga clic en **Activar o desactivar Firewall de Windows**, seleccione la opción **Activar Firewall de Windows** y, a continuación, haga clic en **Aceptar**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>El adaptador de red interno no está configurado para registrar la dirección IP en DNS  
 **Problema:**  El adaptador de red interno no está configurado para registrar su dirección IP en DNS.  
  
 **Impacto:**  Si la dirección IP del adaptador de red interno no está registrada en DNS, no sería posible tener acceso al servidor utilizando el nombre del equipo servidor.  
  
 **Solución:**  Compruebe que el adaptador de red interno está configurado para registrar en DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: Los valores de la clave del Registro RecursionTimeout y ForwardingTimeout de DNS son idénticos  
 **Problema:**  El valor de la clave del Registro ForwardingTimeout de DNS no debe ser igual al valor de la clave del Registro RecursionTimeout.  
  
 **Impacto:**  Podría no tener acceso a recursos de Internet por su nombre.  
  
 **Solución:**  Establezca el valor de la clave del Registro RecursionTimeout para que sea mayor que el valor de la clave ForwardingTimeout, ubicada en el Registro en HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zona directa de DNS para su dominio de Active Directory no permite actualizaciones seguras  
 **Problema:**  Debe configurar la zona de búsqueda directa para permitir solo actualizaciones dinámicas seguras.  
  
 **Impacto:**  Al habilitar las actualizaciones dinámicas seguras, solo los usuarios y hosts autorizados pueden realizar cambios en los registros.  
  
 **Solución:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Para configurar la zona de búsqueda directa de su dominio de Active Directory  
  
1.  Abra dnsmgmt.msc en el servidor.  
  
2.  Haga clic con el botón derecho en la zona de búsqueda directa de su dominio de Active Directory y, a continuación, haga clic en **Propiedades**.  
  
3.  En la lista desplegable **Actualizaciones dinámicas**, seleccione **Solo con seguridad** y haga clic en **Aceptar**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>La zona directa de DNS no permite las actualizaciones seguras  
 **Problema:**  Debe configurar la zona de búsqueda directa para que la zona _msdcs.* solo permita las actualizaciones dinámicas seguras.  
  
 **Impacto:**  Al habilitar las actualizaciones dinámicas seguras, solo los usuarios y hosts autorizados pueden realizar cambios en los registros en la zona _msdcs.*.  
  
 **Solución:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Para permitir actualizaciones seguras en la zona _msdcs  
  
1.  Abra dnsmgmt.msc en el servidor.  
  
2.  Haga clic con el botón derecho en la zona de búsqueda directa de la zona _msdcs y, a continuación, haga clic en **Propiedades**.  
  
3.  En la lista desplegable **Actualizaciones dinámicas**, seleccione **Solo con seguridad** y haga clic en **Aceptar**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>La Configuración de seguridad mejorada de Internet Explorer no está habilitada  
 **Problema:**  Configuración de seguridad mejorada de Internet Explorer (IE ESC) no está habilitada actualmente para el grupo de administradores.  
  
 **Impacto:**  Si la Configuración de seguridad mejorada de Internet Explorer no está habilitada para el grupo Administradores, su servidor e Internet Explorer estarán más expuestos a ataques malintencionados que pueden producirse a través de contenido web y scripts de aplicaciones.  
  
 **Solución:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar la Configuración de seguridad mejorada de Internet Explorer  
  
1.  Abra **Administrador del servidor** en el servidor y haga clic en **Servidor local**.  
  
2.  En el panel **Propiedades**, cambie la configuración de **Configuración de seguridad mejorada de Internet Explorer** a **Activada** y haga clic en **Aceptar**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>La Configuración de seguridad mejorada de Internet Explorer no está habilitada  
 **Problema:**  Configuración de seguridad mejorada de Internet Explorer (IE ESC) no está habilitada actualmente para el grupo de usuarios.  
  
 **Impacto:**  Si la Configuración de seguridad mejorada de Internet Explorer no está habilitada para el grupo Usuarios, su servidor e Internet Explorer estarán más expuestos a ataques malintencionados que pueden producirse a través de contenido web y scripts de aplicaciones.  
  
 **Solución:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar la Configuración de seguridad mejorada de Internet Explorer  
  
1.  Abra **Administrador del servidor** y haga clic en **Servidor local**.  
  
2.  En el panel **Propiedades**, cambie la configuración de **Configuración de seguridad mejorada de Internet Explorer** a **Activada** y haga clic en **Aceptar**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>El servidor de origen permanece en Sitios y servicios de Active Directory  
 **Problema:**  El servidor de origen que ejecuta Windows Small Business Server sigue existiendo en Sitios y servicios de Active Directory en Default-First-Site-Name.  
  
 **Impacto:**  Si el servidor de origen permanece en sitios de Active Directory y servicios, los equipos cliente pueden experimentar el problema de conectividad: s.  
  
 **Solución:**  Debe disminuir el nivel del servidor de origen, quitarlo del dominio, y eliminar el servidor de origen de Sitios y servicios de Active Directory y de Usuarios y equipos de Active Directory.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>El servidor de origen permanece en la unidad organizativa SBSComputer  
 **Problema:**  El servidor de origen que ejecuta Windows Small Business Server sigue existiendo en Usuarios y equipos de Active Directory.  
  
 **Impacto:**  Si el servidor de origen permanece en equipos y usuarios de Active Directory, los equipos cliente pueden experimentar el problema de conectividad: s.  
  
 **Solución:**  Debe disminuir el nivel del servidor de origen, quitarlo del dominio, y eliminar el servidor de origen de Sitios y servicios de Active Directory y de Usuarios y equipos de Active Directory.  
  
### <a name="a-group-policy-is-missing"></a>Falta una directiva de grupo  
 **Problema:**  Falta la directiva de grupo Default Domain Policy.  
  
 **Impacto:**  Default Domain Policy es necesaria para las funciones de dominio adecuadas.  
  
 **Solución:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Para restaurar una directiva de grupo que falta  
  
1.  Abra gpmc.msc en el servidor.  
  
2.  En el Administrador de directivas de grupo, expanda el bosque de dominio y busque en el árbol de consola el objeto de directiva de grupo **Default Domain Policy**.  
  
3.  Si la directiva no aparece en el árbol, restáurela a partir de una copia de seguridad del estado de sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>No hay registros de recursos del servidor de nombres DNS  
 **Problema:**  No hay ningún registro de recursos del servidor de nombres (NS) DNS en la zona de búsqueda directa del servidor.  
  
 **Impacto:**  Si no existe ningún registro de recursos del servidor de nombres (NS) DNS en la zona de búsqueda directa del dominio de Active Directory, los usuarios podrían no tener acceso a recursos de la red o de Internet.  
  
 **Solución:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Para restaurar los registros de recursos del servidor de nombres DNS que faltan  
  
1.  Abra dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, haga clic con el botón derecho en la zona de búsqueda directa de su dominio de Active Directory y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Servidores de nombres**, compruebe que la configuración sea correcta.  
  
4.  Realice los cambios necesarios y haga clic en **Aceptar** para guardar la configuración.  
  
### <a name="no-dns-name-server-records"></a>No hay registros del servidor de nombres DNS  
 **Problema:**  No hay ningún registro de recursos del servidor de nombres (NS) DNS en la zona _msdcs del servidor (por ejemplo, _msdcs.contoso.local).  
  
 **Impacto:**  Si no existe ningún registro de recursos del servidor de nombres (NS) DNS en la zona _msdcs del dominio de Active Directory, los usuarios podrían no tener acceso a recursos de la red o de Internet.  
  
 **Solución:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Para restaurar los registros del servidor de nombres DNS que faltan  
  
1.  Abra dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, haga clic con el botón derecho en la zona de búsqueda directa de la zona _msdcs y, a continuación, haga clic en **Propiedades**.  
  
3.  En la pestaña **Servidores de nombres**, compruebe que la configuración sea correcta.  
  
4.  Realice los cambios necesarios y haga clic en **Aceptar** para guardar la configuración.  
  
### <a name="no-dns-name-server-records"></a>No hay registros del servidor de nombres DNS  
 **Problema:**  No hay ningún registro de recursos del servidor de nombres (NS) DNS de la zona de búsqueda directa _msdcs delegada.  
  
 **Impacto:**  Si no hay ningún registro de recursos del servidor de nombres (NS) DNS de la zona de búsqueda directa _msdcs delegada, el servicio Servidor DNS no puede resolver los registros de recursos DNS del dominio y no se iniciará.  
  
 **Solución:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Para reconfigurar los registros del servidor de nombres DNS que faltan  
  
1.  Abra dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, expanda el nombre del servidor y, después, expanda **Zonas de búsqueda directa**.  
  
3.  Haga clic en la zona de búsqueda directa de su dominio de Active Directory (por ejemplo, contoso.local).  
  
4.  La zona _msdcs delegada aparece como una carpeta atenuada. Haga clic con el botón derecho en la zona _msdcs y, a continuación, haga clic en **Propiedades**.  
  
5.  En la pestaña **Servidores de nombres**, compruebe que la configuración sea correcta.  
  
6.  Realice los cambios necesarios y haga clic en **Aceptar** para guardar la configuración.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Usuarios autenticados no pertenece al grupo de acceso compatible con versiones anteriores a Windows 2000  
 **Problema:**  El grupo Usuarios autenticados no pertenece al grupo de acceso compatible con versiones anteriores a Windows 2000.  
  
 **Impacto:**  Si el grupo Usuarios autenticados integrado no pertenece al grupo de acceso compatible con versiones anteriores a Windows 2000, los usuarios de red pueden encontrarse con errores de tipo "Acceso denegado".  
  
 **Solución:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Para agregar Usuarios autenticados al grupo de acceso compatible con versiones anteriores a Windows 2000  
  
1.  Abra dsa.msc en el servidor.  
  
2.  En la carpeta **Builtin**, haga clic con el botón derecho en **Acceso compatible con versiones anteriores a Windows 2000** y, a continuación, haga clic en **Propiedades**.  
  
3.  Haga clic en **Agregar**, escriba **Usuarios autenticados** y, después, haga clic en **Aceptar** dos veces.  
  
### <a name="dns-client-not-configured"></a>Cliente DNS no configurado  
 **Problema:**  El cliente DNS no está configurado para que solo apunte a la dirección IP interna del servidor.  
  
 **Impacto:**  Si el cliente DNS no está configurado para apuntar solamente a la dirección IP interna del servidor, se puede producir un error en la resolución de nombres DNS.  
  
 **Solución:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Para configurar DNS para que solo apunte a la dirección IP interna de servidor s  
  
1.  En el equipo cliente, abra la página **Propiedades** de la conexión de red.  
  
2.  Asegúrese de que DNS esté configurado para que solo apunte a la dirección IP interna del servidor.  
  
### <a name="default-application-pool-value-changed"></a>Se cambió el valor de grupo de aplicaciones predeterminado  
 **Problema:**  El número máximo de procesos de trabajo del grupo de aplicaciones DefaultAppPool no está establecido en el valor predeterminado de 1.  
  
 **Impacto:**  Es posible que los usuarios no puedan conectarse a los servicios basados en web de Windows Small Business Server.  
  
 **Solución:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Para restablecer el número máximo de procesos de trabajo del grupo de aplicaciones predeterminado  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Grupos de aplicaciones**.  
  
3.  En **Grupos de aplicaciones**, haga clic con el botón derecho en **DefaultAppPool** y, después, haga clic en **Configuración avanzada**.  
  
4.  En **Configuración avanzada**, cambie el valor de **Número máximo de procesos de trabajo** a 1 y, a continuación, haga clic en **Aceptar**.  
  
5.  Cierre **Configuración avanzada**, haga clic con el botón derecho en **DefaultAppPool** y, después, detenga y vuelva a iniciar la aplicación.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>El grupo de aplicaciones para el acceso Web remoto no usa la cuenta predeterminada  
 **Problema:**  El grupo de aplicaciones RemoteAppPool no se está ejecutando con la cuenta predeterminada.  
  
 **Impacto:**  Los usuarios de red podrían no tener acceso al sitio web de acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Para restablecer el grupo de aplicaciones remotas para que use la cuenta predeterminada  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Grupos de aplicaciones**.  
  
3.  En **Grupos de aplicaciones**, haga clic con el botón derecho en **RemoteAppPool** y, después, haga clic en **Configuración avanzada**.  
  
4.  En **Configuración avanzada**, cambie la **Identidad** a **NetworkService** y, a continuación, haga clic en **Aceptar**.  
  
5.  Cierre **Configuración avanzada**, haga clic con el botón derecho en **RemoteAppPool** y, después, detenga y vuelva a iniciar la aplicación.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>El grupo de aplicaciones para el acceso Web remoto no usa la versión predeterminada de .NET Framework  
 **Problema:**  El grupo de aplicaciones RemoteAppPool no se está ejecutando con la versión predeterminada de Microsoft .NET Framework  
  
 **Impacto:**  Los usuarios de red podrían no tener acceso al sitio web de acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Para cambiar la versión de .NET Framework usada por RemoteAppPool  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Grupos de aplicaciones**.  
  
3.  En **Grupos de aplicaciones**, haga clic con el botón derecho en **RemoteAppPool** y, después, haga clic en **Configuración avanzada**.  
  
4.  En **Configuración avanzada**, cambie la **Versión de .NET Framework** a v4.0 y haga clic en **Aceptar**.  
  
5.  Cierre **Configuración avanzada**, haga clic con el botón derecho en **RemoteAppPool** y, después, detenga y vuelva a iniciar la aplicación.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>El archivo RemoteAccess.log tiene un tamaño mayor que 1 GB  
 **Problema:**  Si el tamaño del archivo Remoteaccess.log supera 1 GB, puede experimentar errores por falta de espacio en disco en la unidad del sistema.  
  
 **Impacto:**  Si el archivo Remoteaccess.log es demasiado grande, podría hacer que el problema de espacio libre: s en la unidad C:.  
  
 **Solución:**  Después de realizar la copia de seguridad del servidor, puede eliminar el archivo Remoteaccess.log, que se encuentra en la carpeta %ProgramData%\Microsoft\Windows Server\Logs\WebApps.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>El directorio de registro del sitio web predeterminado tiene un tamaño superior a 1 GB  
 **Problema:**  Si el tamaño de carpeta de registro del sitio Web de predeterminado supera 1 GB, puede experimentar errores por espacio en disco insuficiente en la unidad del sistema.  
  
 **Impacto:**  Si la carpeta de registro del sitio Web de forma predeterminada es demasiado grande, podría causar problema de espacio libre: s en la unidad C:  
  
 **Solución:**  Después de realizar la copia de seguridad del servidor, y mientras el sitio web predeterminado esté detenido, puede eliminar los archivos de registro de la carpeta C:\inetpub\logs\LogFiles\W3SVC1. A continuación, inicie el sitio web predeterminado.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>No hay ningún enlace para SSL en ninguna de las direcciones IP  
 **Problema:**  No hay ningún enlace para la Capa de sockets seguros (SSL) en ninguna de las direcciones IP del servidor.  
  
 **Impacto:**  Si SSL no está enlazado a todas las direcciones IP del servidor, algunos sitios web no estarán disponibles para los usuarios.  
  
 **Solución:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Para enlazar SSL a todas las direcciones IP del servidor  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de ISS, en el panel **Conexiones**, expanda su servidor, expanda **Sitios**, haga clic con el botón derecho en **Sitio web predeterminado** y, a continuación, haga clic en **Modificar enlaces**.  
  
3.  En **Enlaces de sitios**, haga clic en **Agregar** y, a continuación, seleccione la siguiente configuración:  
  
    -   **Tipo** = **https**  
  
    -   **Dirección IP** = **todas sin asignar**  
  
    -   **Puerto** = 443  
  
4.  Seleccione un certificado SSL y haga clic en **Aceptar** para guardar los cambios.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>No hay enlaces para SSL en el sitio web predeterminado  
 **Problema:**  No hay enlaces para SSL en el sitio web predeterminado.  
  
 **Impacto:**  Si SSL no está enlazado al sitio web predeterminado, es posible que algunos sitios web no estén disponibles para los usuarios.  
  
 **Solución:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Para enlazar SSL al sitio web predeterminado  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de ISS, en el panel **Conexiones**, expanda su servidor, expanda **Sitios**, haga clic con el botón derecho en **Sitio web predeterminado** y, a continuación, haga clic en **Modificar enlaces**.  
  
3.  En **Enlaces de sitios**, haga clic en **Agregar** y, a continuación, seleccione las siguientes opciones:  
  
    -   **Tipo** = **https**  
  
    -   **Dirección IP** = **todas sin asignar**  
  
    -   **Puerto** = 443  
  
    > [!NOTE]
    >  Si existe un enlace HTTPS para el puerto 443 para una dirección IP específica, cambie el atributo **Dirección IP** al que enlace a **Todas sin asignar**. La excepción es la dirección IP 127.0.0.1. No cambie el enlace de 127.0.0.1.  
  
4.  Seleccione un certificado SSL y haga clic en **Aceptar** para guardar los cambios.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificado expira en los próximos 30 días  
 **Problema:**  El certificado del servidor expirará en 30 días.  
  
 **Impacto:**  El servidor no puede usar un certificado expirado. Si el certificado expira, es posible que los usuarios no puedan usar las funciones de Acceso desde cualquier lugar.  
  
 **Solución:**  Para evitar que el certificado expire, renuévelo con la entidad de certificación de confianza.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>El firmante del certificado no coincide con el nombre configurado por el Asistente para configurar el nombre de dominio  
 **Problema:**  El firmante del certificado no coincide con el nombre que configuró el Asistente para configurar el nombre de dominio.  
  
 **Impacto:**  Si el firmante del certificado no coincide con el nombre que configuró el Asistente para configurar el nombre de dominio, algunos sitios web no se inicializarán. Otros sitios mostrarán el error "Hay un problema con el certificado de seguridad del sitio Web".  
  
 **Solución:**  Para resolver este problema:, vuelva a ejecutar el Asistente de acceso desde cualquier lugar de configurar y proporcione el nombre de dominio correcto para el certificado o comprar un nuevo certificado que coincida con el nombre de dominio que desea usar.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Una o varias cuentas de usuario tienen nombres CN duplicados  
 **Problema:**  Una o varias cuentas de usuario tienen nombres CN duplicados: {0}.  
  
 **Impacto:**  Si las cuentas de usuario tienen nombres CN duplicados, es posible que los usuarios no puedan iniciar sesión en la red. Además, las búsquedas de Active Directory para los usuarios pueden devolver valores incorrectos.  
  
 **Solución:**  Para resolver este problema:, asegúrese de que las cuentas de usuario de red no tienen duplicado "CN =" nombres. Para facilitar esta tarea, considere la posibilidad de exportar el contenido de Active Directory a un archivo de texto para su revisión. Para obtener información acerca de cómo hacerlo, consulte [utilización de LDIFDE para importar y exportar objetos de directorio a Active Directory (artículo 237677 de Knowledge Base)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>NT Backup está instalado  
 **Problema:**  El programa Windows NT Backup está instalado en el servidor.  
  
 **Impacto:**   Windows Server Essentials usa copias de seguridad de Windows Server. Si también está instalado el programa Windows NT Backup, puede haber conflictos entre los dos programas de copia de seguridad. Esto puede provocar un error del proceso de Copia de seguridad de Windows Server. Los conflictos también pueden impedirle usar una copia de seguridad para restaurar el servidor.  
  
 **Solución:** Para resolver este problema:, desinstale el programa NT Backup desde el servidor.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS no es propietario del puerto 80 (0.0.0.0:80) ni del puerto 443 (0.0.0.0:443)  
 **Problema:**  Internet Information Services (IIS) no es propietario del puerto 80 (0.0.0.0:80) ni del puerto 443. Estos puertos están enlazados actualmente por otras aplicaciones.  
  
 **Impacto:**   Las aplicaciones web de Windows Server Essentials requieren el uso de los puertos 80 y 443 para que los servicios disponibles para los usuarios. Si otro proceso o aplicación ya está usando el puerto 80 o 443, no se pueden ejecutar las aplicaciones web de Windows Server Essentials. Si esto ocurre, el acceso Web remoto y otras aplicaciones no estarán disponibles para los usuarios.  
  
 **Solución:**  Para resolver este problema:, bien la desinstalarán la aplicación que ya está usando el puerto 80 o 443 o asigne esa aplicación a un puerto diferente.  
  
### <a name="the-default-website-is-not-running"></a>No se está ejecutando el sitio web predeterminado  
 **Problema:**  No se está ejecutando el sitio Web predeterminado en el entorno de Windows Server Essentials.  
  
 **Impacto:**   Las aplicaciones web de Windows Server Essentials requieren el uso del sitio Web predeterminado. Si no se está ejecutando el sitio web predeterminado, el acceso Web remoto y otras aplicaciones no estarán disponibles para los usuarios.  
  
 **Solución:**  
  
##### <a name="to-start-the-default-website"></a>Para iniciar el sitio web predeterminado  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Sitios**.  
  
3.  Haga clic con el botón derecho en **Sitio web predeterminado**, seleccione **Administrar sitio web** y haga clic en **Inicio**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Los permisos Lectura y Script del directorio virtual /Remoto son incorrectos  
 **Problema:**  No se asignaron permisos Lectura y Script al directorio virtual /Remoto.  
  
 **Impacto:**  Si los permisos Lectura y Script del directorio virtual /Remoto son incorrectos, los usuarios no pueden usar el acceso Web remoto. Al intentar usar acceso Web remoto para explorar Internet, podrían encontrarse el error "HTTP Error 403.1 prohibido".  
  
 **Solución:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Para asignar permisos Lectura y Script al directorio /Remoto  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Sitios**.  
  
3.  Expanda **Sitio web predeterminado** y, a continuación, expanda **Remoto**.  
  
4.  En la vista **Características**, haga doble clic en **Asignaciones de controlador**.  
  
5.  En el panel **Acciones**, haga clic en **Editar permisos de característica**.  
  
6.  Active las casillas **Leer** y **Script** y, a continuación, haga clic en **Aceptar**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>La Redirección HTTP se configuró o se heredó en el directorio virtual /Remoto  
 **Problema:**  El atributo Redirección HTTP se configuró o se heredó en el directorio virtual /Remoto de forma inesperada.  
  
 **Impacto:**  Si el atributo Redirección HTTP está establecido en el directorio virtual /Remoto, Lugar de trabajo remoto en Web no funcionará correctamente.  
  
 **Solución:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Para quitar el atributo Redirección HTTP  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Sitios**.  
  
3.  Expanda **Sitio web predeterminado** y, a continuación, expanda **Remoto**.  
  
4.  En la vista **Características**, haga doble clic en **Redirección HTTP**.  
  
5.  Desactive la casilla **Redirigir las solicitudes a este destino** y, a continuación, haga clic en **Aplicar** en el panel **Acciones**.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Existe un nombre de host para el puerto 80 en el sitio web predeterminado  
 **Problema:**  Se asignó un nombre de host al puerto 80 en el sitio web predeterminado.  
  
 **Impacto:**  Si se asigna un nombre de host para el puerto 80 en el sitio Web predeterminado, es posible que no pueda conectarse a algunas aplicaciones web de Windows Server Essentials. Un nombre de host no es necesario y no se recomienda en esta situación.  
  
 **Solución:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Para borrar la entrada del nombre de host del puerto 80 en el sitio web predeterminado  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Sitios**.  
  
3.  En la vista **Características**, haga clic con el botón derecho en **Sitio web predeterminado** y, a continuación, haga clic en **Enlaces**.  
  
4.  En **Enlaces de sitios**, seleccione la configuración **http para el puerto 80** y, a continuación, haga clic en **Editar**.  
  
5.  En **Editar el enlace de sitio**, desactive la entrada **Nombre de host** y haga clic en **Aceptar**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>La copia de seguridad no se realiza correctamente debido a una partición oculta  
 **Problema:**  Está programada la copia de seguridad de una partición que no es NTFS mediante Copias de seguridad de Windows Server.  
  
 **Impacto:**  Copias de seguridad de Windows Server solo puede hacer copias de seguridad de particiones que tienen formato NTFS.  
  
 **Solución:**  No configure Copias de seguridad de Windows Server para realizar copias de seguridad de particiones que no son NTFS. Para obtener más información, consulte [se registran eventos identificadores 12290 y 16387 cuando se produce un error en la copia de seguridad de estado del sistema en un equipo basado en Windows Server 2008 (artículo 968128 de Knowledge Base)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>La copia de seguridad más reciente no se realizó correctamente  
 **Problema:**  La copia de seguridad más reciente no se completó correctamente.  
  
 **Impacto:**  El estado de copia de seguridad del sistema no es correcto.  
  
 **Solución:**  Revise los registros de eventos y los registros de copia de seguridad de los errores que se produjeron durante la copia de seguridad más reciente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>El tipo de inicio del servicio de replicación de archivos no se estableció en Automático  
 **Problema:**  El servicio de replicación de archivos (FRS) podría no iniciarse si el tipo de inicio no se establece en el valor predeterminado automático.  
  
 **Impacto:**  Si el servicio de replicación de archivos no se está ejecutando, el controlador de dominio podría dejar de anunciar sus servicios. Esto puede provocar otros problemas, como errores de inicio de sesión y errores de directiva de grupo.  
  
 **Solución:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Para configurar el servicio de replicación de archivos para el inicio automático  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Replicación de archivos**.  
  
3.  En **Tipo de inicio**, seleccione **Automático** y, a continuación, haga clic en **Aplicar**.  
  
### <a name="the-file-replication-service-is-not-running"></a>El servicio de replicación de archivos no se está ejecutando  
 **Problema:**  El servicio de replicación de archivos no se está ejecutando.  
  
 **Impacto:**  Si el servicio de replicación de archivos no se está ejecutando, el controlador de dominio podría dejar de anunciar sus servicios. Este comportamiento puede provocar otros problemas, como errores de inicio de sesión y errores de directiva de grupo.  
  
 **Solución:**  
  
##### <a name="to-start-the-file-replication-service"></a>Para iniciar el servicio de replicación de archivos  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio de replicación de archivos**.  
  
3.  Haga clic en **Inicio**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>La cuenta de inicio de sesión del servicio de replicación de archivos no está configurada para usar la cuenta de sistema local  
 **Problema:**  El servicio de replicación de archivos no está configurado para usar la cuenta de sistema local como cuenta de inicio de sesión predeterminada.  
  
 **Impacto:**  Si el servicio de replicación de archivos no usa la cuenta de sistema local como cuenta de inicio de sesión predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden desencadenar otros errores y podrían hacer que el controlador de dominio dejase de anunciar sus servicios.  
  
 **Solución:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Para configurar la cuenta de sistema local como cuenta de inicio de sesión predeterminada para la replicación de archivos  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Replicación de archivos**.  
  
3.  En la página **Propiedades del servicio**, haga clic en la pestaña **Iniciar sesión**.  
  
4.  Seleccione la opción **Cuenta de sistema local** y haga clic en **Aplicar**.  
  
5.  Reinicie el servicio.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>El tipo de inicio del servicio de replicación DFS no se estableció en automático  
 **Problema:**  Puede que el servicio de replicación DFS no se pueda iniciar si no se estableció el tipo de inicio en el valor predeterminado de automático.  
  
 **Impacto:**  Si el servicio de replicación DFS no se está ejecutando, el controlador de dominio podría dejar de anunciar sus servicios. Esto puede provocar otros problemas, como errores de inicio de sesión y errores de directiva de grupo.  
  
 **Solución:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Para configurar el servicio de replicación DFS para el inicio automático  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Replicación DFS**.  
  
3.  En **Tipo de inicio**, seleccione **Automático** y, a continuación, haga clic en **Aplicar**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>El servicio de replicación DFS no se está ejecutando  
 **Problema:**  El servicio de replicación DFS no se está ejecutando actualmente.  
  
 **Impacto:**  Si el servicio de replicación DFS no se está ejecutando, el controlador de dominio podría dejar de anunciar sus servicios. Este comportamiento puede provocar otros problemas, como errores de inicio de sesión y errores de directiva de grupo.  
  
 **Solución:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Para iniciar el servicio de replicación DFS  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Replicación DFS**.  
  
3.  Haga clic en **Inicio**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>El servicio de replicación DFS no está configurado para usar la cuenta de sistema local  
 **Problema:**  El servicio de replicación DFS no está configurado para usar la cuenta de sistema local como cuenta de inicio de sesión predeterminada.  
  
 **Impacto:**  Si el servicio de replicación DFS no usa la cuenta de sistema local como cuenta de inicio de sesión predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden desencadenar otros errores y podrían hacer que el controlador de dominio dejase de anunciar sus servicios.  
  
 **Solución:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Para configurar la replicación DFS para que use la cuenta de sistema local como cuenta de inicio de sesión predeterminada  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Replicación DFS**.  
  
3.  En la página **Propiedades del servicio**, haga clic en la pestaña **Iniciar sesión**.  
  
4.  Seleccione la opción **Cuenta de sistema local** y haga clic en **Aplicar**.  
  
5.  Reinicie el servicio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>El Servicio de integración de Windows Server Office 365 no está configurado para usar la cuenta de sistema local  
 **Problema:**  El Servicio de integración de Windows Server Office 365 no está configurado para usar la cuenta de sistema local como cuenta de inicio de sesión predeterminada.  
  
 **Impacto:**  Si el servicio de integración de Windows Server Office 365 no usa la cuenta de sistema local como cuenta de inicio de sesión predeterminada, algunas características de Office 365 podrían no funcionar correctamente. También podría encontrarse con errores relacionados con los permisos.  
  
 **Solución:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar el servicio de integración de Office 365 para que use la cuenta de sistema local como cuenta de inicio de sesión predeterminada  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio de integración de Windows Server Office 365**.  
  
3.  En la página **Propiedades del servicio**, haga clic en la pestaña **Iniciar sesión**.  
  
4.  Seleccione la opción **Cuenta de sistema local** y haga clic en **Aplicar**.  
  
5.  Reinicie el servicio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>El servicio de integración de Windows Server Office 365 no se está ejecutando  
 **Problema:**  El servicio de integración de Windows Server Office 365 no se está ejecutando actualmente.  
  
 **Impacto:**  Si el servicio de integración de Windows Server Office 365 no se está ejecutando, no estarán disponibles las características basadas en la nube de Office 365.  
  
 **Solución:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Para iniciar el servicio de integración de Windows Server Office 365  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio de integración de Windows Server Office 365**.  
  
3.  Haga clic en **Inicio**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>El tipo de inicio del servicio de integración de Windows Server Office 365 no está establecido en automático  
 **Problema:**  Puede que el servicio de integración de Windows Server Office 365 no se pueda iniciar si no se estableció el tipo de inicio en el valor predeterminado de automático.  
  
 **Impacto:**  Si el servicio de integración de Windows Server Office 365 no se está ejecutando, no estarán disponibles las características basadas en la nube de Office 365.  
  
 **Solución:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Para configurar el servicio de integración de Office 365 para el inicio automático  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio de integración de Windows Server Office 365**.  
  
3.  En **Tipo de inicio**, seleccione **Automático** y, a continuación, haga clic en **Aplicar**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Falta un valor del Registro o no se estableció correctamente  
 **Problema:**  Una clave del Registro en HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy no contiene valores correctos o no existe.  
  
 **Impacto:**  Si la clave del Registro RPCProxy no se ha configurado correctamente, puede que reciba un mensaje de error similar al siguiente: "El equipo no se puede conectar al equipo remoto porque el servidor de la Puerta de enlace de Escritorio remoto está fuera de servicio temporalmente. Intente conectarse de nuevo más tarde o póngase en contacto con el administrador de red para obtener ayuda”.  
  
 **Solución:**  
  
##### <a name="to-correct-the-registry-setting"></a>Para corregir la configuración del Registro  
  
1.  Abre el Editor del Registro.  
  
2.  Desplácese hasta la siguiente clave del Registro:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Asegúrese de que la cadena denominada "Sitio Web" tiene un valor de datos del sitio Web predeterminado:  
  
    -   Si el valor de datos es incorrecto, modifique la cadena para usar el valor correcto.  
  
    -   Si la cadena no existe, cree una nueva cadena denominada "Sitio Web" y establezca el valor de datos para el sitio Web predeterminado."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>El tipo de inicio del Servicio del módulo de copia de seguridad a nivel de bloque no se estableció en Manual  
 **Problema:**  El Servicio del módulo de copia de seguridad a nivel de bloque no usa el tipo de inicio predeterminado de Manual.  
  
 **Impacto:**  El Servicio del módulo de copia de seguridad a nivel de bloque podría no iniciarse si el tipo de inicio no se establece en Manual. Este problema: puede provocar errores de los trabajos de copia de seguridad de Windows Server.  
  
 **Solución:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Para configurar el Servicio del módulo de copia de seguridad a nivel de bloque para que se inicie manualmente  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio del módulo de copia de seguridad a nivel de bloque**.  
  
3.  En **Tipo de inicio**, seleccione **Manual** y, a continuación, haga clic en **Aplicar**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>La cuenta de inicio de sesión del Servicio del módulo de copia de seguridad a nivel de bloque no está configurada para usar la cuenta de sistema local  
 **Problema:**  El Servicio del módulo de copia de seguridad a nivel de bloque no está configurado para usar la cuenta de sistema local como cuenta de inicio de sesión predeterminada.  
  
 **Impacto:**  Si el Servicio del módulo de copia de seguridad a nivel de bloque no usa la cuenta de sistema local como cuenta de inicio de sesión predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden impedir que los trabajos de Copias de seguridad de Windows Server se completen correctamente.  
  
 **Solución:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar el Servicio del módulo de copia de seguridad a nivel de bloque para que use la cuenta de sistema local como cuenta de inicio de sesión predeterminada  
  
1.  Abra la consola Servicios.  
  
2.  En la lista de servicios, haga doble clic en **Servicio del módulo de copia de seguridad a nivel de bloque**.  
  
3.  En la página **Propiedades del servicio**, haga clic en la pestaña **Iniciar sesión**.  
  
4.  Seleccione la opción **Cuenta de sistema local** y haga clic en **Aplicar**.  
  
5.  Reinicie el servicio.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>El nombre común del certificado que está enlazado al sitio web de WSS Certificate Web Service no coincide con el nombre del servidor  
 **Problema:**  Se enlazó un certificado no válido al sitio web de WSS Certificate Web Service en IIS. El nombre común de este certificado no coincide con el nombre del servidor.  
  
 **Impacto:**  Si enlaza un certificado no válido al sitio web de WSS Certificate Web Service, el Asistente para conectarse podría no funcionar correctamente.  
  
 **Solución:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Para configurar un certificado válido para el WSS Certificate Web Service  
  
1.  Abra el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y haga clic en **Sitios**.  
  
3.  Haga clic con el botón derecho en **WSS Certificate Web Service** y, a continuación, haga clic en **Editar enlaces**.  
  
4.  En **Enlaces de sitios**, haga clic en **HTTPS** y, a continuación, haga clic en **Editar**.  
  
5.  En **Editar el enlace de sitio**, para el **Certificado SSL**, seleccione el certificado que tenga el mismo nombre que el servidor.  
  
6.  Si más de una entrada de certificado tiene el mismo nombre que el servidor, haga clic en **Ver** para determinar qué certificado es válido y, a continuación, seleccione el certificado adecuado.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Parece haber un problema con el enlace de certificado del servicio Puerta de enlace de Escritorio remoto  
 **Problema:**  Parece que el certificado del servicio Puerta de enlace de Escritorio remoto no se enlazó correctamente.  
  
 **Impacto:**  Si el certificado del servicio de Puerta de enlace de Escritorio remoto no está configurado correctamente, los usuarios no pueden conectarse al acceso Web remoto.  
  
 **Solución:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Para corregir el enlace del servicio de Puerta de enlace de Escritorio remoto  
  
-   Abra un símbolo del sistema como administrador y escriba los comandos siguientes:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Para obtener más información, consulte [cómo administrar el servicio de puerta de enlace de escritorio remoto en Windows Server Essentials (artículo 2472211 de Knowledge Base)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
