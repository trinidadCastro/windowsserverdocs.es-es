---
title: Paso 3 Configuración de la implementación multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ac6e231ac797d1ba1e8dfb314c6aa3df99ff91d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313961"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Paso 3 Configuración de la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de configurar la infraestructura multisitio, siga estos pasos para configurar la implementación multisitio de acceso remoto.  
  
|Tarea|Descripción|  
|----|--------|  
|3.1. Configurar servidores de acceso remoto|Configure servidores de acceso remoto adicionales mediante la configuración de direcciones IP, su unión al dominio y la instalación del rol de acceso remoto.|  
|3.2. Conceder acceso de administrador|Conceda privilegios en los servidores de acceso remoto adicionales al administrador de DirectAccess.|  
|3.3. Configuración de IP-HTTPS para una implementación multisitio|Configure el certificado IP-HTTPS que se usa en una implementación multisitio.|  
|3.4. Configurar el servidor de ubicación de red para una implementación multisitio|Configure el certificado del servidor de ubicación de red que se usa en una implementación multisitio.|  
|3.5. Configurar clientes de DirectAccess para una implementación multisitio|Quite los equipos cliente de Windows 7 de los grupos de seguridad de Windows 8.|  
|3.6. Habilitar la implementación multisitio|Habilitar la implementación multisitio en el primer servidor de acceso remoto.|  
|3,7. Agregar puntos de entrada a la implementación multisitio|Agregue puntos de entrada adicionales a la implementación multisitio.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-configure-remote-access-servers"></a><a name="BKMK_ConfigServer"></a>3.1. Configurar servidores de acceso remoto  

  
### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
1.  Asegúrese de que cada servidor de acceso remoto está configurado con la topología de implementación adecuada (perimetral, detrás de un NAT, una sola interfaz de red) y las rutas correspondientes.  
  
2.  Configure las direcciones IP en cada servidor de acceso remoto según la topología del sitio y el esquema de direcciones IP de su organización.  
  
3.  Una el servidor de acceso remoto a un dominio de Active Directory.  
  
4.  En la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
5.   Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
6.  En el cuadro de diálogo **Seleccionar roles de servidor** , seleccione **acceso remoto**y, a continuación, haga clic en **siguiente**.  
  
7.  Haga clic en **siguiente** tres veces.  
  
8.  En el cuadro de diálogo **seleccionar servicios de rol** , seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **Agregar características**.  
  
9.  Seleccione **enrutamiento**, **proxy de aplicación web**, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.  
  
10. Haga clic en **Siguiente** y, después, en **Instalar**.  
  
11.  En el cuadro de diálogo **Progreso de la instalación**, comprueba que la instalación se realiza correctamente y, a continuación, haz clic en **Cerrar**.  
  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  

  
Los pasos 1-3 deben realizarse manualmente y no se pueden realizar con este cmdlet de Windows PowerShell.  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="32-grant-administrator-access"></a><a name="BKMK_Admin"></a>3.2. Conceder acceso de administrador  
  
#### <a name="to-grant-administrator-permissions"></a>Para conceder permisos de administrador  
  
1.  En el servidor de acceso remoto en el punto de entrada adicional: en la pantalla **Inicio** , escriba **Administración de equipos**y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo, haga clic en **usuarios y grupos locales**.  
  
3.  Haga doble clic en **grupos**y, a continuación, haga doble clic en **administradores**.  
  
4.  En el cuadro de diálogo **propiedades de administradores** , haga clic en **Agregar**y, en el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , haga clic en **ubicaciones**.  
  
5.  En el cuadro de diálogo **ubicaciones** , en el árbol **Ubicación** , haga clic en la ubicación que contiene la cuenta de usuario del administrador de DirectAccess y, a continuación, haga clic en **Aceptar**.  
  
6.  En **Escriba los nombres de objeto que desea seleccionar**, escriba el nombre de usuario del administrador de DirectAccess y, a continuación, haga clic en **Aceptar** dos veces.  
  
7.  En el cuadro de diálogo **propiedades de administradores** , haga clic en **Aceptar**.  
  
