---
title: Reglas que se utiliza la herramienta Best Practices Analyzer (BPA) de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Reglas que se utiliza la herramienta Best Practices Analyzer (BPA) de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este artículo describen las reglas utilizadas por el Windows Server Essentials Best Practices Analyzer (BPA). El BPA examina un servidor que ejecuta Windows Server Essentials y presenta un informe que se describe problemas y proporciona recomendaciones para resolverlos problemas. Las recomendaciones están desarrolladas por la organización de soporte técnico de producto de Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Con la herramienta  
 Es un procedimiento estándar, al migrar a Windows Server Essentials de Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials o Windows Home Server 2011, para ejecutar el BPA en el servidor de destino después de finalizar migrar configuraciones y datos. Puede ejecutar la herramienta desde el panel en cualquier momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Para ejecutar el BPA de Windows Server Essentials en el servidor  
  
1.  Iniciar sesión en el servidor como administrador y, a continuación, abra el panel.  
  
2.  En el panel, haz clic en el **dispositivos** pestaña.  
  
3.  En la **tareas de servidor** panel, haz clic en **analizador de procedimientos recomendados**.  
  
4.  Revisa cada mensaje BPA y sigue las instrucciones para resolver problemas si es necesario.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Reglas utilizadas por el analizador de procedimientos recomendados  
  
### <a name="disable-ip-filtering"></a>Deshabilitar el filtrado de IP  
 **Problema:** filtrado de IP está habilitado en el servidor. Debes deshabilitar el filtrado de IP.  
  
 **Impacto:** si se habilita el filtrado de IP, el tráfico de red podría estar bloqueado.  
  
 **Resolución:**  
  
##### <a name="to-disable-ip-filtering"></a>Para deshabilitar el filtrado de IP  
  
1.  Abre regedit.exe en el servidor.  
  
2.  Navegar a HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Haz clic en **EnableSecurityFilters**y, a continuación, haz clic en **modificar**.  
  
4.  En la **Editar valor DWORD (32 bits) valor** ventana, cambiar la **información del valor** campo a cero y, a continuación, haz clic en **Aceptar**.  
  
