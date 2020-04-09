---
title: Configurar DirectAccess en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d8029b954a5957433fb0fcc71d3bef610a187939
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819709"
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurar DirectAccess en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este tema se proporcionan instrucciones paso a paso para configurar DirectAccess en Windows Server Essentials con el fin de permitir que los trabajadores móviles se conecten sin problemas a la red de su organización desde cualquier ubicación remota equipada con Internet sin establecer una conexión de red privada virtual (VPN). DirectAccess puede ofrecer a los trabajadores móviles la misma experiencia de conectividad dentro y fuera de la oficina desde sus equipos Windows 8.1, Windows 8 y Windows 7.  
  
 En Windows Server Essentials, si el dominio contiene más de un servidor de Windows Server Essentials, DirectAccess debe estar configurado en el controlador de dominio.  
  
> [!NOTE]
>  En este tema se proporcionan instrucciones para configurar DirectAccess cuando el servidor de Windows Server Essentials es el controlador de dominio. Si el servidor de Windows Server Essentials es un miembro del dominio, siga las instrucciones para configurar DirectAccess en un miembro del dominio en [Agregar DirectAccess a una implementación de acceso remoto (VPN) existente](https://technet.microsoft.com/library/jj574220.aspx) en su lugar.  
  
## <a name="process-overview"></a>Información general del proceso  
 Para configurar DirectAccess en Windows Server Essentials, realice los pasos siguientes.  
  
> [!IMPORTANT]
>  Antes de usar los procedimientos de esta guía para configurar DirectAccess en Windows Server Essentials, debe habilitar la VPN en el servidor. Para obtener instrucciones, consulte [administrar VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Paso 1: agregar herramientas de administración de acceso remoto al servidor](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Paso 2: cambiar la dirección del adaptador de red del servidor a una dirección IP estática](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Paso 3: preparar un certificado y un registro DNS para el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Paso 3A: conceder permisos totales a los usuarios autenticados para la plantilla de certificado del servidor Web](#BKMK_GrantFullPermissions)  
  
    -   [Paso 3B: inscribir un certificado para el servidor de ubicación de red con un nombre común que no se pueda resolver desde la red externa](#BKMK_EnrollaCertificate)  
  
    -   [Paso 3c: agregar un nuevo host en el servidor DNS y asignarlo a la dirección del servidor de Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Paso 4: crear un grupo de seguridad para equipos cliente de DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Paso 5: habilitar y configurar DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Paso 5A: habilitación de DirectAccess mediante la consola de administración de acceso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Paso 5B: quitar el IPv6Prefix no válido en el GPO RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Paso 5C: habilitar equipos cliente que ejecutan Windows 7 Enterprise para usar DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Paso 5D: configurar el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Paso 5e: agregar una clave del registro para omitir la certificación de CA al establecer un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Paso 6: configurar las opciones de la tabla de directivas de resolución de nombres para el servidor de DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Paso 7: configurar las reglas de Firewall TCP y UDP para los GPO de servidor de DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Paso 8: cambiar la configuración de DNS64 para que escuche la interfaz IP-HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Paso 9: reservar puertos para el servicio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Paso 10: reinicio del servicio WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Apéndice: Instalar DirectAccess mediante Windows PowerShell](#BKMK_AppendixBPowerShellScript) proporciona un script de Windows PowerShell que puede usar para realizar la instalación de DirectAccess.  
  
##  <a name="step-1-add-remote-access-management-tools-to-your-server"></a><a name="BKMK_AddRAM"></a>Paso 1: agregar herramientas de administración de acceso remoto al servidor  
  
#### <a name="to-add-remote-accregss-management-tools--reg"></a>Para agregar herramientas de administración de&reg;SS remotas &reg;
  
1.  En el servidor, en la esquina inferior izquierda de la página de inicio, haga clic en el icono **Administrador del servidor**.  
  
     En Windows Server Essentials, tendrá que buscar Administrador del servidor para abrirlo. En la página de inicio, escriba **Server Manager** y, después, haga clic en **Administrador del servidor** en los resultados de búsqueda. Para anclar el administrador del servidor a la página de inicio, haga clic con el botón derecho en Administrador del servidor en los resultados de búsqueda y, después, haga clic en **Anclar a Inicio**.  
  
2.  Si se muestra un mensaje de advertencia de **Control de cuentas de usuario**, haga clic en **Sí**.  
  
3.  En el panel del administrador del servidor, haga clic en **Administrar** y, después, haga clic en **Agregar roles y características**.  
  
4.  En el Asistente para agregar roles y características, realice las siguientes acciones:  
  
    1.  En la página **Tipo de instalación**, haga clic en **Instalación basada en características o en roles**.  
  
    2.  En la **Página selección de servidor** (o en la página **Seleccionar servidor de destino** en Windows Server Essentials), haga clic en **seleccionar un servidor del grupo de servidores**.  
  
    3.  En la página **Características**, expanda **Herramientas de administración del servidor remoto (instalado)** , expanda **Herramientas de administración de acceso remoto (instalado)** , expanda **Herramientas de administración de roles (instalado)** , expanda **Herramientas de administración de acceso remoto** y, después, seleccione **Herramientas de línea de comandos y GUI de acceso remoto**.  
  
    4.  Siga las instrucciones para completar el asistente.  
  
##  <a name="step-2-change-the-network-adapter-address-of-the-server-to-a-static-ip-address"></a><a name="BKMK_AddStaticIP"></a>Paso 2: cambiar la dirección del adaptador de red del servidor a una dirección IP estática  
 DirectAccess requiere un adaptador con una dirección IP estática. Debe cambiar la dirección IP del adaptador de red local en su servidor.  
  
#### <a name="to-add-a-static-ip-address"></a>Para agregar una dirección IP estática  
  
1.  En la página de inicio, abra el **Panel de control**.  
  
2.  Haga clic en **Red e Internet** y en **Ver el estado y las tareas de red**.  
  
3.  En el panel de tareas del **Centro de redes y recursos compartidos**, haga clic en **Cambiar configuración del adaptador**.  
  
4.  Haga clic con el botón secundario en el adaptador de red local y, a continuación, haga clic en **Propiedades**.  
  
5.  En la pestaña **Funciones de red**, haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, luego, haga clic en **Propiedades**.  
  
6.  En la pestaña **General**, haga clic en **Usar la siguiente dirección IP** y escriba la dirección IP que desea usar.  
  
     En el cuadro **Máscara de subred** aparece automáticamente un valor predeterminado para la máscara de subred. Acepte el valor predeterminado o escriba el valor de máscara de subred que quiera usar.  
  
7.  En el cuadro **Puerta de enlace predeterminada**, escriba la dirección IP de la puerta de enlace predeterminada.  
  
8.  En el cuadro **Servidor DNS preferido**, escriba la dirección IP del servidor DNS.  
  
    > [!NOTE]
    >  Use la dirección IP que DHCP asignó a su adaptador de red, (por ejemplo, 192.168.X.X) en lugar de una red de bucle invertido (por ejemplo,127.0.0.1). Para obtener la dirección IP asignada, ejecute **ipconfig** en un símbolo del sistema.  
  
9. En el cuadro **Servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo, si lo hay.  
  
10. Haga clic en **Aceptar** y, a continuación, en **Cerrar**.  
  
> [!IMPORTANT]
>  Asegúrese de que configura el enrutador para que derive los puertos 80 y 443 a la nueva dirección IP estática del servidor.  
  
##  <a name="step-3-prepare-a-certificate-and-dns-record-for-the-network-location-server"></a><a name="BKMK_DNS"></a>Paso 3: preparar un certificado y un registro DNS para el servidor de ubicación de red  
 Para preparar un certificado y un registro DNS para el servidor de ubicación de red, realice las siguientes tareas:  
  
-   [Paso 3A: conceder permisos totales a los usuarios autenticados para la plantilla de certificado del servidor Web](#BKMK_GrantFullPermissions)  
  
-   [Paso 3B: inscribir un certificado para el servidor de ubicación de red con un nombre común que no se pueda resolver desde la red externa](#BKMK_EnrollaCertificate)  
  
-   [Paso 3c: agregar un nuevo host en el servidor DNS y asignarlo a la dirección del servidor de Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="step-3a-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_GrantFullPermissions"></a>Paso 3A: conceder permisos totales a los usuarios autenticados para la plantilla de certificado del servidor Web  
 La primera tarea consiste en conceder permisos completos para autenticar a los usuarios de la plantilla de certificado del servidor Web en la entidad de certificación.  
  
####  <a name="to-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_ToGrantFullPermissions"></a>Para conceder permisos totales a los usuarios autenticados para la plantilla de certificado del servidor Web  
  
1.  En la página **Inicio**, abra **Entidad de certificación**.  
  
2.  En el árbol de consola, en **entidad de certificación (local)** , expanda **< NOMBREDESERVIDOR\>-CA**, haga clic con el botón secundario en **plantillas de certificado**y, a continuación, haga clic en **administrar**.  
  
3.  En **Entidad de certificación (Local)** , haga clic con el botón derecho en **Servidor Web** y, después, haga clic en **Propiedades**.  
  
4.  En las propiedades del servidor web, en la pestaña **Seguridad**, haga clic en **Usuarios autenticados**, seleccione **Control total** y, después, haga clic en **Aceptar**.  
  
5.  Reinicie **Servicios de certificados de Active Directory**. En el Panel de control, abra **Ver servicios locales**. En la lista de servicios, haga clic con el botón derecho en **Servicios de certificados de Active Directory** y, después, haga clic en **Reiniciar**.  
  
###  <a name="step-3b-enroll-a-certificate-for-the-network-location-server-with-a-common-name-that-is-unresolvable-from-the-external-network"></a><a name="BKMK_EnrollaCertificate"></a>Paso 3B: inscribir un certificado para el servidor de ubicación de red con un nombre común que no se pueda resolver desde la red externa  
 A continuación, inscriba un certificado para el servidor de ubicación de red con un nombre común que no se pueda resolver desde la red externa.  
  
####  <a name="to-enroll-a-certificate-for-the-network-location-server"></a><a name="BKMK_ToEnrollaCertificate"></a>Para inscribir un certificado para el servidor de ubicación de red  
  
1.  En la página **Inicio**, abra **mmc** (Microsoft Management Console).  
  
2.  Si se muestra un mensaje de advertencia de **Control de cuentas de usuario**, haga clic en **Sí**.  
  
     Se abrirá Microsoft Management Console (MMC).  
  
3.  En el menú **Archivo**, haga clic en **Agregar o quitar complementos**.  
  
4.  En el cuadro **Agregar o quitar complementos**, haga clic en **Certificados** y, después, haga clic en **Agregar**.  
  
5.  En la página **Complemento Certificados**, haga clic en **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  En la página **Seleccionar equipo** , haga clic en **Equipo local**, haga clic en **Finalizar**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el árbol de consola, expanda **Certificados (equipo local)** , expanda **Personal**, haga clic con el botón derecho en **Certificados**y, después, en **Todas las tareas**, haga clic en **Solicitar un nuevo certificado**.  
  
8.  Cuando aparezca el Asistente para inscripción de certificados, haga clic en **Siguiente**.  
  
9. En la página **Seleccionar directiva de inscripción de certificados**, haga clic en **Siguiente**.  
  
10. En la página **Solicitar certificados**, active la casilla **Servidor Web** y, después, haga clic en **Se requiere más información para poder inscribir este certificado**.  
  
11. En el cuadro **Propiedades de certificado**, especifique la configuración siguiente para **Nombre de sujeto**:  
  
    1.  En **Tipo**, seleccione **Nombre común**.  
  
    2.  En **Valor**, escriba el nombre del servidor de ubicación de red (por ejemplo, DirectAccess-NLS.contoso.local) y, después, haga clic en **Agregar**.  
  
    3.  Haga clic en **Aceptar** y, después, haga clic en **Inscribir**.  
  
12. Cuando finalice la inscripción de certificados, haga clic en **Finalizar**.  
  
###  <a name="step-3c-add-a-new-host-on-the-dns-server-and-map-it-to-the-windows-server-essentials-server-address"></a><a name="BKMK_MapNewHosttoServerAddress"></a>Paso 3c: agregar un nuevo host en el servidor DNS y asignarlo a la dirección del servidor de Windows Server Essentials  
 Para completar la configuración de DNS, agregue un nuevo host en el servidor DNS y asígnelo a la dirección del servidor de Windows Server Essentials.  
  
####  <a name="to-map-a-new-host-to-the-windows-server-essentials-server-address"></a><a name="BKMK_ToMapNewHosttoServerAddress"></a>Para asignar un nuevo host a la dirección del servidor de Windows Server Essentials  
  
1.  En la página de inicio, abra el Administrador de DNS. Para abrir el Administrador de DNS, busque **dnsmgmt.msc**y, después, haga clic en **dnsmgmt.msc** en los resultados.  
  
2.  En el árbol de la consola del administrador de DNS, expanda el servidor local, expanda **zonas de búsqueda directa**, haga clic con el botón secundario en la zona con el sufijo de dominio del servidor y, a continuación, haga clic en **host nuevo (a o aaaa)** .  
  
3.  Escriba el nombre y la dirección IP del servidor (por ejemplo, DirectAccess-NLS.contoso.local) y su dirección de servidor correspondiente (por ejemplo, 192.168.x.x).  
  
4.  Haga clic en **Agregar host**, haga clic en **Aceptar** y, después, haga clic en **Listo**.  
  
##  <a name="step-4-create-a-security-group-for-directaccess-client-computers"></a><a name="BKMK_AddSecurityGroup"></a>Paso 4: crear un grupo de seguridad para equipos cliente de DirectAccess  
 A continuación, cree un grupo de seguridad que se utilizará para los equipos cliente de DirectAccess y agregue las cuentas de equipo al grupo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Para agregar un grupo de seguridad para los equipos cliente que usan DirectAccess  
  
1. En el panel del administrador del servidor, haga clic en **Herramientas** y en **Equipos y usuarios de Active Directory**.  
  
   > [!NOTE]
   >  Si no ve **Equipos y usuarios de Active Directory** en el menú **Herramientas**, debe instalar la característica. Para instalar Grupos y usuarios de Active Directory, ejecute el siguiente cmdlet de Windows PowerShell como administrador: `Install-WindowsFeature RSAT-ADDS-Tools`. Para obtener más información, consulte [Instalación o eliminación del paquete de Herramientas de administración remota del servidor](https://technet.microsoft.com/library/cc730825.aspx).  
  
2. En el árbol de consola, expanda el servidor, haga clic con el botón derecho en **Usuarios**, haga clic en **Nuevo** y, después, haga clic en **Grupo**.  
  
3. Escriba un nombre, el ámbito y el tipo del grupo (cree un grupo de seguridad) y haga clic en **Aceptar**.  
  
   El nuevo grupo de seguridad se agregará a la carpeta **Usuarios**.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Para agregar cuentas de equipo al grupo de seguridad  
  
1.  En el panel del administrador del servidor, haga clic en **Herramientas** y en **Equipos y usuarios de Active Directory**.  
  
2.  En el árbol de consola, expanda el servidor y haga clic en **Usuarios**.  
  
3.  En la lista de cuentas de usuario y grupos de seguridad en el servidor, haga clic con el botón derecho en el grupo de seguridad que creó para DirectAccess y, después, haga clic en **Propiedades**.  
  
4.  En la pestaña **Miembros**, haga clic en **Agregar**.  
  
5.  En el cuadro de diálogo, escriba los nombres de las cuentas de equipo que desea agregar al grupo, separando los nombres con un punto y coma (;). Haga clic en **Comprobar nombres**.  
  
6.  Una vez validadas las cuentas de equipo, haga clic en **Aceptar**. Haga clic en **Aceptar** de nuevo.  
  
> [!NOTE]
>  También puede usar la pestaña **Miembro de** en las propiedades de la cuenta de equipo para agregar la cuenta al grupo de seguridad.  
  
##  <a name="step-5-enable-and-configure-directaccess"></a><a name="BKMK_EnableConfigureDA"></a>Paso 5: habilitar y configurar DirectAccess  
 Para habilitar y configurar DirectAccess en Windows Server Essentials, debe completar los pasos siguientes:  
  
-   [Paso 5A: habilitación de DirectAccess mediante la consola de administración de acceso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Paso 5B: quitar el IPv6Prefix no válido en el GPO RRAS (solo Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Paso 5C: habilitar equipos cliente que ejecutan Windows 7 Enterprise para usar DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Paso 5D: configurar el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Paso 5e: agregar una clave del registro para omitir la certificación de CA al establecer un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="step-5a-enable-directaccess-by-using-the-remote-access-management-console"></a><a name="BKMK_EnableDA"></a>Paso 5A: habilitación de DirectAccess mediante la consola de administración de acceso remoto  
 En esta sección se proporcionan instrucciones paso a paso para habilitar DirectAccess en Windows Server Essentials. Si no ha configurado aún una VPN en el servidor, debe hacerlo antes de iniciar este procedimiento. Para obtener instrucciones, consulte [administrar VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Para habilitar DirectAccess usando la consola de administración de acceso remoto  
  
1.  En la página de inicio, abra **Administración de acceso remoto**.  
  
2.  En el Asistente para habilitar DirectAccess, realice estas acciones:  
  
    1.  Revise los **Requisitos previos de DirectAccess** y haga clic en **Siguiente**.  
  
    2.  En la pestaña **Seleccionar grupos**, agregue el grupo de seguridad que creó anteriormente para los clientes de DirectAccess. (Si no ha creado el grupo de seguridad, consulte [paso 4: crear un grupo de seguridad para equipos cliente de DirectAccess](#BKMK_AddSecurityGroup) para obtener instrucciones).  
  
    3.  En la pestaña **Seleccionar grupos**, haga clic en **Habilitar DirectAccess solo para equipos móviles** si desea permitir que los equipos móviles usen DirectAccess para acceder al servidor de forma remota y, a continuación, haga clic en **Siguiente**.  
  
    4.  En **Topología de red**, seleccione la topología del servidor y, a continuación, haga clic en **Siguiente**.  
  
    5.  En **Lista de búsqueda de sufijos DNS**, agregue los sufijos DNS adicionales para los equipos cliente, si fuera necesario, y haga clic en **Siguiente**.  
  
        > [!NOTE]
        >  De forma predeterminada, el Asistente para habilitar DirectAccess ya agrega el sufijo DNS para el dominio actual. Sin embargo, puede agregar más si fuera necesario.  
  
    6.  Revise los objetos de directiva de grupo (GPO) que se aplicarán y modifíquelos, si fuera necesario.  
  
    7.  Haga clic en **Siguiente**y después en **Finalizar**.  
  
    8.  Reinicie el servicio de administración de acceso remoto ejecutando el siguiente comando de Windows PowerShell en modo con privilegios elevados:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="step-5b-remove-the-invalid-ipv6prefix-in-rras-gpo-windows-server-essentials-only"></a><a name="BKMK_RemoveIPv6"></a>Paso 5B: quitar el IPv6Prefix no válido en el GPO RRAS (solo Windows Server Essentials)  
  Esta sección se aplica a un servidor que ejecuta Windows Server Essentials.  
  
 Abra Windows PowerShell como administrador y ejecute los siguientes comandos:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="step-5c-enable-client-computers-running-windows-7-enterprise-to-use-directaccess"></a><a name="BKMK_Step4cWindows7Setup"></a>Paso 5C: habilitar equipos cliente que ejecutan Windows 7 Enterprise para usar DirectAccess  
 Si tiene equipos cliente que ejecutan Windows 7 Enterprise, complete el siguiente procedimiento para habilitar DirectAccess desde esos equipos.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Para permitir que los equipos de Windows 7 Enterprise usen DirectAccess  
  
1.  En la página de inicio del servidor, Abra **Administración de acceso remoto**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. A continuación, en el panel **Detalles de configuración**, en el **Paso 2**, haga clic en **Editar**.  
  
     Se abrirá el Asistente para la instalación del servidor de acceso remoto.  
  
3.  En la pestaña **autenticación** , elija el certificado de la entidad de certificación (CA) que será el certificado raíz de confianza (puede elegir el certificado de CA del servidor de Windows Server Essentials). Haga clic en **Habilitar los equipos cliente con Windows 7 para que se conecten mediante DirectAccess** y, después, haga clic en **Siguiente**.  
  
4.  Siga las instrucciones para completar el asistente.  
  
> [!IMPORTANT]
>  Existe un problema conocido con los equipos de Windows 7 Enterprise que se conectan a través de DirectAccess si el servidor de Windows Server Essentials no se incluía con UR1 preinstalado. Para habilitar las conexiones de DirectAccess en el entorno, debe realizar estos pasos adicionales:  
> 
> 1. Instale la revisión descrita en el [artículo 2796394 de Microsoft Knowledge base (KB)](https://support.microsoft.com/kb/2796394) en el servidor de Windows Server Essentials. Reinicie el servidor.  
>    2. A continuación, instale la revisión descrita en el [artículo 2615847 de Microsoft Knowledge base (KB)](https://support.microsoft.com/kb/2615847) en cada equipo con Windows 7.  
> 
>    Este problema se resolvió en Windows Server Essentials.  
  
###  <a name="step-5d-configure-the-network-location-server"></a><a name="BKMK_NLS"></a>Paso 5D: configurar el servidor de ubicación de red  
 Esta sección proporciona instrucciones paso a paso para configurar las opciones del servidor de ubicación de red.  
  
> [!NOTE]
>  Antes de comenzar, copie el contenido de la carpeta < unidadDelSistema\>\Inetpub\wwwroot en la carpeta < unidadDelSistema\>\Archivos de Programa\windows Server\bin\webapps\site\insideoutside. Copie también el archivo default. aspx de la carpeta < SystemDrive\>\Archivos de Programa\windows Server\Bin\WebApps\Site a la carpeta < SystemDrive\>\Archivos de Programa\windows Server\bin\webapps\site\insideoutside.  
  
##### <a name="to-configure-the-network-location-server"></a>Para configurar el servidor de ubicación de red  
  
1.  En la página de inicio, abra **Administración de acceso remoto**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración** y, en el panel de detalles **Instalación de acceso remoto**, en el **Paso 3**, haga clic en **Editar**.  
  
3.  En el Asistente para la instalación del servidor de acceso remoto, en la pestaña **Servidor de ubicación de red** , seleccione **El servidor de ubicación de red se implementa en el servidor de acceso remoto**y, después, seleccione el certificado emitido anteriormente (en el [Step 3: Prepare a certificate and DNS record for the network location server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Siga las instrucciones para completar el asistente y, a continuación, haga clic en **Finalizar**.  
  
###  <a name="step-5e-add-a-registry-key-to-bypass-ca-certification-when-you-establish-an-ipsec-channel"></a><a name="BKMK_CA"></a>Paso 5e: agregar una clave del registro para omitir la certificación de CA al establecer un canal IPsec  
 El siguiente paso consiste en configurar el servidor para pasar por alto los certificados de CA cuando se establezca un canal IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Para agregar una clave de Registro para omitir la certificación de CA  
  
1.  En la página de inicio, abra **regedit** (el Editor del Registro).  
  
2.  En el Editor del Registro, expanda **HKEY_LOCAL_MACHINE**, expanda **System**, expanda **CurrentControlSet**, expanda **Services** y expanda **IKEEXT**.  
  
3.  En **IKEEXT**, haga clic con el botón derecho en **Parámetros**, haga clic en **Nuevo** y haga clic en **Valor DWORD (32 bits)** .  
  
4.  Cambie el nombre del valor recién agregado por **ikeflags**.  
  
5.  Haga doble clic en **ikeflags**, establezca el **Tipo** en **Hexadecimal**, establezca el valor en **8000** y, después, haga clic en **Aceptar**.  
  
> [!NOTE]
>  Puede usar el siguiente comando de Windows PowerShell en modo con privilegios elevados para agregar esta clave del Registro:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="step-6-configure-name-resolution-policy-table-settings-for-the-directaccess-server"></a><a name="BKMK_NRPT"></a>Paso 6: configurar las opciones de la tabla de directivas de resolución de nombres para el servidor de DirectAccess  
 Esta sección ofrece instrucciones para editar las entradas de la Tabla de directivas de resolución de nombres (NPRT) para las direcciones internas (por ejemplo, entradas con un sufijo contoso.local) para los GPO cliente de DirectAccess y, después, establecer la dirección de la interfaz IPHTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Para configurar las entradas de la tabla de directivas de resolución de nombres  
  
1.  En la página de inicio, abra **Administración de directivas de grupo**.  
  
2.  En la consola Administración de directivas de grupo, haga clic en el dominio y bosque predeterminado, haga clic con el botón secundario en **Configuración del cliente de DirectAccess** y, después, haga clic en **Editar**.  
  
3.  Haga clic en **Configuraciones de equipos**, en **Directivas**, en **Configuración de Windows** y en **Directiva de resolución de nombres**. Seleccione la entrada cuyo espacio de nombres sea idéntico a su sufijo DNS y, a continuación, haga clic en **Editar regla**.  
  
4.  Haga clic en la pestaña **Configuración de DNS para DirectAccess** y, después, haga clic en **Habilitar configuración de DNS para DirectAccess en esta regla**. Agregue la dirección IPv6 de la interfaz IP-HTTPS a la lista de servidores DNS.  
  
    > [!NOTE]
    >  Puede usar el siguiente comando de Windows PowerShell para obtener la dirección IPv6:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="step-7-configure-tcp-and-udp-firewall-rules-for-the-directaccess-server-gpos"></a><a name="BKMK_TCPUDP"></a>Paso 7: configurar las reglas de Firewall TCP y UDP para los GPO de servidor de DirectAccess  
 Esta sección incluye instrucciones paso a paso para configurar las reglas de firewall TCP y UDP para los GPO de servidor de DirectAccess.  
  
#### <a name="to-configure-firewall-rules"></a>Para configurar las reglas del firewall  
  
1.  En la página de inicio, abra **Administración de directivas de grupo**.  
  
2.  En la consola Administración de directivas de grupo, haga clic en el dominio y bosque predeterminado, haga clic con el botón derecho en **Configuración del servidor de DirectAccess** y, después, haga clic en **Editar**.  
  
3.  Haga clic en **Configuración del equipo**, en **Directivas**, en **Configuración de Windows**, en **Configuración de seguridad**, en **Firewall de Windows con seguridad avanzada**, en **Firewall de Windows con seguridad avanzada** y, después, haga clic en **Reglas de entrada**. Haga clic con el botón secundario en **Servidor de nombres de dominio (TCP de entrada)** y, a continuación, haga clic en **Propiedades**.  
  
4.  Haga clic en la pestaña **Ámbito** y, en la lista **Dirección IP local**, agregue la dirección IPv6 de la interfaz IP-HTTPS.  
  
5.  Repita el mismo procedimiento para **Servidor de nombres de dominio (UDP de entrada)** .  
  
##  <a name="step-8-change-the-dns64-configuration-to-listen-to-the-ip-https-interface"></a><a name="BKMK_DNS64"></a>Paso 8: cambiar la configuración de DNS64 para que escuche la interfaz IP-HTTPS  
 Debe cambiar la configuración de DNS64 para que escuche la interfaz IP-HTTPS usando el siguiente comando de Windows PowerShell.  
  
```powershell  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="step-9-reserve-ports-for-the-winnat-service"></a><a name="BKMK_ExemptPort"></a>Paso 9: reservar puertos para el servicio WinNat  
 Utilice el siguiente comando de Windows PowerShell para reservar puertos para el servicio WinNat. Reemplace "192.168.1.100" con la dirección IPv4 real de su servidor de Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Para evitar conflictos de puerto con las aplicaciones, asegúrese de que el intervalo de puerto que se reserva para el servicio WinNat no incluye el puerto 6602.  
  
##  <a name="step-10-restart-the-winnat-service"></a><a name="BKMK_WinNAT"></a>Paso 10: reinicio del servicio WinNat  
 Reinicie el servicio de controlador de Windows NAT (WinNat) ejecutando el siguiente comando de Windows PowerShell.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="appendix-set-up-directaccess-by-using-windows-powershell"></a><a name="BKMK_AppendixBPowerShellScript"></a>Apéndice: configurar DirectAccess mediante Windows PowerShell  
 Esta sección describe cómo instalar y configurar DirectAccess mediante Windows PowerShell.  
  
### <a name="preparation"></a>Preparación  
 Antes de comenzar a configurar su servidor para DirectAccess, debe completar los siguientes pasos:  
  
1.  Siga el procedimiento descrito en el [paso 3: preparar un certificado y un registro DNS para el servidor de ubicación de red](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) para inscribir un certificado denominado **DirectAccess-NLS.contoso.com** (donde **contoso.com** se reemplaza por el nombre de dominio interno real) y para agregar un registro DNS para el servidor de ubicación de red (NLS).  
  
2.  Agregue un grupo de seguridad denominado **DirectAccessClients** a Active Directory y, a continuación, agregue los equipos cliente para los que desea proporcionar la funcionalidad de DirectAccess. Para obtener más información, vea [paso 4: crear un grupo de seguridad para equipos cliente de DirectAccess](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Commands  
  
> [!IMPORTANT]
>  En Windows Server Essentials, no es necesario quitar el GPO de prefijo IPv6 innecesario. Elimine la sección de código con la etiqueta siguiente: `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
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
Set-DAServer -IPSecRootCertificate $rootcert[0]  
Set -DAClient -OnlyRemoteComputers Disabled -Downlevel Enabled  
  
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
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar el acceso desde cualquier lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