8.  Cierre la ventana Administración de equipos.  
  
9. Repita este procedimiento en todos los servidores de acceso remoto que formarán parte de la implementación multisitio.  
  
## <a name="33-configure-ip-https-for-a-multisite-deployment"></a><a name="BKMK_IPHTTPS"></a>3.3. Configuración de IP-HTTPS para una implementación multisitio  
En cada servidor de acceso remoto que se va a agregar a la implementación multisitio, se requiere un certificado SSL para comprobar la conexión HTTPS con el servidor Web IP-HTTPS. La pertenencia al grupo local **Administradores**, o equivalente, es el requisito mínimo necesario para completar este procedimiento.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Para obtener un certificado IP-HTTPS  
  
1.  En cada servidor de acceso remoto: en la pantalla **Inicio** , escriba **MMC**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic sucesivamente en **Certificados**, **Agregar**, **Cuenta de equipo** y **Siguiente**, seleccione **Equipo local**, haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
    Si no aparece la plantilla de certificado de servidor Web, asegúrese de que la cuenta de equipo del servidor de acceso remoto tiene permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, vea [configurar permisos en la plantilla de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  En la pestaña **asunto** del cuadro de diálogo **propiedades de certificado** , en nombre de **sujeto**, en **tipo**, seleccione **nombre común**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) del nombre de Internet del servidor de acceso remoto (por ejemplo, Europe.contoso.com) y, a continuación, haga clic en **Agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado IP-HTTPS** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero facilitan la selección del certificado para IP-HTTPS al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores de acceso remoto de la implementación.  
  
## <a name="34-configure-the-network-location-server-for-a-multisite-deployment"></a><a name="BKMK_NLS"></a>3,4. Configurar el servidor de ubicación de red para una implementación multisitio  
Si seleccionó configurar el sitio web del servidor de ubicación de red en el servidor de acceso remoto al configurar el primer servidor, cada nuevo servidor de acceso remoto que agregue debe configurarse con un certificado de servidor Web que tenga el mismo nombre de sujeto que se seleccionó para el servidor de ubicación de red para el primer servidor. Cada servidor requiere un certificado para autenticar la conexión al servidor de ubicación de red y los equipos cliente ubicados en la red interna deben poder resolver el nombre del sitio web en DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar un certificado para la ubicación de red  
  
1.  En el servidor de acceso remoto: en la pantalla **Inicio** , escriba **MMC**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic sucesivamente en **Certificados**, **Agregar**, **Cuenta de equipo** y **Siguiente**, seleccione **Equipo local**, haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
    > [!NOTE]  
    > También puede importar el mismo certificado que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
    Si no aparece la plantilla de certificado de servidor Web, asegúrese de que la cuenta de equipo del servidor de acceso remoto tiene permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, vea [configurar permisos en la plantilla de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  En la pestaña **asunto** del cuadro de diálogo **propiedades de certificado** , en nombre de **sujeto**, en **tipo**, seleccione **nombre común**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) que se configuró para el certificado del servidor de ubicación de red del primer servidor de acceso remoto (por ejemplo, NLS.Corp.contoso.com) y, a continuación, haga clic en **Agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado de ubicación de red** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero facilitan la selección del certificado para la ubicación de red al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores de acceso remoto de la implementación.  
  
### <a name="to-create-the-network-location-server-dns-records"></a><a name="NLS"></a>Para crear los registros DNS del servidor de ubicación de red  
  
1.  En el servidor DNS: en la pantalla **Inicio** , escriba **DNSMgmt. msc**y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo de la consola del **Administrador de DNS** , abra la zona de búsqueda directa de la red interna. Haga clic con el botón secundario en la zona correspondiente y haga clic en **nuevo host (A o aaaa)** .  
  
3.  En el cuadro de diálogo **nuevo host** , en el cuadro **nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escriba el nombre que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto. En el cuadro **dirección IP** , escriba la dirección IPv4 de la intranet del servidor de acceso remoto y, a continuación, haga clic en **Agregar host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
4.  En el cuadro de diálogo **nuevo host** , en el cuadro **nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escriba el nombre que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto. En el cuadro **dirección IP** , escriba la dirección IPv6 accesible desde la intranet del servidor de acceso remoto y, a continuación, haga clic en **Agregar host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
5.  Repita los pasos 3 y 4 para cada servidor de acceso remoto de la implementación.  
  