5.  Para aplicar el cambio, reinicie el servidor.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>El servicio del Coordinador de transacciones distribuidas (MSDTC) debe establecerse para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio MSDTC no está configurado para iniciarse automáticamente  
  
 **Impacto:** el servicio MSDTC podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, se pueden producir un error en algunas funciones de COM o SQL Server. Como resultado, las aplicaciones que usan las funciones de Microsoft SQL Server o COM no funcionen correctamente.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Para configurar el servicio MSDTC que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Coordinador de transacciones distribuidas** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automático (Inicio retrasado)**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de Net Logon debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio de Net Logon no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio de Net Logon podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, el servidor no puede autenticar usuarios y servicios.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Para configurar el servicio de Netlogon que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Netlogon** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>El servicio cliente DNS debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio cliente DNS no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio cliente DNS podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, el servidor no poder resolver los nombres DNS.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Para configurar el servicio cliente DNS que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DNS** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de servidor DNS debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio de servidor DNS no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio de servidor DNS podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, no se producirán actualizaciones de DNS.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Para configurar el servicio de servidor DNS que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **servidor DNS** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Servicios Web de Active Directory no se establece en el modo de inicio predeterminado  
 **Problema:** servicios Web de Active Directory no se establece en el modo de inicio predeterminado de automático.  
  
 **Impacto:** servicios de Web de Active Directory (ADWS) no está establecida en el modo de inicio predeterminado de automático. Si se detiene o se deshabilita, aplicaciones de cliente como el módulo de Active Directory para Windows PowerShell ADWS en el servidor o el centro de administración de Active Directory no puede tener acceso o administrar instancias de servicio de directorio que se ejecutan en el servidor. Para obtener más información, consulta [Novedades en AD DS: servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Para configurar el servicio de servicios Web de Active Directory que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **servicios Web de Active Directory** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>El servicio cliente DHCP debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio cliente DHCP no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio cliente DHCP no iniciará automáticamente cuando se inicia el servidor. Si este servicio se detiene, los equipos cliente no pueden recibir una dirección IP del servidor.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Para configurar el servicio cliente DHCP para iniciarse automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DHCP** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de administración de IIS deben estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio de administración de IIS no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio de administración de IIS no se iniciará automáticamente cuando se inicia el servidor. Si se detiene el servicio, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Para configurar el servicio de administración de IIS que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicio de administración de IIS**y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de publicación de World Wide Web debe configurarse para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio de publicación de World Wide Web no está configurado para iniciarse automáticamente.  
  
 **Impacto:** el servicio de publicación de World Wide Web podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Para configurar el servicio de publicación de World Wide Web que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicio de publicación de World Wide Web**y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de registro remoto debe estar configurado para iniciarse automáticamente de manera predeterminada  
 **Problema:** el servicio de registro remoto no está configurado para iniciarse automáticamente.  
  
 **Impacto:**  
  
 El servicio de registro remoto podría no iniciarse automáticamente cuando se inicia el servidor. Si se detiene el servicio, es posible que no se puede realizar algunas operaciones de red de forma remota.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Para configurar el servicio de registro remoto que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **registro remoto** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>El servicio de puerta de enlace de escritorio remoto debe estar configurado para iniciarse automáticamente de manera predeterminada  
 **Problema:** el servicio de puerta de enlace de escritorio remoto no está configurado para iniciarse automáticamente.  
  
 **Impacto:** si se detiene el servicio, es posible que usuarios no puedan acceder a equipos con acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Para configurar el servicio de puerta de enlace de escritorio remoto que se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **puerta de enlace de escritorio remoto** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automático (Inicio retrasado)**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>El servicio hora de Windows debe estar configurado para iniciarse automáticamente de forma predeterminada  
 **Problema:** el servicio hora de Windows no está configurado para iniciarse automáticamente.  
  
 **Impacto:** si se detiene el servicio, sincronización de datos y el tiempo no están disponibles.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Para configurar el servicio hora de Windows se inicie automáticamente  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **tiempo Windows** de servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **General** pestaña, cambia la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Se debe iniciar el servicio del Coordinador de transacciones distribuidas (MSDTC)  
 **Problema:** el servicio MSDTC no se está ejecutando en el servidor.  
  
 **Impacto:** si se detiene el servicio, algunas funciones de SQL Server o COM pueden producir un error. Como resultado, las aplicaciones que usan las funciones de Microsoft SQL Server o COM no funcionen correctamente.  
  
 **Resolución:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Para iniciar el servicio del Coordinador de transacciones distribuidas  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Coordinador de transacciones distribuidas** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Se debe iniciar el servicio Netlogon  
 **Problema:** el servicio de Net Logon no se está ejecutando en el servidor.  
  
 **Impacto:** si no se inicia este servicio, el servidor no puede autenticar usuarios y servicios.  
  
 **Resolución:**  
  
##### <a name="to-start-the-netlogon-service"></a>Para iniciar el servicio de Netlogon  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Netlogon** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Se debe iniciar el servicio cliente DNS  
 **Problema:** no se está ejecutando el servicio cliente DNS en el servidor.  
  
 **Impacto:** si no se inicia este servicio, el servidor es posible que pueda resolver los nombres DNS.  
  
 **Resolución:**  
  
##### <a name="to-start-the-dns-client-service"></a>Para iniciar el servicio cliente DNS  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DNS** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Se debe iniciar el servicio de servidor DNS  
 **Problema:** no se está ejecutando el servicio de servidor DNS en el servidor.  
  
 **Impacto:** si no se inicia el servicio de servidor DNS, las actualizaciones de DNS podrían no producirse.  
  
 **Resolución:**  
  
##### <a name="to-start-the-dns-server-service"></a>Para iniciar el servicio de servidor DNS  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **servidor DNS** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="active-directory-web-services-is-not-started"></a>No se inicia de servicios Web de Active Directory  
 **Problema:** servicios Web de Active Directory no se inicia.  
  
 **Impacto:** servicios de Web de Active Directory (ADWS) no se inicia. Si se detiene o se deshabilita, aplicaciones de cliente como el módulo de Active Directory para Windows PowerShell ADWS en el servidor o el centro de administración de Active Directory no puede tener acceso o administrar instancias de servicio de directorio que se ejecutan en el servidor. Para obtener más información, consulta [Novedades en AD DS: servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Resolución:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Para iniciar el servicio de servicios Web de Active Directory  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicios Web de Active Directory**y, a continuación, haz clic en **inicio**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Se debe iniciar el servicio cliente DHCP  
 **Problema:** el servicio cliente DHCP no se está ejecutando en el servidor.  
  
 **Impacto:** si este servicio se detiene, los equipos cliente no pueden recibir una dirección IP del servidor.  
  
 **Resolución:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Para iniciar el servicio cliente DHCP  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DHCP** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Se debe iniciar el servicio de administración de IIS  
 **Problema:** el servicio de administración de IIS no se está ejecutando en el servidor.  
  
 **Impacto:** si se detiene el servicio, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Para iniciar el servicio de administración de IIS  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicio de administración de IIS**y, a continuación, haz clic en **inicio**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Se debe iniciar el servicio de publicación de World Wide Web  
 **Problema:** el servicio de publicación de World Wide Web no se está ejecutando en el servidor.  
  
 **Impacto:** si se detiene el servicio, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Para iniciar el servicio de publicación de World Wide Web  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicio de publicación de World Wide Web**y, a continuación, haz clic en **inicio**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Se debe iniciar el servicio de puerta de enlace de escritorio remoto  
 **Problema:** no se está ejecutando el servicio de puerta de enlace de escritorio remoto en el servidor.  
  
 **Impacto:** si se detiene el servicio, es posible que usuarios no puedan acceder a equipos mediante el uso de acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Para iniciar el servicio de puerta de enlace de escritorio remoto  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **puerta de enlace de escritorio remoto** servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Se debe iniciar el servicio hora de Windows  
 **Problema:** el servicio hora de Windows no se está ejecutando en el servidor.  
  
 **Impacto:** si se detiene el servicio, sincronización de datos y el tiempo no será disponible.  
  
 **Resolución:**  
  
##### <a name="to-start-the-windows-time-service"></a>Para iniciar el servicio hora de Windows  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **tiempo Windows** de servicio y, a continuación, haz clic en **inicio**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>La cuenta de inicio de sesión de servicio del Coordinador de transacciones distribuidas (MSDTC) debe ser NT Authority\servicio  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio del Coordinador de transacciones distribuidas (MSDTC).  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, las aplicaciones que usan las funciones de SQL Server o COM no funcionen correctamente.  
  
 **Resolución:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Para cambiar la cuenta de inicio de sesión para el servicio  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Coordinador de transacciones distribuidas** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **esta cuenta**, tipo **NT Authority\servicio**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de Net Logon debe usar la cuenta de sistema Local como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de Net Logon.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, el servidor no puede autenticar usuarios y servicios.  
  
 **Resolución:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de Netlogon  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Netlogon** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **cuenta de sistema Local**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio cliente DNS debe usar la cuenta como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio cliente DNS.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, el servidor es posible que pueda resolver los nombres DNS.  
  
 **Resolución:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de cliente DNS  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DNS** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **esta cuenta**y, a continuación, escribe **NT Authority\servicio**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de servidor DNS debe usar la cuenta de sistema Local como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de servidor DNS.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, las actualizaciones de DNS podrían no producirse.  
  
 **Resolución:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de servidor DNS  
  
1.  Abre services.msc en el servidor  
  
2.  Haz clic en el **servidor DNS** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **cuenta de sistema Local**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Servicios Web de Active Directory no es la cuenta de inicio de sesión de forma predeterminada  
 **Problema:** servicios Web de Active Directory no es la cuenta de inicio de sesión de forma predeterminada. De manera predeterminada, la cuenta de inicio de sesión se establece en **cuenta de sistema Local**.  
  
 **Impacto:** servicios de Web de Active Directory (ADWS) no se inicia. Si se detiene o se deshabilita, aplicaciones de cliente como el módulo de Active Directory para Windows PowerShell ADWS en el servidor o el centro de administración de Active Directory no puede tener acceso o administrar instancias de servicio de directorio que se ejecutan en el servidor. Para obtener más información, consulta [Novedades en AD DS: servicios Web de Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) en la biblioteca técnica de Windows Server.  
  
 **Resolución:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Para cambiar la cuenta de inicio de sesión de servicios Web de Active Directory  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicios Web de Active Directory**y, a continuación, haz clic en **propiedades**.  
  
3.  Cambiar la **tipo de inicio** a **automática**y, a continuación, haz clic en **Aceptar**.  
  
4.  Servicios de Active Directory Web **propiedades**, haz clic en el **iniciar sesión** pestaña.  
  
5.  Selecciona el **cuenta de sistema Local** opción y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de actualización de Windows debe usar la cuenta de sistema Local como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de actualizaciones automáticas.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, el servidor no puede recibir actualizaciones automáticas.  
  
 **Resolución:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de Windows Update  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **Windows Update** de servicio y, a continuación, haz clic en Propiedades.  
  
3.  En la **iniciar sesión**, selecciona **cuenta de sistema Local**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>El servicio cliente DHCP debe usar la cuenta de NT AUTHORITY\LocalService como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio cliente DHCP.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, el equipo cliente no recibirá direcciones IP del servidor.  
  
 **Resolución:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio cliente DHCP  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **cliente DHCP** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **esta cuenta**y, a continuación, escribe **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de administración de IIS debe usar la cuenta de sistema Local como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de administración de IIS.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios que sean necesarios para el funcionamiento esperado. Como resultado, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-change-the-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión de servicio  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **el servicio de administración de IIS**y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **cuenta de sistema Local**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>El servicio de publicación de World Wide Web deben usar la cuenta de sistema Local como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de publicación de World Wide Web.  
  
 **Impacto:** el servicio no podría tener los permisos necesarios para que funcione según lo esperado. Como resultado, es posible que no puede acceder a los sitios Web que se ejecutan en el servidor, como acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de publicación de World Wide Web  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en **servicio de publicación de World Wide Web**y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **cuenta de sistema Local**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio de puerta de enlace de escritorio remoto debe usar la cuenta como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio de puerta de enlace de escritorio remoto.  
  
 **Impacto:** el servicio no podría tener los permisos adecuados para que funcione según lo esperado. Como resultado, es posible que los usuarios no puedan acceder a equipos mediante el uso de acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio de puerta de enlace de escritorio remoto  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **puerta de enlace de escritorio remoto** servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **esta cuenta**y, a continuación, escribe **NT Authority\servicio**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>El servicio hora de Windows debe usar la cuenta como su cuenta de inicio de sesión  
 **Problema:** ha cambiado la cuenta de inicio de sesión de forma predeterminada para el servicio hora de Windows.  
  
 **Impacto:** el servicio no podría tener los permisos adecuados para que funcione según lo esperado. Como resultado, sincronización de fecha y hora podría no estar disponible.  
  
 **Resolución:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Para cambiar la cuenta de inicio de sesión del servicio hora de Windows  
  
1.  Abre services.msc en el servidor.  
  
2.  Haz clic en el **tiempo Windows** de servicio y, a continuación, haz clic en **propiedades**.  
  
3.  En la **iniciar sesión**, selecciona **esta cuenta**y, a continuación, escribe **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>El grupo integrado Administradores no tiene el derecho de iniciar sesión como trabajo por lotes  
 **Problema:** grupo integrado Administradores no tiene el derecho de iniciar sesión como trabajo por lotes.  
  
 **Impacto:** si el administrador crea una alerta y configura la alerta para ejecutarse cuando el administrador no ha iniciado sesión, se producirá un error en la alerta con un código de error de 2147943785.  
  
 **Resolución:** para obtener información sobre cómo proporcionar a los administradores integrados permiso de grupo para iniciar sesión como trabajo por lotes, consulta [dar el grupo Administrador integrado de la derecha para iniciar sesión como trabajo por lotes](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>El Firewall de Windows está desactivado  
 **Problema:** Firewall de Windows está desactivado. El valor predeterminado está activada.  
  
 **Impacto:** según la configuración del firewall, Firewall de Windows puede ayudar a proteger el servidor y la red de actividades malintencionadas bloqueando cierta información de pasar a través del servidor.  
  
 **Resolución:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Para activar Firewall de Windows en el servidor  
  
1.  Abre el Panel de Control en el servidor.  
  
2.  En el Panel de Control, haz clic en **sistema y seguridad**y, a continuación, haz clic en **Firewall de Windows**.  
  
3.  En el Firewall de Windows, haz clic en **activar o desactivar Firewall de Windows**, selecciona el **activar Firewall de Windows** opción y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>Adaptador de red no está configurado para registrar la dirección IP en DNS  
 **Problema:** adaptador de red no está configurado para registrar su dirección IP en DNS.  
  
 **Impacto:** si la dirección IP del adaptador de red no está registrada en DNS, no sería posible tener acceso al servidor con el nombre de equipo servidor s.  
  
 **Resolución:** comprobar que el adaptador de red interna está configurado para registrar en DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: Los valores de la clave del registro de DNS ForwardingTimeout y RecursionTimeout son idénticos  
 **Problema:** el valor de la clave del registro de DNS ForwardingTimeout no debe ser el mismo que el valor de la clave del registro de RecursionTimeout.  
  
 **Impacto:** no es posible que pueda obtener acceso a recursos de Internet, por su nombre.  
  
 **Resolución:** establece el valor de la clave del registro RecursionTimeout sea mayor que el valor de la clave ForwardingTimeout, ubicado en el registro en HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zona DNS hacia delante de tu dominio de Active Directory no permitir actualizaciones seguras  
 **Problema:** debes configurar la zona de búsqueda directa para permitir solamente actualizaciones dinámicas seguras.  
  
 **Impacto:** cuando habilitas las actualizaciones dinámicas seguras, solo los usuarios autorizados y hosts pueden realizar cambios en los registros.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Para configurar la zona de búsqueda hacia delante de tu dominio de Active Directory  
  
1.  Abre dnsmgmt.msc en el servidor.  
  
2.  Haz clic en la zona de búsqueda hacia delante de tu dominio de Active Directory y, a continuación, haz clic en **propiedades**.  
  
3.  En la **actualizaciones dinámicas** lista desplegable, selecciona **seguro solo**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>Actualiza el reenvío no admite segura zona DNS  
 **Problema:** debes configurar la zona de búsqueda hacia delante de la zona de _msdcs.* para permitir solamente actualizaciones dinámicas seguras.  
  
 **Impacto:** cuando habilitas las actualizaciones dinámicas seguras, solo los usuarios autorizados y hosts pueden realizar cambios en los registros en la zona msdcs.*.  
  
 **Resolución:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Para permitir actualizaciones seguras en la zona _msdcs  
  
1.  Abre dnsmgmt.msc en el servidor.  
  
2.  Haz clic en la zona de la zona _msdcs búsqueda hacia delante y, a continuación, haz clic en **propiedades**.  
  
3.  En la **actualizaciones dinámicas** lista desplegable, selecciona **seguro solo**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>No está habilitada la configuración de seguridad mejorada de Internet Explorer  
 **Problema:** configuración de seguridad mejorada de Internet Explorer (IE ESC) actualmente no está habilitada para el grupo de administradores.  
  
 **Impacto:** si no está habilitada la configuración de seguridad mejorada de Internet Explorer para el grupo de administradores, el servidor e Internet Explorer han aumentado la exposición a los ataques malintencionados que se pueden producir contenido a través de la Web y scripts de la aplicación.  
  
 **Resolución:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar la configuración de seguridad mejorada de Internet Explorer  
  
1.  Abre **administrador del servidor** en el servidor y, a continuación, haz clic en **servidor Local**.  
  
2.  En la **propiedades** panel, cambiar la configuración de **configuración de seguridad mejorada de Internet Explorer** a **en**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>No está habilitada la configuración de seguridad mejorada de Internet Explorer  
 **Problema:** configuración de seguridad mejorada de Internet Explorer (IE ESC) actualmente no está habilitada para el grupo de usuarios.  
  
 **Impacto:** si no está habilitada la configuración de seguridad mejorada de Internet Explorer para el grupo de usuarios, tu servidor e Internet Explorer han aumentado la exposición a los ataques malintencionados que se pueden producir contenido a través de la Web y scripts de la aplicación.  
  
 **Resolución:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Para habilitar la configuración de seguridad mejorada de Internet Explorer  
  
1.  Abre **administrador del servidor**y, a continuación, haz clic en **servidor Local**.  
  
2.  En la **propiedades** panel, cambiar la configuración de **configuración de seguridad mejorada de Internet Explorer** a **en**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>El servidor de origen permanece en servicios y sitios de Active Directory  
 **Problema:** el servidor de origen que ejecuta Windows Small Business Server aún existe en servicios y sitios de Active Directory en el Default-First-Site-Name.  
  
 **Impacto:** si el servidor de origen permanece en Active Director sitios y servicios, los equipos cliente pueden experimentar el problema de conectividad: s.  
  
 **Resolución:** debe degradar el servidor de origen, se quite del dominio y, a continuación, eliminar el servidor de origen de los sitios de Active Directory y servicios y usuarios de Active Directory y equipos.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Servidor de origen permanece en SBSComputer OU  
 **Problema:** el servidor de origen que ejecuta Windows Small Business Server aún existe en equipos y usuarios de Active Directory.  
  
 **Impacto:** si el servidor de origen permanece en Director de activo a los usuarios y equipos, los equipos cliente pueden experimentar el problema de conectividad: s.  
  
 **Resolución:** debe degradar el servidor de origen, se quite del dominio y, a continuación, eliminar el servidor de origen de los sitios de Active Directory y servicios y usuarios de Active Directory y equipos.  
  
### <a name="a-group-policy-is-missing"></a>No existe una directiva de grupo  
 **Problema:** carece de la directiva de grupo de directiva de dominio predeterminada.  
  
 **Impacto:** la directiva de dominio predeterminada es necesaria para funciones de dominio adecuados.  
  
 **Resolución:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Para restaurar una directiva de grupo que faltan  
  
1.  Abre gpmc.msc en el servidor.  
  
2.  En el Administrador de directivas de grupo, expanda el bosque de dominio y buscar en el árbol de consola para el **directiva de dominio predeterminada** objeto de directiva de grupo.  
  
3.  Si la directiva no aparece en el árbol, restaurar una copia de seguridad de estado del sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>Sin DNS los registros de recursos de servidor de nombres  
 **Problema:** no hay ningún registros de recursos del servidor (NS) de nombre DNS en la zona de búsqueda hacia delante de tu servidor.  
  
 **Impacto:** si no registro de recursos del servidor (NS) de nombre DNS no existe en la zona de búsqueda hacia delante para el dominio de Active Directory, los usuarios no puede tener acceso a recursos de la red o en Internet.  
  
 **Resolución:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Para restaurar la falta de nombre DNS servidor registros de recursos  
  
1.  Abre dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, haz clic en la zona de búsqueda hacia delante para el dominio de Active Directory y, a continuación, haz clic en **propiedades**.  
  
3.  En la **servidores de nombres**, compruebe que la configuración sea correcta.  
  
4.  Realiza los cambios necesarios y, a continuación, haz clic en **Aceptar** para guardar la configuración.  
  
### <a name="no-dns-name-server-records"></a>No hay registros de servidor de nombres DNS  
 **Problema:** no hay ningún servidor de nombres DNS (NS) registros de recursos en la zona _msdcs de tu servidor (por ejemplo: _msdcs.contoso.local).  
  
 **Impacto:** si ningún registro de recursos del servidor (NS) de nombre DNS no existe en la zona _msdcs para el dominio de Active Directory, los usuarios no posible tener acceso a recursos de la red o en Internet.  
  
 **Resolución:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Para restaurar los registros de servidor de nombres DNS que faltan  
  
1.  Abre dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, haz clic en la zona de búsqueda hacia delante para la zona _msdcs y, a continuación, haz clic en **propiedades**.  
  
3.  En la **servidores de nombres**, compruebe que la configuración sea correcta.  
  
4.  Realiza los cambios necesarios y, a continuación, haz clic en **Aceptar** para guardar la configuración.  
  
### <a name="no-dns-name-server-records"></a>No hay registros de servidor de nombres DNS  
 **Problema:** no hay ningún servidor de nombres DNS (NS) registros de recursos para el delegado _msdcs reenviar zona de búsqueda.  
  
 **Impacto:** si no existe ningún registro de recursos del servidor (NS) de nombre DNS para el delegado _msdcs reenviar zona de búsqueda, el servicio de servidor DNS no puede resolver los registros de recursos DNS del dominio y no podrán iniciarse.  
  
 **Resolución:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Para volver a configurar el servidor de nombres DNS falta registros  
  
1.  Abre dnsmgmt.msc en el servidor.  
  
2.  En el Administrador de DNS, expande el nombre del servidor y, a continuación, expanda **zonas de búsqueda directa**.  
  
3.  Haz clic en la zona de búsqueda hacia delante de tu dominio de Active Directory (por ejemplo: contoso.local).  
  
4.  La zona _msdcs delegadas aparece como una carpeta atenuada. Haz clic en la zona _msdcs y, a continuación, haz clic en **propiedades**.  
  
5.  En la **servidores de nombres**, compruebe que la configuración sea correcta.  
  
6.  Realiza los cambios necesarios y, a continuación, haz clic en **Aceptar** para guardar la configuración.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Usuarios autenticados no un miembro del grupo de acceso de compatible con versiones anteriores de Windows 2000  
 **Problema:** el grupo Usuarios autenticados no es un miembro del grupo de acceso de compatible con versiones anteriores de Windows 2000.  
  
 **Impacto:** si el grupo Usuarios autenticados integrado no es un miembro del grupo de acceso de compatible con versiones anteriores de Windows 2000, los usuarios de red pueden producirse errores de "Acceso denegado".  
  
 **Resolución:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Para agregar usuarios autenticados al grupo de acceso de compatible con versiones anteriores de Windows 2000  
  
1.  Abre dsa.msc en el servidor.  
  
2.  En la **Builtin** carpeta, haz clic en **acceso de compatible con versiones anteriores de Windows 2000**y, a continuación, haz clic en **propiedades**.  
  
3.  Haz clic en **agregar**, tipo **usuarios autenticados**y, a continuación, haz clic en **Aceptar** dos veces.  
  
### <a name="dns-client-not-configured"></a>Cliente DNS no configurado  
 **Problema:** el cliente DNS no está configurado para apuntar solo a la dirección IP interna del servidor.  
  
 **Impacto:** si el cliente DNS no está configurado para que apunte solo a la dirección IP interna del servidor, se puede producir un error de resolución de nombres DNS.  
  
 **Resolución:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Configurar DNS para apuntar solo a la dirección IP de servidor s interna  
  
1.  En el equipo cliente, abra el **propiedades** página para la conexión de red.  
  
2.  Asegúrate de que DNS está configurado para que apunte solo a la dirección IP interna del servidor.  
  
### <a name="default-application-pool-value-changed"></a>Ha cambiado el valor de grupo de aplicaciones predeterminadas  
 **Problema:** el número máximo de procesos de trabajo para el grupo de aplicaciones DefaultAppPool no se establece en el valor predeterminado de 1.  
  
 **Impacto:** es posible que los usuarios no puedan conectarse a servicios basados en web de Windows Small Business Server.  
  
 **Resolución:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Para restablecer el máximo procesos de trabajo para el grupo de aplicaciones predeterminadas  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **Application Pools**.  
  
3.  En **Application Pools**, haz clic en **DefaultAppPool**y, a continuación, haz clic en **configuración avanzada**.  
  
4.  En **configuración avanzada**, cambiar el valor de **máximo procesos de trabajo** a 1 y, a continuación, haz clic en **Aceptar**.  
  
5.  Cerrar **configuración avanzada**, haz clic en **DefaultAppPool**y, a continuación, detener y reiniciar el grupo de aplicaciones.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Grupo de aplicaciones para el acceso Web remoto no usa la cuenta predeterminada  
 **Problema:** el grupo de aplicaciones RemoteAppPool no se está ejecutando con la cuenta predeterminada.  
  
 **Impacto:** posible que los usuarios de red no pueda acceder al sitio Web de acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Para restablecer el grupo de aplicaciones remoto para usar la cuenta predeterminada  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **Application Pools**.  
  
3.  En **Application Pools**, haz clic en **RemoteAppPool**y, a continuación, haz clic en **configuración avanzada**.  
  
4.  En **configuración avanzada**, cambiar el **identidad** a **NetworkService**y, a continuación, haz clic en **Aceptar**.  
  
5.  Cerrar **configuración avanzada**, haz clic en **RemoteAppPool**y, a continuación, detener y reiniciar el grupo de aplicaciones.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Grupo de aplicaciones para el acceso Web remoto no utiliza la versión de .NET Framework predeterminada  
 **Problema:** el grupo de aplicaciones RemoteAppPool no se está ejecutando con la versión predeterminada de Microsoft .NET Framework.  
  
 **Impacto:** posible que los usuarios de red no pueda acceder al sitio Web de acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Para cambiar la versión de .NET Framework usada por la RemoteAppPool  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **Application Pools**.  
  
3.  En **Application Pools**, haz clic en **RemoteAppPool**y, a continuación, haz clic en **configuración avanzada**.  
  
4.  En **configuración avanzada**, cambiar el **versión de .NET Framework** v4.0 y, a continuación, haz clic en **Aceptar**.  
  
5.  Cerrar **configuración avanzada**, haz clic en **RemoteAppPool**y, a continuación, detener y reiniciar el grupo de aplicaciones.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>El archivo RemoteAccess.log es mayor que 1 GB de tamaño  
 **Problema:** si el tamaño del archivo Remoteaccess.log supera 1 GB, pueden experimentar poco espacio en disco en la unidad del sistema.  
  
 **Impacto:** si el archivo Remoteaccess.log es demasiado grande, podría provocar que el problema de espacio libre: s en la unidad C:.  
  
 **Resolución:** después de realizar la copia de seguridad del servidor, puede eliminar el archivo Remoteaccess.log, que se encuentra en la carpeta Server\Logs\WebApps %ProgramData%\Microsoft\Windows.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>Directorio de registro del sitio Web predeterminado es más de 1 GB de tamaño  
 **Problema:** si el tamaño de la carpeta de registro del sitio Web de forma predeterminada es superior a 1 GB, pueden experimentar poco espacio en disco en la unidad del sistema.  
  
 **Impacto:** si es demasiado grande la carpeta de registro del sitio Web de forma predeterminada, podría provocar que el problema de espacio libre: s en la unidad C:  
  
 **Resolución:** después de hacer la copia de seguridad del servidor, y mientras se detiene el sitio Web predeterminado, puedes eliminar los archivos de registro en la carpeta C:\inetpub\logs\LogFiles\W3SVC1. A continuación, inicia el sitio Web predeterminado.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Ningún enlace SSL en todas las direcciones IP  
 **Problema:** hay ningún enlace para la capa de Sockets seguros (SSL) en todas las direcciones IP en el servidor.  
  
 **Impacto:** si SSL no está enlazado a todas las direcciones IP en el servidor, algunos sitios Web no estará disponible para los usuarios.  
  
 **Resolución:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Para enlazar SSL a todas las direcciones IP en el servidor  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, en la **conexiones** panel, expanda el servidor, **sitios**, haz clic en **sitio Web predeterminado**y, a continuación, haz clic en **Editar enlaces**.  
  
3.  En **enlaces de sitios**, haz clic en **agregar**y, a continuación, selecciona la configuración siguiente:  
  
    -   **Tipo** = **https**  
  
    -   **Dirección IP** = **todos sin asignar**  
  
    -   **Puerto** = 443  
  
4.  Selecciona un certificado SSL y, a continuación, haz clic en **Aceptar** para guardar los cambios.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Ningún enlace SSL en el sitio Web predeterminado  
 **Problema:** hay ningún enlace SSL en el sitio Web predeterminado.  
  
 **Impacto:** si no está enlazado SSL para el sitio Web predeterminado, algunos sitios Web no estén disponibles para los usuarios.  
  
 **Resolución:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Para enlazar SSL para el sitio Web predeterminado  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, en la **conexiones** panel, expanda el servidor, **sitios**, haz clic en **sitio Web predeterminado**y, a continuación, haz clic en **Editar enlaces**.  
  
3.  En **enlaces de sitios**, haz clic en **agregar**y, a continuación, selecciona las siguientes opciones:  
  
    -   **Tipo** = **https**  
  
    -   **Dirección IP** = **todos sin asignar**  
  
    -   **Puerto** = 443  
  
    > [!NOTE]
    >  Si hay un enlace de HTTPS para el puerto 443 para una dirección IP específica, cambiar el **dirección IP** atributo para que enlace a **todos sin asignar**. La excepción a esto es para la dirección IP 127.0.0.1. No cambies el enlace para 127.0.0.1.  
  
4.  Selecciona un certificado SSL y, a continuación, haz clic en **Aceptar** para guardar los cambios.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificado caduca en 30 días  
 **Problema:** el certificado de servidor se agotará antes de 30 días.  
  
 **Impacto:** el servidor no puede usar un certificado expirado. Si expira el certificado, los usuarios no podrán usar las funciones de acceso desde cualquier lugar.  
  
 **Resolución:** para impedir que el certificado que expira, renovar un certificado con la entidad de certificación de confianza.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>Sujeto del certificado no coincide con el nombre configurado por el Asistente para el nombre de dominio  
 **Problema:** firmante del certificado no coincide con el nombre que se ha configurado por el Asistente para el nombre de dominio.  
  
 **Impacto:** si firmante del certificado no coincide con el nombre que se ha configurado por el Asistente para el nombre de dominio, algunos sitios Web no se inicializará. Otros sitios mostrarán el error "Hay un problema con el certificado de seguridad del sitio Web".  
  
 **Resolución:** para resolver este problema:, vuelve a ejecutar el Asistente de acceso desde cualquier lugar de configurar y proporcione el nombre de dominio correcto para el certificado o comprar un nuevo certificado que coincida con el nombre de dominio que quieras usar.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Uno o más cuentas de usuario tienen nombres CN duplicados  
 **Problema:** uno o más cuentas de usuario tengan dupliquen los nombres CN: {0}.  
  
 **Impacto:** si cuentas de usuario tienen nombres CN duplicados, los usuarios podrían no podrá iniciar sesión en la red. Además, las búsquedas de Active Directory para los usuarios pueden devolver valores incorrectos.  
  
 **Resolución:** para resolver este problema:, asegúrate de que las cuentas de usuario de la red no tienen duplicar "CN =" nombres. Para hacerlo más fácil, considera la posibilidad de exportación de contenido de Active Directory a un archivo de texto para su revisión. Para obtener información sobre cómo hacerlo, consulta [Using LDIFDE para importar y exportar objetos de directorio a Active Directory (artículo de Knowledge Base 237677)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>Copia de seguridad de NT está instalado  
 **Problema:** el programa copia de seguridad de Windows NT está instalado en el servidor.  
  
 **Impacto:** Windows Server Essentials usa la copia de seguridad de Windows Server. Si el programa copia de seguridad de Windows NT también está instalado, pueden haber conflictos entre los dos programas de copia de seguridad. Esto puede provocar que el proceso de copia de seguridad de Windows Server que un error. Los conflictos también podrían impedir que usen una copia de seguridad para restaurar el servidor.  
  
 **Resolución:** para resolver este problema:, desinstalar el programa de copia de seguridad de NT desde el servidor.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS no posee el puerto 80 (0.0.0.0:80) o el puerto 443 (0.0.0.0:443)  
 **Problema:** Internet Information Services (IIS) no posee el puerto 80 (0.0.0.0:80) o el puerto 443. Estos puertos actualmente están limitados por otras aplicaciones.  
  
 **Impacto:** aplicaciones web de Windows Server Essentials requieren el uso de puerto 80 y 443 para disponer de los servicios a los usuarios de puerto. Si otro proceso o aplicación ya está usando el puerto 80 o 443, no se pueden ejecutar las aplicaciones web de Windows Server Essentials. Si esto ocurre, no están disponibles para los usuarios acceso Web remoto y otras aplicaciones.  
  
 **Resolución:** para resolver este problema:, ya sea desinstalar la aplicación que ya está usando el puerto 80 o 443 o se asigna a un puerto diferente.  
  
### <a name="the-default-website-is-not-running"></a>No se está ejecutando el sitio Web predeterminado  
 **Problema:** del sitio Web predeterminado no se está ejecutando en el entorno de Windows Server Essentials.  
  
 **Impacto:** aplicaciones web de Windows Server Essentials requieren el uso del sitio Web predeterminado. Si no se está ejecutando el sitio Web predeterminado, no están disponibles para los usuarios acceso Web remoto y otras aplicaciones.  
  
 **Resolución:**  
  
##### <a name="to-start-the-default-website"></a>Para iniciar el sitio Web predeterminado  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **sitios**.  
  
3.  Haz clic en **sitio Web predeterminado**, elija **sitio Web de administrar**y, a continuación, haz clic en **inicio**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Permisos de lectura y el Script para el directorio virtual /Remote son incorrectos  
 **Problema:** permisos de lectura y el Script no se asignan al directorio virtual /Remote.  
  
 **Impacto:** si los permisos de lectura y el Script para el directorio virtual /Remote son incorrectos, los usuarios no pueden usar acceso Web remoto. Al intentar usar el acceso Web remoto para navegar por Internet, que puedan encontrar el error "HTTP Error 403.1 prohibido".  
  
 **Resolución:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Para asignar permisos de lectura y el Script en el directorio /Remote  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **sitios**.  
  
3.  Expande **sitio Web predeterminado**y después expande **remoto**.  
  
4.  En **vista características**, haz doble clic en **asignaciones de controlador**.  
  
5.  En la **acciones** panel, haz clic en **Editar permisos de característica**.  
  
6.  Selecciona el **lectura** y **Script** casillas de verificación y, a continuación, haz clic en **Aceptar**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>Redirección HTTP se establece o se heredan en el directorio virtual /Remote  
 **Problema:** el atributo de redirección HTTP se establece o se heredan en el directorio virtual /Remote inesperadamente.  
  
 **Impacto:** si se establece el atributo de redirección HTTP en el directorio virtual /Remote, área de trabajo Web remoto no funciona correctamente.  
  
 **Resolución:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Para quitar el atributo de redirección HTTP  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **sitios**.  
  
3.  Expande **sitio Web predeterminado**y después expande **remoto**.  
  
4.  En **vista características**, haz doble clic en **redirección HTTP**.  
  
5.  Borrar la **redirigir las solicitudes a este destino** y, a continuación, haz clic en **aplicar** en la **acciones** panel.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Existe un nombre de host para el puerto 80 en el sitio Web predeterminado  
 **Problema:** se asigna un nombre de host para el puerto 80 en el sitio Web predeterminado.  
  
 **Impacto:** si se asigna un nombre de host para el puerto 80 en el sitio Web de forma predeterminada, es posible que no podrás conectar con algunas aplicaciones web de Windows Server Essentials. Un nombre de host no es necesario y no se recomienda en esta situación  
  
 **Resolución:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Para borrar la entrada de nombre de host para el puerto 80 en el sitio Web predeterminado  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **sitios**.  
  
3.  En **vista características**, haz clic en **sitio Web predeterminado**y, a continuación, haz clic en **enlaces**.  
  
4.  En **enlaces de sitios**, selecciona el **http para el puerto 80** configuración y, a continuación, haz clic en **editar**.  
  
5.  En **editar sitio enlace**, desactiva la **nombre de Host** entrada y, a continuación, haz clic en **Aceptar**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Copia de seguridad no se realiza correctamente debido a una partición oculta  
 **Problema:** una partición NTFS no se ha programado para copia de seguridad de la copia de Windows Server.  
  
 **Impacto:** copias de seguridad de Windows Server solo hacer una copia de las particiones que son el formato NTFS.  
  
 **Resolución:** no configure copias de seguridad de Windows Server para hacer copia de las particiones no son NTFS. Para obtener más información, consulta [12290 de identificadores de evento y 16387 se registran cuando se produce un error en la copia de seguridad en un equipo basado en Windows Server 2008 (artículo de Knowledge Base 968128)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>No se pudo realizar la copia de seguridad más reciente  
 **Problema:** el último intento de copia de seguridad no se completó correctamente.  
  
 **Impacto:** el estado de copia de seguridad para el sistema no es correcto.  
  
 **Resolución:** revisar los registros de eventos y registros de copia de seguridad para los errores que ocurrieron durante la copia de seguridad más reciente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>El tipo de inicio para el servicio de replicación de archivo no esté establecido como automático  
 **Problema:** el servicio de replicación de archivos (FRS) podría no iniciarse si el tipo de inicio no se establece en el valor predeterminado de automático.  
  
 **Impacto:** si no se está ejecutando el servicio de replicación de archivo, el controlador de dominio podría dejar de sus servicios de publicidad. Esto puede conducir a otros problemas como errores de inicio de sesión y los errores de la directiva de grupo.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Para configurar el servicio de replicación de archivo para iniciarse automáticamente.  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **archivo replicación**.  
  
3.  Para **tipo de inicio**, selecciona **automática**y, a continuación, haz clic en **aplicar**.  
  
### <a name="the-file-replication-service-is-not-running"></a>No se está ejecutando el servicio de replicación de archivos  
 **Problema:** no se está ejecutando el servicio de replicación de archivos.  
  
 **Impacto:** si no se está ejecutando el servicio de replicación de archivo, el controlador de dominio podría dejar de sus servicios de publicidad. Este comportamiento puede dar lugar a otros problemas como errores de inicio de sesión y los errores de la directiva de grupo.  
  
 **Resolución:**  
  
##### <a name="to-start-the-file-replication-service"></a>Para iniciar el servicio de replicación de archivo  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de replicación**.  
  
3.  Haz clic en **inicio**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>La cuenta de inicio de sesión para el servicio de replicación de archivos no está establecida para usar la cuenta de sistema Local  
 **Problema:** el servicio de replicación de archivo no está configurado para usar la cuenta de sistema Local como la cuenta de inicio de sesión de forma predeterminada.  
  
 **Impacto:** si el servicio de replicación de archivos no usa el sistema Local como la cuenta de inicio de sesión de forma predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden desencadenar otros errores y pueden que el controlador de dominio dejar de sus servicios de publicidad en el futuro.  
  
 **Resolución:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Para configurar el sistema Local como la cuenta de inicio de sesión de forma predeterminada para la replicación de archivo  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **archivo replicación**.  
  
3.  En la **propiedades del servicio** página, haz clic en el **iniciar sesión** pestaña.  
  
4.  Selecciona el **cuenta de sistema Local** opción y, a continuación, haz clic en **aplicar**.  
  
5.  Reiniciar el servicio.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>El tipo de inicio para el servicio de replicación DFS no esté establecido como automático  
 **Problema:** el servicio de replicación de DFS podría no iniciarse si el tipo de inicio no se establece en el valor predeterminado de automático.  
  
 **Impacto:** si no se está ejecutando el servicio de replicación de DFS, el controlador de dominio podría dejar de sus servicios de publicidad. Esto puede conducir a otros problemas como errores de inicio de sesión y los errores de la directiva de grupo.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Para configurar el servicio de replicación DFS para iniciarse automáticamente.  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **replicación DFS**.  
  
3.  Para **tipo de inicio**, selecciona **automática**y, a continuación, haz clic en **aplicar**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>No se está ejecutando el servicio de replicación de DFS  
 **Problema:** no se está ejecutando el servicio de replicación de DFS.  
  
 **Impacto:** si no se está ejecutando el servicio de replicación de DFS, el controlador de dominio podría dejar de sus servicios de publicidad. Este comportamiento puede dar lugar a otros problemas como errores de inicio de sesión y los errores de la directiva de grupo.  
  
 **Resolución:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Para iniciar el servicio de replicación de DFS  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **replicación DFS**.  
  
3.  Haz clic en **inicio**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>La replicación de DFS no es el servicio no está establecida para usar la cuenta de sistema Local  
 **Problema:** el servicio de replicación de DFS no está establecido para usar la cuenta de sistema Local como la cuenta de inicio de sesión de forma predeterminada.  
  
 **Impacto:** si el servicio de replicación de DFS no usa el sistema Local como la cuenta de inicio de sesión de forma predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden desencadenar otros errores y pueden que el controlador de dominio dejar de sus servicios de publicidad en el futuro.  
  
 **Resolución:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Para configurar la replicación de DFS para usar el sistema Local como la cuenta de inicio de sesión predeterminada  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **replicación DFS**.  
  
3.  En la **propiedades del servicio** página, haz clic en el **iniciar sesión** pestaña.  
  
4.  Selecciona el **cuenta de sistema Local** opción y, a continuación, haz clic en **aplicar**.  
  
5.  Reiniciar el servicio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>El servicio de integración de Windows Server Office 365 no está establecido para usar la cuenta de sistema Local  
 **Problema:** el servicio de integración de Windows Server Office 365 no está establecido para usar la cuenta de sistema Local como la cuenta de inicio de sesión de forma predeterminada.  
  
 **Impacto:** si el servicio de integración de Windows Server Office 365 no usa el sistema Local como la cuenta de inicio de sesión de forma predeterminada, algunas características de Office 365 no funcionen correctamente. También pueden producirse errores relacionados con los permisos.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar el servicio de integración de Office 365 para usar el sistema Local como la cuenta de inicio de sesión de forma predeterminada  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de integración de Windows Server Office 365**.  
  
3.  En la **propiedades del servicio** página, haz clic en el **iniciar sesión** pestaña.  
  
4.  Selecciona el **cuenta de sistema Local** opción y, a continuación, haz clic en **aplicar**.  
  
5.  Reiniciar el servicio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>No se está ejecutando el servicio de integración de Windows Server Office 365  
 **Problema:** no se está ejecutando el servicio de integración de Windows Server Office 365.  
  
 **Impacto:** si no se está ejecutando el servicio de integración de Windows Server Office 365, las características en la nube de Office 365 no están disponibles.  
  
 **Resolución:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Para iniciar el servicio de integración de Windows Server Office 365  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de integración de Windows Server Office 365**.  
  
3.  Haz clic en **inicio**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>El tipo de inicio para el servicio de integración de Windows Server Office 365 no esté establecido como automático  
 **Problema:** el servicio de integración de Windows Server Office 365 podría no iniciarse si el tipo de inicio no se establece en el valor predeterminado de automático.  
  
 **Impacto:** si no se está ejecutando el servicio de integración de Windows Server Office 365, las características en la nube de Office 365 no están disponibles.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Para configurar el servicio de integración de Office 365 para iniciarse automáticamente.  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de integración de Windows Server Office 365**.  
  
3.  Para **tipo de inicio**, selecciona **automática**y, a continuación, haz clic en **aplicar**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Un valor del registro falta o está correctamente configurado  
 **Problema:** una clave del registro bajo HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contiene valores incorrectos o no existe.  
  
 **Impacto:** si la clave del registro RPCProxy está configurada correctamente, es posible que recibes un mensaje de error similar al siguiente: "el equipo no se puede conectar al equipo remoto porque el servidor de puerta de enlace de escritorio remoto temporalmente no está disponible. Intenta conectarte de nuevo más tarde o ponte en contacto con el Administrador de red para obtener ayuda."  
  
 **Resolución:**  
  
##### <a name="to-correct-the-registry-setting"></a>Para corregir la configuración del registro  
  
1.  Abre el Editor del registro.  
  
2.  Ve a la siguiente clave del registro:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Asegúrate de que la cadena denominada "Sitio Web" tiene un valor de datos del sitio Web predeterminado:  
  
    -   Si el valor de datos es incorrecto, modificar la cadena para usar el valor correcto.  
  
    -   Si la cadena no existe, crear una nueva cadena denominada "Sitio Web" y establece el valor de datos en el sitio Web predeterminado."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>No se establece el tipo de inicio para el servicio de motor de copia de seguridad de nivel de bloque manual  
 **Problema:** el servicio de motor de copia de seguridad de nivel de bloque no está usando el tipo de inicio predeterminado de Manual.  
  
 **Impacto:** el servicio de motor de copia de seguridad de nivel de bloque podría no iniciarse si el tipo de inicio no se establece en Manual. Este problema: puede ocasionar trabajos de copia de seguridad de Windows Server un error.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Para configurar el servicio de motor de bloque de nivel de copia de seguridad para inicio manual  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de motor de copia de seguridad de nivel de bloque**.  
  
3.  Para **tipo de inicio**, selecciona **Manual**y, a continuación, haz clic en **aplicar**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>La cuenta de inicio de sesión para el servicio de motor de copia de seguridad de nivel de bloque no está establecida para usar la cuenta de sistema Local  
 **Problema:** el servicio de motor de copia de seguridad de nivel de bloque no está establecido para usar la cuenta de sistema Local como la cuenta de inicio de sesión de forma predeterminada.  
  
 **Impacto:** si el servicio de motor de copia de seguridad de nivel de bloqueo no usa el sistema Local como la cuenta de inicio de sesión de forma predeterminada, pueden producirse errores relacionados con los permisos. Estos errores pueden impedir que los trabajos de copia de seguridad de Windows Server se completó correctamente.  
  
 **Resolución:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Para configurar el servicio de motor de copia de seguridad de nivel de bloque para usar el sistema Local como la cuenta de inicio de sesión de forma predeterminada  
  
1.  Abre la consola de servicios.  
  
2.  En la lista de servicios, haz doble clic en **servicio de motor de copia de seguridad de nivel de bloque**.  
  
3.  En la **propiedades del servicio** página, haz clic en el **iniciar sesión** pestaña.  
  
4.  Selecciona el **cuenta de sistema Local** opción y, a continuación, haz clic en **aplicar**.  
  
5.  Reiniciar el servicio.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>El nombre común en el certificado que está enlazado al sitio Web de servicio Web de WSS certificado no coincide con el nombre del servidor  
 **Problema:** el sitio Web de servicio Web de WSS certificados en IIS depende de un certificado no sea válido. El nombre común en este certificado no coincide con el nombre del servidor.  
  
 **Impacto:** si enlazas un certificado no válida para el sitio Web del servicio Web de certificados WSS, el Asistente para conectarse no funcionen correctamente.  
  
 **Resolución:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Para configurar un certificado válido para el servicio Web de certificados de WSS  
  
1.  Abre el Administrador de Internet Information Services (IIS) en el servidor.  
  
2.  En el Administrador de IIS, expanda el nombre del servidor y, a continuación, haz clic en **sitios**.  
  
3.  Haz clic en **servicio Web de certificados WSS**y, a continuación, haz clic en **Editar enlaces**.  
  
4.  En **enlaces de sitios**, haz clic en **HTTPS**y, a continuación, haz clic en **editar**.  
  
5.  En **editar sitio enlace**, para **certificado SSL**, selecciona el certificado que tenga el mismo nombre que el servidor.  
  
6.  Si más de una entrada de certificado tiene el mismo nombre que el servidor, haz clic en **vista** para determinar qué certificado es válido y, a continuación, selecciona el certificado apropiado.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Parece que es un problema con el enlace de certificado para el servicio de puerta de enlace de escritorio remoto  
 **Problema:** el certificado para el servicio de puerta de enlace de escritorio remoto parece estar vinculado incorrectamente.  
  
 **Impacto:** si el certificado para el servicio de puerta de enlace de escritorio remoto no está correctamente configurado, los usuarios no se pueden conectar al acceso Web remoto.  
  
 **Resolución:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Para corregir el enlace para el servicio de puerta de enlace de escritorio remoto  
  
-   Abre un símbolo del sistema como administrador y escribe los siguientes comandos:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Para obtener más información, consulta [cómo administrar el servicio de puerta de enlace de escritorio remoto en Windows Server Essentials (artículo de Knowledge Base 2472211)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
