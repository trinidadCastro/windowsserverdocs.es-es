---
title: Configurar DirectAccess en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurar DirectAccess en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tema proporciona instrucciones paso a paso para configurar DirectAccess en Windows Server Essentials para habilitar la movilidad para conectarse fácilmente a la red de organización s desde cualquier ubicación remota equipado con Internet sin establecer una conexión de red privada virtual (VPN). DirectAccess puede ofrecer a los trabajadores móviles la misma experiencia de conectividad dentro y fuera de la oficina desde sus equipos de Windows 8.1, Windows 8 y Windows 7.  
  
 En Windows Server Essentials, si el dominio contiene más de un servidor de Windows Server Essentials, DirectAccess debe estar configurada en el controlador de dominio.  
  
> [!NOTE]
>  Este tema proporciona instrucciones para configurar DirectAccess cuando el servidor de Windows Server Essentials es el controlador de dominio. Si el servidor de Windows Server Essentials es un miembro de dominio, sigue las instrucciones para configurar DirectAccess en un miembro de dominio en [DirectAccess agregar a una implementación de acceso remoto existente (VPN)](https://technet.microsoft.com/library/jj574220.aspx) en su lugar.  
  
## <a name="process-overview"></a>Introducción al proceso  
 Para configurar DirectAccess en Windows Server Essentials, completa los siguientes pasos.  
  
> [!IMPORTANT]
>  Antes de usar los procedimientos descritos en esta guía para configurar DirectAccess en Windows Server Essentials, debes habilitar VPN en el servidor. Para obtener instrucciones, consulta [administrar VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Paso 1: Agregar herramientas de administración de acceso remoto al servidor](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Paso 2: Cambiar la dirección del adaptador de red del servidor a una dirección IP estática](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Paso 3: Preparar un certificado y el registro de DNS para el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Paso 3a: conceder todos los permisos a los usuarios autenticados de la plantilla de certificado de servidor s Web](#BKMK_GrantFullPermissions)  
  
    -   [Paso 3 b: inscribir un certificado de servidor de la ubicación de red con un nombre común que es no puede resolverse de la red externa](#BKMK_EnrollaCertificate)  
  
    -   [Paso 3c: agregar un nuevo host en el servidor DNS y se asigna a la dirección del servidor de Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Paso 4: Crear un grupo de seguridad para los equipos de cliente de DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Paso 5: Habilitar y configurar DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Paso 5a: habilitar DirectAccess mediante la consola de administración de acceso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Paso 5b: quitar el IPv6Prefix no válido en el GPO de RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Paso 5c: activar equipos cliente que ejecutan Windows 7 Enterprise usar DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Paso 5d: configurar el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Paso 5e: agregar una clave del registro para omitir la certificación de la CA cuando se establece un canal de IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Paso 6: Establecer la configuración de la tabla de directiva de resolución de nombres para el servidor de DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Paso 7: Configurar las reglas de firewall TCP y UDP para el servidor DirectAccess GPO](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Paso 8: Cambiar la configuración de DNS64 para escuchar la interfaz de IP HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Paso 9: Reservar puertos para el servicio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Paso 10: Reiniciar el servicio de WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Apéndice: Configurar DirectAccess mediante Windows PowerShell](#BKMK_AppendixBPowerShellScript) proporciona un script de Windows PowerShell que puedes usar para realizar la instalación de DirectAccess.  
  
##  <a name="BKMK_AddRAM"></a>Paso 1: Agregar herramientas de administración de acceso remoto al servidor  
  
#### <a name="to-add-remote-access-management-tools"></a>Agregar herramientas de administración de acceso remoto  
  
1.  En el servidor, en la esquina inferior izquierda de la página de inicio, haz clic en el **administrador del servidor** icono.  
  
     En Windows Server Essentials, tendrás que buscar para el administrador del servidor para abrirlo. En la página de inicio, escribe **administrador del servidor**y, a continuación, haz clic en **administrador del servidor** en los resultados de búsqueda. Para anclar el administrador del servidor a la página de inicio, haz clic en el administrador del servidor en los resultados de búsqueda y haz clic en **anclar a inicio**.  
  
2.  Si un **Control de cuentas de usuario** muestra el mensaje de advertencia, haz clic en **Sí**.  
  
3.  En el panel del administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**.  
  
4.  En el agregar Roles y características de asistente, haz lo siguiente:  
  
    1.  En la **tipo de instalación** página, haz clic en **instalación basada en rol o característica**.  
  
    2.  En la **página Selección de servidor** (o la **servidor de destino selecciona** página en Windows Server Essentials), haz clic en **seleccionar un servidor desde el grupo de servidores**.  
  
    3.  En la **características** , expanda **Remote Server Administration Tools (Installed)**, expanda **herramientas de administración de acceso remoto (Installed)**, expanda **herramientas de administración de roles (Installed)**, expande **herramientas de administración de acceso remoto**y, a continuación, selecciona **interfaz gráfica de usuario de acceso remoto y herramientas de línea de comandos**.  
  
    4.  Sigue las instrucciones para completar al asistente.  
  
##  <a name="BKMK_AddStaticIP"></a>Paso 2: Cambiar la dirección del adaptador de red del servidor a una dirección IP estática  
 DirectAccess requiere un adaptador con una dirección IP estática. Debes cambiar la dirección IP para el adaptador de red local en el servidor.  
  
#### <a name="to-add-a-static-ip-address"></a>Para agregar una dirección IP estática  
  
1.  En la página de inicio, abre **Panel de Control**.  
  
2.  Haz clic en **red e Internet**y, a continuación, haz clic en **ver estado de la red y tareas**.  
  
3.  En el panel de tareas de la **centro de redes y recursos compartidos**, haz clic en **cambiar la configuración del adaptador**.  
  
4.  Haz clic en el adaptador de red local y, a continuación, haz clic en **propiedades**.  
  
5.  En la **redes** , haga clic **protocolo de Internet versión 4 (TCP/IPv4)**y, a continuación, haz clic en **propiedades**.  
  
6.  En la **General** , haga clic **usar la siguiente dirección IP**y, a continuación, escribe la dirección IP que quieras usar.  
  
     Un valor predeterminado para la máscara de subred aparece automáticamente en el **máscara de subred** cuadro. Acepta el valor predeterminado, o escribe el valor de máscara de subred que quieras usar.  
  
7.  En la **puerta de enlace predeterminada** , escriba la dirección IP de la puerta de enlace predeterminada.  
  
8.  En la **servidor DNS preferido** , escriba la dirección IP del servidor DNS.  
  
    > [!NOTE]
    >  Usa la dirección IP que se asigna al adaptador de red DHCP (por ejemplo, 192.168) en lugar de una red de bucle invertido (por ejemplo, 127.0.0.1). Para averiguar la dirección IP asignada, ejecutar **ipconfig** en un símbolo del sistema.  
  
9. En la **servidor DNS alternativo** , escriba la dirección IP del servidor DNS alternativo, si existe.  
  
10. Haz clic en **Aceptar**y, a continuación, haz clic en **cerrar**.  
  
> [!IMPORTANT]
>  Asegúrate de que configures el enrutador para reenviarlos puertos 80 y 443 a la nueva dirección IP estática del servidor.  
  
##  <a name="BKMK_DNS"></a>Paso 3: Preparar un certificado y el registro de DNS para el servidor de ubicación de red  
 Para preparar un certificado y el registro de DNS para el servidor de ubicación de red, realiza las siguientes tareas:  
  
-   [Paso 3a: conceder todos los permisos a los usuarios autenticados de la plantilla de certificado de servidor s Web](#BKMK_GrantFullPermissions)  
  
-   [Paso 3 b: inscribir un certificado de servidor de la ubicación de red con un nombre común que es no puede resolverse de la red externa](#BKMK_EnrollaCertificate)  
  
-   [Paso 3c: agregar un nuevo host en el servidor DNS y se asigna a la dirección del servidor de Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>Paso 3a: conceder todos los permisos a los usuarios autenticados de la plantilla de certificado de servidor s Web  
 Es tu primera tarea conceder permisos completos para autenticar los usuarios de la plantilla de certificado s de servidor Web de la entidad de certificación.  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>Para conceder permisos completos para usuarios autenticados para el servidor Web plantilla de certificado s  
  
1.  En la **inicio** página abierta **entidad de certificación**.  
  
2.  En el árbol de consola, en **entidad de certificación (locales)**, expanda **< servername\ >-CA**, haz clic en **plantillas de certificado**y, a continuación, haz clic en **administrar**.  
  
3.  En **entidad de certificación (locales)**, haz clic en **servidor Web**y, a continuación, haz clic en **propiedades**.  
  
4.  En Propiedades del servidor Web, en la **seguridad** , haga clic **usuarios autenticados**, selecciona **Control total**y, a continuación, haz clic en **Aceptar**.  
  
5.  Reiniciar **servicios de certificados de Active Directory**. En el Panel de Control, abre **ver servicios locales**. En la lista de servicios, haz clic en **servicios de certificados de Active Directory**y, a continuación, haz clic en **reiniciar**.  
  
###  <a name="BKMK_EnrollaCertificate"></a>Paso 3 b: inscribir un certificado de servidor de la ubicación de red con un nombre común que es no puede resolverse de la red externa  
 A continuación, inscribir un certificado de servidor de la ubicación de red con un nombre común que es no puede resolverse de la red externa.  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>Para inscribir un certificado del servidor de ubicación de red  
  
1.  En la **inicio** página abierta **mmc** (Microsoft Management Console).  
  
2.  Si un **Control de cuentas de usuario** aparece el mensaje de advertencia, haz clic en **Sí**.  
  
     Abre Microsoft Management Console (MMC).  
  
3.  En la **archivo** menú, haz clic en **agregar o quitar complementos**.  
  
4.  En la **agregar o complementos remotos** cuadro, haz clic en **certificados**y, a continuación, haz clic en **agregar**.  
  
5.  En la **certificados, complemento** página, haz clic en **cuenta de equipo**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **Seleccionar equipo** página, haz clic en **equipo Local**, haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.  
  
7.  En el árbol de consola, expande **certificados (equipo Local)**, expanda **Personal**, haz clic en **certificados**y, a continuación, en **todas las tareas**, haz clic en **solicitar un nuevo certificado**.  
  
8.  Cuando aparezca el Asistente para la inscripción de certificados, haz clic en **siguiente**.  
  
9. En la **selecciona Directiva de inscripción de certificados** página, haz clic en **siguiente**.  
  
10. En la **solicitar certificados** página, seleccione la **servidor Web** y, a continuación, haz clic en **se necesita más información para inscribir este certificado**.  
  
11. En la **propiedades de certificado** cuadro, escribe la siguiente configuración para **nombre de sujeto**:  
  
    1.  Para **tipo**, selecciona **nombre común**.  
  
    2.  Para **valor**, escribe el nombre del servidor de ubicación de red (por ejemplo, DirectAccess-NLS.contoso.local) y, a continuación, haz clic en **agregar**.  
  
    3.  Haz clic en **Aceptar**y, a continuación, haz clic en **inscribir**.  
  
12. Cuando se completa la inscripción de certificados, haz clic en **finalizar**.  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>Paso 3c: agregar un nuevo host en el servidor DNS y se asigna a la dirección del servidor de Windows Server Essentials  
 Para completar la configuración de DNS, agregar un nuevo host en el servidor DNS y se asigna a la dirección del servidor de Windows Server Essentials.  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>Para asignar un nuevo host a la dirección del servidor de Windows Server Essentials  
  
1.  En la página de inicio, abre el Administrador de DNS. Para abrir el Administrador de DNS, buscar **dnsmgmt.msc**y, a continuación, haz clic en **dnsmgmt.msc** en los resultados.  
  
2.  En el árbol de consola del Administrador de DNS, expanda el servidor local, **zonas de búsqueda directa**, haz clic en la zona con el sufijo de dominio del servidor s y, a continuación, haz clic en **nuevo Host (A o AAAA)**.  
  
3.  Escribe el nombre y dirección IP del servidor (por ejemplo, DirectAccess-NLS.contoso.local) y su correspondiente dirección del servidor (por ejemplo, 192.168).  
  
4.  Haz clic en **agregar Host**, haz clic en **Aceptar**y, a continuación, haz clic en **listo**.  
  
##  <a name="BKMK_AddSecurityGroup"></a>Paso 4: Crear un grupo de seguridad para los equipos de cliente de DirectAccess  
 A continuación, crear un grupo de seguridad que se usará para los equipos cliente de DirectAccess y, a continuación, agrega las cuentas de equipo al grupo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Para agregar un grupo de seguridad para los equipos cliente que usan DirectAccess  
  
1.  En el panel del administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
  
    > [!NOTE]
    >  Si no ves **equipos y usuarios de Active Directory** en la **herramientas** menú, debes instalar la característica. Para instalar los usuarios de Active Directory y grupos, ejecuta el siguiente cmdlet de Windows PowerShell como administrador: `Install-WindowsFeature RSAT-ADDS-Tools`. Para obtener más información, consulta [instalar o quitar el paquete de herramienta de administración remota del servidor](https://technet.microsoft.com/library/cc730825.aspx).  
  
2.  En el árbol de consola, expande el servidor, haz clic en **usuarios**, haz clic en **nueva**y, a continuación, haz clic en **grupo**.  
  
3.  Escribe un nombre de grupo, el ámbito del grupo y el tipo de grupo (crean un grupo de seguridad) y, a continuación, haz clic en **Aceptar**.  
  
 El nuevo grupo de seguridad se agrega a la **usuarios** carpeta.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Para agregar cuentas de equipo en el grupo de seguridad  
  
1.  En el panel del administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
  
2.  En el árbol de consola, expande el servidor y, a continuación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario y grupos de seguridad en el servidor, haz clic en el grupo de seguridad que creaste para DirectAccess y, a continuación, haz clic en **propiedades**.  
  
4.  En la **miembros** , haga clic **agregar**.  
  
5.  En el cuadro de diálogo, escriba los nombres de las cuentas de equipo que quieras agregar al grupo, separándolos con un punto y coma (;). A continuación, haz clic en **comprobar nombres**.  
  
6.  Después de que se validan las cuentas de equipo, haz clic en **Aceptar**. A continuación, haz clic en **Aceptar** nuevamente.  
  
> [!NOTE]
>  También puedes usar la **miembro de** ficha en las propiedades de la cuenta de equipo para agregar la cuenta al grupo de seguridad.  
  
##  <a name="BKMK_EnableConfigureDA"></a>Paso 5: Habilitar y configurar DirectAccess  
 Para habilitar y configurar DirectAccess en Windows Server Essentials, debes realizar los siguientes pasos:  
  
-   [Paso 5a: habilitar DirectAccess mediante la consola de administración de acceso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Paso 5b: quitar el IPv6Prefix no válido en el GPO de RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Paso 5c: activar equipos cliente que ejecutan Windows 7 Enterprise usar DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Paso 5d: configurar el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Paso 5e: agregar una clave del registro para omitir la certificación de la CA cuando se establece un canal de IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>Paso 5a: habilitar DirectAccess mediante la consola de administración de acceso remoto  
 Esta sección proporciona instrucciones paso a paso para habilitar DirectAccess en Windows Server Essentials. Si no has configurado aún VPN en el servidor, debes hacerlo antes de iniciar este procedimiento. Para obtener instrucciones, consulta [administrar VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Para habilitar DirectAccess mediante la consola de administración de acceso remoto  
  
1.  En la página de inicio, abre **administración de acceso remoto**.  
  
2.  En el Asistente para habilitar DirectAccess, haz lo siguiente:  
  
    1.  Revisión **requisitos previos de DirectAccess**y haz clic en **siguiente**.  
  
    2.  En la **seleccionar grupos** pestaña, agrega el grupo de seguridad que creaste anteriormente para los clientes de DirectAccess. (Si no has creado el grupo de seguridad, consulta [paso 4: crear un grupo de seguridad de cliente de DirectAccess equipos](#BKMK_AddSecurityGroup) para obtener instrucciones.)  
  
    3.  En la **seleccionar grupos** , haga clic **habilitar DirectAccess para equipos portátiles solo** si desea permitir que los equipos móviles usar DirectAccess para acceder al servidor de forma remota y, a continuación, haz clic en **siguiente**.  
  
    4.  En **topología de red**, selecciona la topología del servidor y, a continuación, haz clic en **siguiente**.  
  
    5.  En **DNS Suffix Search List**, agregar el sufijo DNS adicional para los equipos cliente, si es necesario y, a continuación, haz clic en **siguiente**.  
  
        > [!NOTE]
        >  De manera predeterminada, el Asistente para habilitar DirectAccess ya agrega el sufijo DNS para el dominio actual. Sin embargo, puedes agregar más si es necesario.  
  
    6.  Revisa los objetos de directiva de grupo (GPO) que se aplicarán y modificarla si es necesario.  
  
    7.  Haz clic en **siguiente**y, a continuación, haz clic en **finalizar**.  
  
    8.  Ejecuta el siguiente comando de Windows PowerShell en modo elevado para reiniciar el servicio de administración de acceso remoto:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>Paso 5b: quitar el IPv6Prefix no válido en el GPO de RRAS (solo Windows Server Essentials)  
  En esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
 Abre Windows PowerShell como administrador y ejecuta los siguientes comandos:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>Paso 5c: activar equipos cliente que ejecutan Windows 7 Enterprise usar DirectAccess  
 Si tienes equipos cliente que ejecutan Windows 7 Enterprise, completa el siguiente procedimiento para habilitar DirectAccess desde los equipos.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Para permitir que los equipos de Windows 7 Enterprise usar DirectAccess  
  
1.  En la página de inicio de servidor s, abre **administración de acceso remoto**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**. A continuación, en la **detalles de la instalación** panel, en **paso 2**, haz clic en **editar**.  
  
     Abre el Asistente para configuración de servidor de acceso remoto.  
  
3.  En la **autenticación** pestaña, elige el certificado de entidad de (certificación CA) de certificación que será el certificado raíz de confianza (puedes elegir el certificado de CA del servidor de Windows Server Essentials). Haz clic en **habilitar Windows 7 los equipos cliente para conectarse a través de DirectAccess**y, a continuación, haz clic en **siguiente**.  
  
4.  Sigue las instrucciones para completar al asistente.  
  
> [!IMPORTANT]
>  Hay un problema conocido para los equipos de Windows 7 Enterprise conectarse a través de DirectAccess si el servidor de Windows Server Essentials no incluía UR1 preinstalado. Para permitir conexiones de DirectAccess en el entorno, debes realizar estos pasos adicionales:  
>   
>  1.  Instalar la revisión descrita en [artículo de Microsoft Knowledge Base (KB) 2796394](https://support.microsoft.com/kb/2796394) en el servidor de Windows Server Essentials. A continuación, reiniciar el servidor.  
> 2.  A continuación, instalar la revisión descrita en [artículo de Microsoft Knowledge Base (KB) 2615847](https://support.microsoft.com/kb/2615847) en cada equipo con Windows 7.  
>   
>      Este problema se resolvió en Windows Server Essentials.  
  
###  <a name="BKMK_NLS"></a>Paso 5d: configurar el servidor de ubicación de red  
 Esta sección proporciona instrucciones paso a paso para configurar la configuración del servidor de ubicación de red.  
  
> [!NOTE]
>  Antes de comenzar, copia el contenido de la carpeta de \inetpub\wwwroot < SystemDrive\ > en la carpeta de Server\Bin\WebApps\Site\insideoutside Files\Windows < SystemDrive\ >. También copia el archivo de default.aspx desde la carpeta de < SystemDrive\ > Files\Windows Server\Bin\WebApps\Site a la carpeta de Server\Bin\WebApps\Site\insideoutside Files\Windows < SystemDrive\ >.  
  
##### <a name="to-configure-the-network-location-server"></a>Para configurar el servidor de ubicación de red  
  
1.  En la página de inicio, abre **administración de acceso remoto**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**y en la **configuración de acceso remoto** panel, se detallan en **paso 3**, haz clic en **editar**.  
  
3.  En el Asistente de instalación de servidor de acceso remoto, en la **servidor de ubicación de red** , selecciona **el servidor de ubicación de red se implementa en el servidor de acceso remoto**y, a continuación, selecciona el certificado que se emitió anteriormente (en [paso 3: preparar un certificado y el registro de DNS para el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Sigue las instrucciones para completar el asistente y, a continuación, haz clic en **finalizar**.  
  
###  <a name="BKMK_CA"></a>Paso 5e: agregar una clave del registro para omitir la certificación de la CA cuando se establece un canal de IPsec  
 El siguiente paso es configurar el servidor para omitir la certificación de la CA cuando se establezca un canal de IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Para agregar una clave del registro para omitir la certificación de la entidad de certificación  
  
1.  En la página de inicio, abre **regedit** (el Editor del registro).  
  
2.  En el Editor del registro, expanda **HKEY_LOCAL_MACHINE**, expanda **sistema**, expanda **CurrentControlSet**, expanda **servicios**y expande **IKEEXT**.  
  
3.  En **IKEEXT**, haz clic en **parámetros**, haz clic en **nueva**y, a continuación, haz clic en **DWORD (32 bits) valor**.  
  
4.  Cambie el valor recién agregado para **ikeflags**.  
  
5.  Haz doble clic en **ikeflags**, Establece la **tipo** a **Hexadecimal**, Establece el valor en **8000**y, a continuación, haz clic en **Aceptar**.  
  
> [!NOTE]
>  Puedes usar el siguiente comando de Windows PowerShell en modo elevado para agregar esta clave del registro:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>Paso 6: Establecer la configuración de la tabla de directiva de resolución de nombres para el servidor de DirectAccess  
 Esta sección proporciona instrucciones para editar los GPO de cliente de DirectAccess las entradas de la tabla de directiva de resolución de nombre (NPRT) para las direcciones internas (por ejemplo, las entradas con un sufijo contoso.local) y, a continuación, Establece la dirección de la interfaz IPHTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Para configurar las entradas de la tabla de directiva de resolución de nombres  
  
1.  En la página de inicio, abre **Group Policy Management**.  
  
2.  En la consola de administración de directivas de grupo, haz clic en el bosque de forma predeterminada y el dominio, haz clic en **configuración de cliente de DirectAccess**y, a continuación, haz clic en **editar**.  
  
3.  Haz clic en **configuraciones de equipo**, haz clic en **directivas**, haz clic en **configuración de Windows**y, a continuación, haz clic en **directiva de resolución de nombre**. Elige la entrada que tiene el espacio de nombres que sea idéntica al sufijo DNS y, a continuación, haz clic en **Editar regla**.  
  
4.  Haz clic en el **configuración DNS de DirectAccess** pestaña; a continuación, seleccione **habilitar la configuración DNS de DirectAccess en esta regla**. Agrega la dirección IPv6 para la interfaz de HTTPS de IP en la lista de servidores DNS.  
  
    > [!NOTE]
    >  Puedes usar el siguiente comando de Windows PowerShell para obtener la dirección IPv6:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>Paso 7: Configurar las reglas de firewall TCP y UDP para el servidor DirectAccess GPO  
 Esta sección incluye instrucciones paso a paso para configurar las reglas de firewall TCP y UDP para el servidor DirectAccess GPO.  
  
#### <a name="to-configure-firewall-rules"></a>Para configurar las reglas del firewall  
  
1.  En la página de inicio, abre **Group Policy Management**.  
  
2.  En la consola de administración de directivas de grupo, haz clic en el bosque de forma predeterminada y el dominio, haz clic en **configuración del servidor DirectAccess**y, a continuación, haz clic en **editar**.  
  
3.  Haz clic en **configuración del equipo**, haz clic en **directivas**, haz clic en **configuración de Windows**, haz clic en **la configuración de seguridad**, haz clic en **Firewall de Windows con seguridad avanzada**, haz clic en el siguiente nivel **Firewall de Windows con seguridad avanzada**y, a continuación, haz clic en **reglas de entrada**. Haz clic en **Server (TCP de entrada) de nombre de dominio**y, a continuación, haz clic en **propiedades**.  
  
4.  Haz clic en el **ámbito** ficha y en la **dirección IP Local** la lista, agrega la dirección IPv6 de la interfaz de IP HTTPS.  
  
5.  Repite el mismo procedimiento para **nombres de dominio (UDP de entrada)**.  
  
##  <a name="BKMK_DNS64"></a>Paso 8: Cambiar la configuración de DNS64 para escuchar la interfaz de IP HTTPS  
 Debes cambiar la configuración de DNS64 para escuchar la interfaz de HTTPS de IP mediante el siguiente comando de Windows PowerShell.  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>Paso 9: Reservar puertos para el servicio WinNat  
 Usa el siguiente comando de Windows PowerShell para reservar puertos para el servicio WinNat. Reemplaza "192.168.1.100" con la dirección IPv4 real de tu servidor de Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Para evitar conflictos de puerto con aplicaciones, asegúrate de que el intervalo de puertos se reserva para el servicio WinNat no incluye puertos 6602.  
  
##  <a name="BKMK_WinNAT"></a>Paso 10: Reiniciar el servicio de WinNat  
 Ejecuta el siguiente comando de Windows PowerShell para reiniciar el servicio de controladores de Windows NAT (WinNat).  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>Apéndice: Configurar DirectAccess mediante Windows PowerShell  
 En esta sección se describe cómo instalar y configurar DirectAccess mediante Windows PowerShell.  
  
### <a name="preparation"></a>Preparación  
 Antes de empezar a configurar el servidor para DirectAccess, debes realizar lo siguiente:  
  
1.  Siga el procedimiento de [paso 3: preparar un certificado y el registro de DNS para el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) para inscribir un certificado denominado **DirectAccess NLS.contoso.com** (donde **contoso.com** se reemplaza por el nombre del dominio real interno) y para agregar un registro DNS para el servidor de ubicación de red (NLS).  
  
2.  Agregar un grupo de seguridad llamado **DirectAccessClients** en Active Directory, y luego agregar los equipos cliente para el que quieres proporcionar la funcionalidad de DirectAccess. Para obtener más información, consulta [paso 4: crear un grupo de seguridad de cliente de DirectAccess equipos](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Comandos  
  
> [!IMPORTANT]
>  En Windows Server Essentials, no es necesario quitar el prefijo IPv6 innecesario GPO. Eliminar la sección de código con la siguiente etiqueta: `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