6.  Haga clic en **Done** (Listo).  
  
7.  Repita este procedimiento antes de agregar los servidores como puntos de entrada adicionales en la implementación.  
  
## <a name="35-configure-directaccess-clients-for-a-multisite-deployment"></a><a name="BKMK_Client"></a>3,5. Configurar clientes de DirectAccess para una implementación multisitio  
Los equipos cliente de Windows de DirectAccess deben ser miembros de los grupos de seguridad que definen su asociación de DirectAccess. Antes de habilitar el multisitio, estos grupos de seguridad pueden contener clientes de Windows 8 y clientes de Windows 7 (si se seleccionó el modo "de nivel inferior" adecuado). Una vez habilitada la opción multisitio, los grupos de seguridad de cliente existentes, en modo de servidor único, solo se convierten en grupos de seguridad para Windows 8. Una vez habilitada la opción multisitio, los equipos cliente de Windows 7 de DirectAccess deben moverse a los grupos de seguridad de cliente de Windows 7 dedicados correspondientes (que están asociados a puntos de entrada específicos) o no podrán conectarse a través de DirectAccess. Los clientes de Windows 7 se deben quitar primero de los grupos de seguridad existentes que son ahora grupos de seguridad de Windows 8. PRECAUCIÓN: los equipos cliente de Windows 7 que son miembros de los grupos de seguridad de cliente de Windows 7 y Windows 8 perderán la conectividad remota y los clientes de Windows 7 sin SP1 instalado también perderán la conectividad corporativa. Por lo tanto, todos los equipos cliente de Windows 7 deben quitarse de los grupos de seguridad de Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Quitar clientes de Windows 7 de grupos de seguridad de Windows 8  
  
1.  En el controlador de dominio principal, haga clic en **Inicio**y, a continuación, en **Active Directory usuarios y equipos**.  
  
2.  Para quitar equipos del grupo de seguridad, haga doble clic en el grupo de seguridad y, en el **< Group_Name** cuadro de diálogo propiedades de >, haga clic en la pestaña **miembros** .  
  
3.  Seleccione el equipo cliente de Windows 7 y haga clic en **quitar**.  
  
4.  Repita este procedimiento para quitar los equipos cliente de Windows 7 de los grupos de seguridad de Windows 8.  
  
> [!IMPORTANT]  
> Al habilitar una configuración de acceso remoto multisitio, todos los equipos cliente (Windows 7 y Windows 8) perderán la conectividad remota hasta que puedan conectarse a la red corporativa directamente o a través de VPN para actualizar sus directivas de grupo. Esto es así cuando se habilita la funcionalidad de multisitio por primera vez, y también cuando se deshabilita multisitio.  
  
## <a name="36-enable-the-multisite-deployment"></a><a name="BKMK_Enable"></a>3,6. Habilitar la implementación multisitio  
Para configurar una implementación multisitio, habilite la característica multisitio en el servidor de acceso remoto existente. Antes de habilitar el multisitio en su implementación, asegúrese de que tiene la siguiente información:  
  
1.  Configuración de equilibrador de carga global y direcciones IP si desea equilibrar la carga de las conexiones de cliente de DirectAccess en todos los puntos de entrada de la implementación.  
  
2.  Los grupos de seguridad que contienen los equipos cliente de Windows 7 para el primer punto de entrada de la implementación, si desea habilitar el acceso remoto para equipos cliente de Windows 7.  
  
3.  Directiva de grupo los nombres de objeto, si es necesario usar objetos de directiva de grupo no predeterminados, que se aplican en equipos cliente de Windows 7 para el primer punto de entrada de la implementación, si necesita compatibilidad con equipos cliente de Windows 7.  
  
### <a name="to-enable-a-multisite-configuration"></a><a name="EnabledMultisite"></a>Para habilitar una configuración multisitio  
  
