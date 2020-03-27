---
title: Paso 2 configuración de servidores de DirectAccess avanzados
description: Este tema forma parte de la guía implementar un único servidor de DirectAccess con configuración avanzada para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: lizross
author: eross-msft
ms.openlocfilehash: bf740143c4d9c855df080addd75fdaeee6a1ceac
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309112"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>Paso 2 configuración de servidores de DirectAccess avanzados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema aprenderás a configurar las opciones de cliente y de servidor necesarias para una implementación de acceso remoto avanzado que use un único servidor de acceso remoto en un entorno donde se combinan IPv4 e IPv6. Antes de comenzar con los pasos de implementación, asegúrese de que ha completado los pasos de planeación que se describen en [planear una implementación de DirectAccess avanzada](Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|2.1. Instalar el rol de acceso remoto|Instalar el rol de acceso remoto.|  
|2.2. Configurar el tipo de implementación|Configura el tipo de implementación como DirectAccess y VPN, solo DirectAccess o solo VPN.|  
|[Planear una implementación avanzada de DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md)|Configura el servidor de acceso remoto con los grupos de seguridad que contengan clientes de DirectAccess.|  
|2.4. Configurar el servidor de acceso remoto|Configura los ajustes del servidor de acceso remoto.|  
|2.5. Configurar los servidores de infraestructura|Configura los servidores de infraestructura que se usan en la organización.|  
|2.6. Configurar servidores de aplicaciones|Configura los servidores de aplicaciones para que requieran autenticación y cifrado.|  
|2.7. Configuración de resumen y GPO alternativos|Consulta el resumen de configuración de acceso remoto y, si fuera necesario, modifica el GPO.|  
|2.8. Configuración del servidor de acceso remoto con Windows PowerShell|Configure el acceso remoto mediante Windows PowerShell.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-install-the-remote-access-role"></a><a name="BKMK_Role"></a>2,1. Instalar el rol de acceso remoto  
Para implementar acceso remoto debes instalar el rol de acceso remoto en un servidor de la organización que actúe como el servidor de acceso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
1.  En el servidor de acceso remoto, en la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haz clic en **Siguiente** tres veces para acceder a la pantalla **Seleccionar roles de servidor**.  
  
3.  En la página **Seleccionar roles de servidor**, selecciona **Acceso remoto**, haz clic en **Agregar características** y después haz clic en **Siguiente**.  
  
4.  Haz clic cinco veces en **Siguiente**.  
  
5.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
6.  En la página **Progreso de la instalación**, comprueba que la instalación se ha completado correctamente y haz clic en **Cerrar**.  
  