1.  En el servidor de acceso remoto existente: en la pantalla **Inicio** , escriba **RAMgmtUI. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**y, en el panel **tareas** , haga clic en **Habilitar multisitio**.  
  
3.  En el Asistente para **Habilitar la implementación multisitio** , en la página **antes de comenzar** , haga clic en **siguiente**.  
  
4.  En la página **nombre de implementación** , en nombre de **implementación multisitio**, escriba un nombre para la implementación. En **primer nombre de punto de entrada**, escriba un nombre para identificar el primer punto de entrada que es el servidor de acceso remoto actual y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **selección de punto de entrada** , realice una de las acciones siguientes:  
  
    -   Haga clic en **asignar puntos de entrada automáticamente y permitir que los clientes seleccionen manualmente** para redirigir automáticamente los equipos cliente al punto de entrada más adecuado, a la vez que permite a los equipos cliente seleccionar un punto de entrada de forma manual. La selección de punto de entrada manual solo está disponible para equipos con Windows 8. Haga clic en **Siguiente**.  
  
    -   Haga clic en **asignar puntos de entrada automáticamente** para redirigir automáticamente los equipos cliente al punto de entrada más adecuado y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **equilibrio de carga global** , realice una de las acciones siguientes:  
  
    -   Haga clic en no **, no usar el equilibrio de carga global** si no desea usar un equilibrio de carga global y, a continuación, haga clic en **siguiente**.  
  
        > [!NOTE]  
        > Al seleccionar esta opción, los equipos cliente se conectan automáticamente a su punto de entrada más cercano.  
  
    -   Haga clic en **sí, usar el equilibrio de carga global** si desea equilibrar la carga del tráfico globalmente entre todos los puntos de entrada. En **Escriba el FQDN de equilibrio de carga global que usarán todos los puntos de entrada**, escriba el FQDN de equilibrio de carga global y, en **tipo, la dirección IP de equilibrio de carga global para este punto de entrada** que contiene el primer servidor de acceso remoto, escriba la dirección IP de equilibrio de carga global para este punto de entrada y, a continuación, haga clic en **siguiente**.  
  
7.  En la página **compatibilidad con clientes** , realice una de las acciones siguientes:  
  
    -   Para limitar el acceso a equipos cliente que ejecutan sistemas operativos Windows 8 o versiones posteriores, haga clic en **limitar el acceso a los equipos cliente que ejecutan Windows 8 o un sistema operativo posterior**y, a continuación, haga clic en **siguiente**.  
  
    -   Para permitir que los equipos cliente que ejecutan Windows 7 tengan acceso a este punto de entrada, haga clic en **permitir que los equipos cliente que ejecutan Windows 7 accedan a este punto de entrada**y haga clic en **Agregar**. En el cuadro de diálogo **seleccionar grupos** , seleccione los grupos de seguridad que contienen los equipos cliente de Windows 7, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
8.  En la página **configuración de GPO de cliente** , acepte el GPO predeterminado para equipos cliente de Windows 7 para este punto de entrada, escriba el nombre del GPO que desea que el acceso remoto cree automáticamente, o haga clic en **examinar** para buscar el GPO para equipos cliente de Windows 7 y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > -   La página **configuración de GPO de cliente** solo aparece cuando se configura el punto de entrada para permitir que los equipos cliente de Windows 7 tengan acceso al punto de entrada.  
    > -   También puede hacer clic en **validar GPO** para asegurarse de que tiene los permisos adecuados para el GPO o GPO seleccionados para este punto de entrada. Si el GPO no existe y se creará automáticamente, se requerirán permisos para crear y vincular. En el caso de que los GPO se hayan creado manualmente, se necesitan los permisos editar, modificar seguridad y eliminar.  
  
9. En la página **Resumen** , haga clic en **confirmar**.  
  
10. En el cuadro de diálogo **Habilitar la implementación multisitio** , haga clic en **cerrar** y, a continuación, en el Asistente para habilitar la implementación multisitio, haga clic en **cerrar**.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
Para habilitar una implementación multisitio denominada ' contoso ' en el primer punto de entrada denominado ' Edge1-US '. La implementación permite a los clientes seleccionar manualmente el punto de entrada y no usa un equilibrador de carga global.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Para permitir el acceso de los equipos cliente de Windows 7 a través del primer punto de entrada a través del grupo de seguridad DA_Clients_US y el uso del DA_W7_Clients_GPO_US de GPO.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="37-add-entry-points-to-the-multisite-deployment"></a><a name="BKMK_EntryPoint"></a>3,7. Agregar puntos de entrada a la implementación multisitio  
Después de habilitar multisite en la implementación, puede Agregar puntos de entrada adicionales mediante el Asistente para agregar un punto de entrada. Antes de agregar puntos de entrada, asegúrese de que tiene la siguiente información:  
  
-   Direcciones IP del equilibrador de carga global para cada nuevo punto de entrada si utiliza el equilibrio de carga global.  
  
-   Los grupos de seguridad que contienen los equipos cliente de Windows 7 para cada punto de entrada que se agregarán si desea habilitar el acceso remoto para equipos cliente de Windows 7.  
  
-   Directiva de grupo los nombres de objeto, si es necesario usar objetos de directiva de grupo no predeterminados, que se aplican en equipos cliente de Windows 7 para cada punto de entrada que se va a agregar, si necesita compatibilidad con equipos cliente de Windows 7.  
  
-   En el caso de que IPv6 esté implementado en la red de la organización, tendrá que preparar el prefijo IP-HTTPS para el nuevo punto de entrada.  
  
### <a name="to-add-entry-points-to-your-multisite-deployment"></a><a name="AddEP"></a>Para agregar puntos de entrada a la implementación multisitio  
  
1.  En el servidor de acceso remoto existente: en la pantalla **Inicio** , escriba **RAMgmtUI. exe**y, a continuación, presione Entrar. Si aparece el cuadro de **Control de cuentas de usuario**, confirma que la acción que se muestra es la que deseas realizar y haz clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**y, en el panel **tareas** , haga clic en **Agregar un punto de entrada**.  
  
3.  En el Asistente para agregar un punto de entrada, en la página **detalles del punto de entrada** , en servidor de **acceso remoto**, escriba el nombre de dominio completo (FQDN) del servidor que se va a agregar. En **nombre de punto de entrada**, escriba el nombre del punto de entrada y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **configuración de equilibrio de carga global** , escriba la dirección IP de equilibrio de carga global de este punto de entrada y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > La página **configuración de equilibrio de carga global** solo aparece cuando la configuración multisitio usa un equilibrador de carga global.  
  
5.  En la página **topología de red** , haga clic en la topología que se corresponda con la topología de red del servidor de acceso remoto que va a agregar y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **nombre de red o dirección IP** , en **Escriba el nombre público o la dirección IP que usan los clientes para conectarse al servidor de acceso remoto**, escriba el nombre público o la dirección IP que usan los clientes para conectarse al servidor de acceso remoto. El nombre público corresponde al nombre del firmante del certificado IP-HTTPS. En el caso de que se haya implementado la topología de red perimetral, la dirección IP es la del adaptador externo del servidor de acceso remoto. Haga clic en **Siguiente**.  
  
7.  En la página **adaptadores de red** , realice una de las acciones siguientes:  
  
    -   Si va a implementar una topología con dos adaptadores de red, en **adaptador externo**, seleccione el adaptador que está conectado a la red externa. En **adaptador interno**, seleccione el adaptador que está conectado a la red interna.  
  
    -   Si va a implementar una topología con un adaptador de red, en **adaptador de red**, seleccione el adaptador que está conectado a la red interna.  
  
8.  En la página **adaptadores de red** , en **Seleccione el certificado usado para autenticar las conexiones IP-https**, haga clic en **examinar** para buscar y seleccionar el certificado IP-https. Haga clic en **Siguiente**.  
  
9. Si IPv6 está configurado en la red corporativa, en la página **configuración de prefijo** , en **prefijo IPv6 asignado a equipos cliente**, escriba un prefijo IP-https para asignar direcciones IPv6 a los equipos cliente de DirectAccess y haga clic en **siguiente**.  
  