![progreso de la instalación correctos](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="22-configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>2,2. Configurar el tipo de implementación  
Acceso remoto se puede implementar mediante la consola de administración de acceso remoto de tres maneras:  
  
-   DirectAccess y VPN  
  
-   Solo DirectAccess  
  
-   Solo VPN  
  
Esta guía usa una implementación de solo DirectAccess en los procedimientos de ejemplo.  
  
#### <a name="to-configure-the-deployment-type"></a>Para configurar el tipo de implementación  
  
1.  En el servidor de acceso remoto, abra la consola de administración de acceso remoto: en la pantalla **Inicio** , escriba**RAMgmtUI. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En el panel central de la consola de administración de acceso remoto, haz clic en **Ejecutar el Asistente para la instalación de acceso remoto**.  
  
3.  En el cuadro de diálogo **Configurar acceso remoto**, haz clic en DirectAccess y VPN, DirectAccess solo o en VPN solo.  
  
## <a name="23-configure-directaccess-clients"></a><a name="BKMK_Clients"></a>2,3. Configurar los clientes de DirectAccess  
Para que un equipo cliente se aprovisione para el uso de DirectAccess, este debe pertenecer al grupo de seguridad seleccionado. Después de configurar DirectAccess, los equipos cliente del grupo de seguridad se aprovisionan para recibir el objeto de directiva de grupo (GPO) de DirectAccess. También se puede configurar el escenario de implementación, lo que permite configurar DirectAccess para el acceso de clientes y administración remota, o bien solo para administración remota.  
  
#### <a name="to-configure-directaccess-clients"></a>Cómo configurar los clientes de DirectAccess  
  
1.  En el área **Paso 1: Clientes remotos** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del cliente de DirectAccess, en la página **Escenario de implementación**, haz clic en el escenario de implementación que quieras usar en la organización (**DirectAccess completo** o **Solo administración remota**) y después haz clic en **Siguiente**.  
  
3.  En la página **Seleccionar grupos** , haz clic en **Agregar**.  
  
4.  En el cuadro de diálogo **Seleccionar grupos**, selecciona los grupos de seguridad que contienen los equipos cliente de DirectAccess.  
  
    > [!NOTE]  
    > Si el grupo de seguridad se encuentra en un bosque diferente al del servidor de acceso remoto, después de completar el Asistente para la instalación de acceso remoto, haga clic en **actualizar servidores de administración** en el panel de **tareas** para detectar los controladores de dominio y los servidores de Configuration Manager en el nuevo bosque.  
  
5.  Si fuera necesario, selecciona la casilla **Habilitar DirectAccess solo para equipos móviles** para que solo puedan acceder equipos móviles a la red interna.  
  
6.  Si fuera necesario, selecciona la casilla **Usar túnel forzado** para dirigir todo el tráfico de clientes (a la red interna y a internet) mediante el servidor de acceso remoto.  
  
7.  Haga clic en **Siguiente**.  
  
8.  En la página **Asistente para la conectividad de red**:  
  
    -   En la tabla, agrega los recursos que se usarán para determinar la conectividad a la red interna. Se creará automáticamente un sondeo web predeterminado si no se configuran otros recursos.  
  
        > [!CAUTION]  
        > Al configurar las ubicaciones de sondeo web para determinar la conectividad a la red empresarial deberás configurar como mínimo un sondeo basado en HTTP. Configurar únicamente un sondeo de **ping** no es suficiente y podría causar que no se detectara correctamente el estado de la conectividad. Esto se debe a que **ping** está exento de IPsec y, como resultado, no garantiza que se establezcan correctamente los túneles IPsec.  
  
    -   Agrega la dirección de correo del servicio de asistencia para permitir a los usuarios enviar información si tienen problemas de conectividad.  
  
    -   Especifica un nombre descriptivo para la conexión de DirectAccess. Este nombre aparece en la lista de redes cuando el usuario hace clic en el icono de red del área de notificación.  
  
    -   Si fuera necesario, selecciona la casilla **Permitir que los clientes de DirectAccess usen la resolución local de nombres**.  
  
        > [!NOTE]  
        > Al habilitar la resolución local de nombres, los usuarios que ejecuten el Asistente para la conectividad de red pueden seleccionar que se resuelvan los nombres mediante servidores DNS configurados en el equipo cliente de DirectAccess.  
  
9. Haga clic en **Finalizar**.  
  
## <a name="24-configure-the-remote-access-server"></a><a name="BKMK_Server"></a>2,4. Configurar el servidor de acceso remoto  
Para implementar acceso remoto es necesario configurar el servidor de acceso remoto con los adaptadores de red correctos, una URL pública para el servidor de acceso remoto a la que se puedan conectar los equipos cliente (la dirección ConnectTo), un certificado IP-HTTPS con un sujeto que coincida con la dirección ConnectTo, configuración de IPv6 y autenticación de equipos cliente.  
  
#### <a name="to-configure-the-remote-access-server"></a>Para configurar el servidor de acceso remoto  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En la página **Topología de red** del Asistente para la instalación del servidor de acceso remoto, haz clic en la topología de implementación que usarás en la organización. En **Escriba el nombre público o dirección IPv4 que usan los clientes para conectarse al servidor de acceso remoto**, escribe el nombre público de la implementación (este nombre coincide con el nombre del firmante del certificado IP-HTTPS, por ejemplo, edge1.contoso.com) y haz clic en **Siguiente**.  
  
3.  En la página **Adaptadores de red**, el asistente detectará automáticamente los adaptadores de red para las redes de la implementación. Si el asistente no detecta los adaptadores de red correctos, selecciona de forma manual los adaptadores correctos. El asistente también detecta automáticamente el certificado IP-HTTPS basándose en el nombre público para la implementación establecida en el paso anterior del asistente. Si el asistente no detecta automáticamente el certificado IP-HTTPS, haz clic en **Examinar** para seleccionar de forma manual el certificado correcto y después haz clic en **Siguiente**.  
  
4.  En la página **Configuración de prefijo** (que solo se muestra si se implementa IPv6 en la red interna), el asistente detectará automáticamente la configuración de IPv6 que se use en la red interna. Si la implementación requiere prefijos adicionales, configura los prefijos IPv6 para la red interna (un prefijo IPv6 para asignarlo a los equipos cliente de DirectAccess y un prefijo IPv6 para asignarlo a los equipos cliente de VPN).  
  
    > [!NOTE]  
    > Puedes especificar varios prefijos IPv6 mediante una lista delimitada por caracteres de punto y coma (por ejemplo, 2001:db8:1::/48;2001:db8:2::/48).  
  
5.  En la página **Autenticación**:  
  
    -   En **Autenticación de usuario**, haz clic en **Credenciales de Active Directory**. Para configurar una implementación mediante el uso de autenticación en dos fases, haz clic en **Autenticación en dos fases**. Para obtener más información, consulta [Implementar el acceso remoto con autenticación OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   Para implementaciones multisitio con autenticación en dos fases es necesario usar la autenticación de certificados de equipo. Activa la casilla **Usar certificados de equipo** para usar autenticación de certificados de equipo y después selecciona el certificado raíz IPsec.  
  
    -   Para permitir que los equipos cliente de Windows 7 se conecten a través de DirectAccess, active la casilla **permitir que los equipos cliente de Windows 7 se conecten a través de DirectAccess** .  
  
        > [!NOTE]  
        > Además, también es necesario usar la autenticación de certificados de equipo en este tipo de implementación.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="25-configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>2,5. Configurar los servidores de infraestructura  
Para configurar los servidores de infraestructura en una implementación de acceso remoto es necesario configurar el servidor de ubicación de red, la configuración DNS (incluida la lista de búsqueda de sufijos DNS) y, además, los servidores de administración que acceso remoto no detecte automáticamente.  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar los servidores de infraestructura  
  
1.  En el área **Paso 3: Servidores de infraestructura** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del servidor de infraestructura, en la página **Servidor de ubicación de red**, haz clic en la opción que se corresponda con la ubicación del servidor de ubicación de red de la implementación. Si el servidor de ubicación de red se encuentra en un servidor web remoto, especifica la URL y haz clic en **Validar** antes de continuar. Si el servidor de ubicación de red se encuentra en el servidor de acceso remoto, haz clic en **Examinar** para buscar el certificado correspondiente y después haz clic en **Siguiente**.  
  
3.  En la tabla de la página **DNS**, especifica los sufijos de nombre adicionales que se aplicarán como exenciones de la Tabla de directivas de resolución de nombres (NRPT). Selecciona una opción de resolución local de nombres y después haz clic en **Siguiente**.  
  
4.  En la página **Lista de búsqueda de sufijos DNS**, el servidor de acceso remoto detecta automáticamente los sufijos de dominio de la implementación. Usa los botones **Agregar** y **Quitar** para agregar y quitar sufijos de dominio de la lista de sufijos de dominio que quieras usar. Para agregar un nuevo sufijo de dominio, especifica el sufijo en el campo **Nuevo sufijo** y haz clic en **Agregar**. Haga clic en **Siguiente**.  
  
5.  En la página **Administración**, agrega los servidores de administración que no se detecten automáticamente y después haz clic en **Siguiente**. El acceso remoto agrega automáticamente controladores de dominio y servidores de Configuration Manager.  
  
    > [!NOTE]  
    > Aunque los servidores se agregan automáticamente, no aparecen en la lista. Después de aplicar la configuración por primera vez, los servidores Configuration Manager aparecen en la lista.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="26-configure-application-servers"></a><a name="BKMK_App"></a>2,6. Configurar servidores de aplicaciones  
La configuración de servidores de aplicaciones en las implementaciones de acceso remoto es una tarea opcional. Acceso remoto permite requerir autenticación para servidores de aplicaciones seleccionados (deberás agregarlos a un grupo de seguridad de servidores de aplicaciones). De manera predeterminada, el tráfico a los servidores de aplicaciones que requieren autenticación también se cifra; pero puedes seleccionar una opción para no cifrar el tráfico a los servidores de aplicaciones y usar solo autenticación.  
  
> [!NOTE]  
> La autenticación sin cifrado solo se admite en servidores de aplicaciones que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2.  
  
#### <a name="to-configure-application-servers"></a>Para configurar servidores de aplicaciones  
  
1.  En el área **Paso 4: Servidores de aplicaciones** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el asistente para la instalación del servidor de aplicaciones de DirectAccess, para requerir la autenticación en los servidores de aplicaciones seleccionados, haz clic en **Extender la autenticación a los servidores de aplicaciones seleccionados**. Haz clic en **Agregar** para seleccionar el grupo de seguridad Servidor de aplicaciones.  
  
3.  Para restringir el acceso a los servidores del grupo de seguridad Servidor de aplicaciones, selecciona la casilla **Permitir el acceso únicamente a los servidores incluidos en los grupos de seguridad**.  
  
4.  Para usar la autenticación sin cifrado, seleccione no **cifrar el tráfico. Casilla usar solo autenticación** .  
  
5.  Haga clic en **Finalizar**.  
  
## <a name="27-configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>2,7. Configuración de resumen y GPO alternativos  
Cuando completes la configuración de acceso remoto verás el cuadro de diálogo **Revisión de acceso remoto**. Podrás revisar todas las opciones que hayas seleccionado anteriormente, como por ejemplo:  
  
1.  **Configuración del GPO**: muestra el nombre del GPO del servidor de DirectAccess y el nombre del GPO del cliente. Además, puedes hacer clic en el vínculo **Cambiar** junto al encabezado **Configuración del GPO** para modificar la configuración del GPO.  
  
2.  **Clientes remotos**: muestra la configuración de los cliente de DirectAccess, incluidos el grupo de seguridad, el estado del túnel forzado, los comprobadores de conectividad y el nombre de la conexión de DirectAccess.  
  
3.  **Servidor de acceso remoto**: muestra la configuración de DirectAccess, incluidos el nombre o la dirección públicos, la configuración del adaptador de red, la información del certificado y la información de OTP (si está configurado).  
  
4.  **Servidores de infraestructura**: esta lista contiene la URL del servidor de ubicación de red, los sufijos DNS usados por los clientes de DirectAccess e información sobre el servidor de administración.  
  
5.  **Servidores de aplicación**: muestra el estado de administración remota de DirectAccess, así como el estado de la autenticación de un extremo a otro de los servidores de aplicaciones específicos.  
  
## <a name="28-how-to-configure-the-remote-access-server-by-using-windows-powershell"></a><a name="BKMK_PS"></a>2,8. Configuración del servidor de acceso remoto con Windows PowerShell  
![](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**comandos equivalentes** de Windows PowerShell Windows PowerShell  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
Para realizar una instalación completa en una topología perimetral de acceso remoto para DirectAccess solo en un dominio con el **Corp.contoso.com** raíz y con los parámetros siguientes: GPO de servidor: **configuración de servidor de DirectAccess**, GPO de cliente: configuración de cliente de DirectAccess, adaptador de red interno: **CorpNet**, adaptador de red externo: **Internet**, dirección ConnectTo: **edge1.contoso.com**y servidor de ubicación de red: **NLS.Corp.contoso.com**:  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
Para configurar el servidor de acceso remoto para que use autenticación de certificados de equipo con un certificado raíz IPsec emitido por la entidad de certificación llamada CORP-APP1-CA:  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
Para agregar el grupo de seguridad que contiene los clientes de DirectAccess llamado **ClientesDirectAccess** y eliminar el grupo de seguridad Equipos del dominio:  
  
```  
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')  
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')  
```  
  
Para habilitar el acceso remoto para todos los equipos (no solo portátiles y portátiles) y para habilitar el acceso remoto para los clientes de Windows 7:  
  
```  
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'  
```  
  
Para configurar la experiencia de cliente de DirectAccess, incluido el nombre descriptivo de la conexión y la URL del sondeo web:  
  
```  
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')  
```  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 1: configurar la infraestructura de DirectAccess avanzada](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>Paso siguiente  
  
-   [Paso 3: comprobar la implementación](Step-3-Verify-the-Deployment.md)  
  