10. En la página **compatibilidad con clientes** , realice una de las acciones siguientes:  
  
    -   Para limitar el acceso a equipos cliente que ejecutan sistemas operativos Windows 8 o versiones posteriores, haga clic en **limitar el acceso a los equipos cliente que ejecutan Windows 8 o un sistema operativo posterior**y, a continuación, haga clic en **siguiente**.  
  
    -   Para permitir que los equipos cliente que ejecutan Windows 7 tengan acceso a este punto de entrada, haga clic en **permitir que los equipos cliente que ejecutan Windows 7 accedan a este punto de entrada**y haga clic en **Agregar**. En el cuadro de diálogo **seleccionar grupos** , seleccione los grupos de seguridad que contienen los equipos cliente de Windows 7 que se conectarán a este punto de entrada, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
11. En la página **configuración de GPO de cliente** , acepte el GPO predeterminado para equipos cliente de Windows 7 para este punto de entrada, escriba el nombre del GPO que desea que el acceso remoto cree automáticamente, o haga clic en **examinar** para buscar el GPO para equipos cliente de Windows 7 y haga clic en **siguiente**.  
  
    > [!NOTE]  
    > -   La página **configuración de GPO de cliente** solo aparece cuando se configura el punto de entrada para permitir que los equipos cliente de Windows 7 tengan acceso al punto de entrada.  
    > -   También puede hacer clic en **validar GPO** para asegurarse de que tiene los permisos adecuados para el GPO o GPO seleccionados para este punto de entrada. Si el GPO no existe y se creará automáticamente, se requerirán permisos para crear y vincular. En el caso de que los GPO se hayan creado manualmente, se necesitan los permisos editar, modificar seguridad y eliminar.  
  
12. En la página **configuración de GPO de servidor** , acepte el GPO predeterminado para este servidor de acceso remoto, escriba el nombre del GPO que desea que el acceso remoto cree automáticamente, o bien haga clic en **examinar** para buscar el GPO de este servidor y, a continuación, haga clic en **siguiente**.  
  
13. En la página **servidor de ubicación de red** , haga clic en **examinar** para seleccionar el certificado para el sitio web del servidor de ubicación de red que se ejecuta en el servidor de acceso remoto y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > La página **servidor de ubicación de red** solo aparece cuando el sitio web del servidor de ubicación de red se ejecuta en el servidor de acceso remoto.  
  
14. En la página **Resumen** , revise la configuración del punto de entrada y, a continuación, haga clic en **confirmar**.  
  
15. En el cuadro de diálogo **Agregar punto de entrada** , haga clic en **cerrar** y, a continuación, en el Asistente para agregar un punto de entrada, haga clic en **cerrar**.  
  
    > [!NOTE]  
    > Si el punto de entrada que se agregó está en un bosque diferente al de los puntos de entrada o equipos cliente existentes, es necesario hacer clic en **actualizar servidores de administración** en el panel de **tareas** para detectar los controladores de dominio y Configuration Manager en el nuevo bosque.  
  
16. Repita este procedimiento desde el paso 2 para cada punto de entrada que desee agregar a la implementación multisitio.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
Para agregar el equipo edge2 del dominio Corp2 como un segundo punto de entrada denominado Edge2-Europe. La configuración del punto de entrada es: un prefijo IPv6 de cliente ' 2001: db8:2: 2000::/64 ', una dirección de conexión a (el certificado IP-HTTPS en el equipo edge2) ' edge2.contoso.com ', un GPO de servidor denominado "configuración del servidor de DirectAccess-Edge2-Europe", y las interfaces internas y externas denominadas Internet y Corpnet2, respectivamente:  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Para permitir el acceso de los equipos cliente de Windows 7 a través del segundo punto de entrada a través del grupo de seguridad DA_Clients_Europe y el uso del DA_W7_Clients_GPO_Europe de GPO.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vea también  
  
-   [Paso 2: configurar la infraestructura multisitio](Step-2-Configure-the-Multisite-Infrastructure.md)
