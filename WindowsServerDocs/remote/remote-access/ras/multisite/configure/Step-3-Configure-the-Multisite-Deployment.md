---
title: Paso 3 configurar la implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5c74a9277af3853d709a8ecd58c1e53e1bccc719
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812516"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Paso 3 configurar la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de configurar la infraestructura de multisitio, siga estos pasos para configurar la implementación multisitio de acceso remoto.  
  
|Tarea|Descripción|  
|----|--------|  
|3.1. Configurar servidores de acceso remoto|Configurar servidores de acceso remoto adicionales mediante la configuración de direcciones IP, une al dominio e instalar el rol de acceso remoto.|  
|3.2. Conceder acceso de administrador|Conceder privilegios en los servidores de acceso remoto adicionales para el Administrador de DirectAccess.|  
|3.3. Configuración de IP-HTTPS para una implementación multisitio|Configure el certificado IP-HTTPS que usa en una implementación multisitio.|  
|3.4. Configurar el servidor de ubicación de red para una implementación multisitio|Configurar el certificado de servidor de ubicación de red utilizado en una implementación multisitio.|  
|3.5. Configurar a los clientes de DirectAccess para una implementación multisitio|Quitar los equipos cliente de Windows 7 de grupos de seguridad de Windows 8.|  
|3.6. Habilitar la implementación multisitio|Habilitar la implementación multisitio en el primer servidor de acceso remoto.|  
|3.7. Agregar puntos de entrada a la implementación multisitio|Agregar puntos de entrada adicionales a la implementación multisitio.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigServer"></a>3.1. Configurar servidores de acceso remoto  

  
### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
1.  Asegúrese de que cada servidor de acceso remoto está configurado con la topología de implementación adecuada (perímetro, detrás de un NAT, la única interfaz de red) y las rutas correspondientes.  
  
2.  Configurar las direcciones IP en cada servidor de acceso remoto según la topología del sitio y el esquema de direccionamiento de IP de su organización.  
  
3.  Únase a cada servidor de acceso remoto a un dominio de Active Directory.  
  
4.  En la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
5.   Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
6.  En el **seleccionar Roles de servidor** cuadro de diálogo, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente**.  
  
7.  Haga clic en **siguiente** tres veces.  
  
8.  En el **seleccionar servicios de rol** cuadro de diálogo, seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **agregar características**.  
  
9.  Seleccione **enrutamiento**, seleccione **Proxy de aplicación Web**, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.  
  
10. Haga clic en **Siguiente**y después en **Instalar**.  
  
11.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  

  
Los pasos 1-3 deben realizarse manualmente y no se realizan mediante este cmdlet de Windows PowerShell.  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Admin"></a>3.2. Conceder acceso de administrador  
  
#### <a name="to-grant-administrator-permissions"></a>Para conceder permisos de administrador  
  
1.  En el servidor de acceso remoto en el punto de entrada adicionales: En el **iniciar** , escriba **administración de equipos**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo, haga clic en **usuarios y grupos locales**.  
  
3.  Haga doble clic en **grupos**y, a continuación, haga doble clic en **administradores**.  
  
4.  En el **propiedades de administradores** cuadro de diálogo, haga clic en **agregar**y en el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, haga clic en  **Ubicaciones**.  
  
5.  En el **ubicaciones** cuadro de diálogo el **ubicación** de árbol, haga clic en la ubicación que contiene la cuenta de usuario del Administrador de DirectAccess y, a continuación, haga clic en **Aceptar**.  
  
6.  En el **escriba los nombres de objeto para seleccionar**, escriba el nombre de usuario del Administrador de DirectAccess y, a continuación, haga clic en **Aceptar** dos veces.  
  
7.  En el **propiedades de administradores** cuadro de diálogo, haga clic en **Aceptar**.  
  
8.  Cierre la ventana de administración de equipos.  
  
9. Repita este procedimiento en todos los servidores de acceso remoto que se va a formar parte de la implementación multisitio.  
  
## <a name="BKMK_IPHTTPS"></a>3.3. Configuración de IP-HTTPS para una implementación multisitio  
En cada servidor de acceso remoto que se agregarán a la implementación multisitio, un certificado SSL es necesario para comprobar la conexión de HTTPS al servidor web IP-HTTPS. Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Para obtener un certificado IP-HTTPS  
  
1.  En cada servidor de acceso remoto: En el **iniciar** , escriba **mmc**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic sucesivamente en **Certificados**, **Agregar**, **Cuenta de equipo** y **Siguiente**, seleccione **Equipo local**, haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En el **solicitar certificados** página, haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribir este certificado**.  
  
    Si la plantilla de certificado de servidor Web no aparece, asegúrese de que la cuenta de equipo del servidor de acceso remoto con permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, consulte [configurar permisos en la plantilla de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  En el **asunto** pestaña de la **las propiedades del certificado** cuadro de diálogo **nombre de sujeto**, para **tipo**, seleccione **comunes nombre**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) del nombre de Internet del servidor de acceso remoto (por ejemplo, Europe.contoso.com) y, a continuación, haga clic en **agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado IP-HTTPS** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero que sea más fácil para seleccionar el certificado para IP-HTTPS al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores de acceso remoto en la implementación.  
  
## <a name="BKMK_NLS"></a>3.4. Configurar el servidor de ubicación de red para una implementación multisitio  
Si ha seleccionado configurar el sitio Web servidor de ubicación de red en el servidor de acceso remoto al configurar el primer servidor, cada servidor de acceso remoto nuevas que agregue debe configurarse con un certificado de servidor Web que tiene el mismo nombre de asunto que se seleccionó para t servidor de ubicación de red para el primer servidor. Cada servidor necesita un certificado para autenticar la conexión al servidor de ubicación de red y los equipos cliente ubicados en la red interna deben ser capaces de resolver el nombre del sitio Web en DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar un certificado para la ubicación de red  
  
1.  En el servidor de acceso remoto: En el **iniciar** , escriba **mmc**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic sucesivamente en **Certificados**, **Agregar**, **Cuenta de equipo** y **Siguiente**, seleccione **Equipo local**, haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
    > [!NOTE]  
    > También puede importar el mismo certificado que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En el **solicitar certificados** página, haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribir este certificado**.  
  
    Si la plantilla de certificado de servidor Web no aparece, asegúrese de que la cuenta de equipo del servidor de acceso remoto con permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, consulte [configurar permisos en la plantilla de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  En el **asunto** pestaña de la **las propiedades del certificado** cuadro de diálogo **nombre de sujeto**, para **tipo**, seleccione **comunes nombre**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) que se configuró para el certificado de servidor de ubicación de red del primer servidor de acceso remoto (por ejemplo, nls.corp.contoso.com) y, a continuación, haga clic en **agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado de ubicación de red** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero que sea más fácil para seleccionar el certificado para la ubicación de red al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores de acceso remoto en la implementación.  
  
### <a name="NLS"></a>Para crear el servidor de ubicación de red en los registros de DNS  
  
1.  En el servidor DNS: En el **iniciar** , escriba **dnsmgmt.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo de la **el Administrador de DNS** de consola, abra la zona de búsqueda directa para la red interna. Haga clic en la zona correspondiente y haga clic en **Host nuevo (A o AAAA)**.  
  
3.  En el **nuevo Host** cuadro de diálogo el **nombre (si está en blanco se usa el nombre del dominio primario)** , escriba el nombre que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto. En el **dirección IP** cuadro, escriba la dirección IPv4 intranet del servidor de acceso remoto y, a continuación, haga clic en **agregar Host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
4.  En el **nuevo Host** cuadro de diálogo el **nombre (si está en blanco se usa el nombre del dominio primario)** , escriba el nombre que se usó para el servidor de ubicación de red para el primer servidor de acceso remoto. En el **dirección IP** cuadro, escriba la dirección de IPv6 expuestas a la intranet del servidor de acceso remoto y, a continuación, haga clic en **agregar Host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
5.  Repita los pasos 3 y 4 para cada servidor de acceso remoto en la implementación.  
  
6.  Haga clic en **Listo**.  
  
7.  Repita este procedimiento antes de agregar servidores como puntos de entrada adicionales en la implementación.  
  
## <a name="BKMK_Client"></a>3.5. Configurar a los clientes de DirectAccess para una implementación multisitio  
Los equipos cliente de Windows de DirectAccess deben ser miembros de los grupos de seguridad que definen su asociación de DirectAccess. Antes de habilita la funcionalidad de multisitio, estos grupos de seguridad pueden contener los clientes de Windows 8 y los clientes de Windows 7 (si se ha seleccionado el modo adecuado "nivel inferior"). Una vez habilitada la funcionalidad de multisitio, grupos de seguridad de cliente existente, en modo de servidor único, se convierten en los grupos de seguridad para Windows 8 solo. Después de la funcionalidad de multisitio está habilitada, los equipos cliente de DirectAccess de Windows 7 se deben mover a dedicado Windows 7 cliente grupos de seguridad correspondientes (que están asociados con los puntos de entrada específicos) o no será capaz de conectarse a través de DirectAccess. Los clientes de Windows 7 en primer lugar deben quitarse de los grupos de seguridad existentes, que ahora son grupos de seguridad de Windows 8. Precaución:  Los equipos de cliente de Windows 7 que son miembros de grupos de seguridad de cliente de Windows 7 y Windows 8 pierden la conectividad remota y los clientes de Windows 7 sin SP1 instalado perderá conectividad corporativa también. Por lo tanto, todos los equipos cliente de Windows 7 deben quitarse de los grupos de seguridad de Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Quitar grupos de seguridad de Windows 8 en los clientes de Windows 7  
  
1.  En el controlador de dominio principal, haga clic en **iniciar**y, a continuación, haga clic en **equipos y usuarios de Active Directory**.  
  
2.  Para quitar equipos del grupo de seguridad, haga doble clic en el grupo de seguridad y en el **< nombre_grupo > propiedades** cuadro de diálogo, haga clic en el **miembros** ficha.  
  
3.  Seleccione el equipo cliente Windows 7 y haga clic en **quitar**.  
  
4.  Repita este procedimiento para quitar los equipos cliente de Windows 7 de los grupos de seguridad de Windows 8.  
  
> [!IMPORTANT]  
> Cuando se habilita una configuración multisitio de acceso remoto todos los equipos cliente (Windows 7 y Windows 8) perderá conectividad remota hasta que se pueda conectar a la red corporativa directamente o mediante VPN para actualizar sus directivas de grupo. Esto es cierto cuando se habilita la funcionalidad de multisitio por primera vez y también al deshabilitar multisitio.  
  
## <a name="BKMK_Enable"></a>3.6. Habilitar la implementación multisitio  
Para configurar una implementación multisitio, habilitar la funcionalidad de multisitio en el servidor de acceso remoto existente. Antes de habilitar el multisitio en su implementación, asegúrese de que tiene la siguiente información:  
  
1.  Configuración del equilibrador de carga global y las direcciones IP si desea cargar equilibrar las conexiones de cliente de DirectAccess en todos los puntos de entrada en la implementación.  
  
2.  La seguridad de grupo (s) que contiene equipos cliente de Windows 7 para el primer punto de entrada en la implementación, si desea permitir que los equipos cliente de acceso remoto para Windows 7.  
  
3.  Los nombres de objeto de directiva de grupo si es necesario utilizar objetos de directiva de grupo no predeterminado, que se aplican en los equipos cliente de Windows 7 para el primer punto de entrada en la implementación, si necesita soporte técnico para los equipos cliente de Windows 7.  
  
### <a name="EnabledMultisite"></a>Para habilitar una configuración multisitio  
  
1.  En el servidor de acceso remoto existente: En el **iniciar** , escriba **RAMgmtUI.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**y, a continuación, en el **tareas** panel, haga clic en **habilitar multisitio**.  
  
3.  En el **Habilitar implementación multisitio** asistente la **antes de comenzar** página, haga clic en **siguiente**.  
  
4.  En el **nombre de la implementación** página **nombre de la implementación multisitio**, escriba un nombre para la implementación. En **primer punto de entrada nombre**, escriba un nombre para identificar el primer punto de entrada que es el servidor de acceso remoto actual y, a continuación, haga clic en **siguiente**.  
  
5.  En el **selección del punto de entrada** , realice una de las siguientes acciones:  
  
    -   Haga clic en **asignar automáticamente los puntos de entrada y permitir a los clientes seleccionar manualmente** para redirigir automáticamente los equipos cliente al punto de entrada más adecuado, mientras que también permite los equipos seleccionar manualmente un punto de entrada de cliente. Selección del punto de entrada manual sólo está disponible para los equipos Windows 8. Haz clic en **Siguiente**.  
  
    -   Haga clic en **asignar puntos de entrada automáticamente** automáticamente enrutar los equipos cliente al punto de entrada más adecuado y, a continuación, haga clic en **siguiente**.  
  
6.  En el **equilibrio de carga Global** , realice una de las siguientes acciones:  
  
    -   Haga clic en **No, no use equilibrio de carga global** si no desea usar un equilibrio de carga global y, a continuación, haga clic en **siguiente**.  
  
        > [!NOTE]  
        > Al seleccionar a este cliente opción equipos se conectan automáticamente a su punto de entrada más cercano.  
  
    -   Haga clic en **Sí, usar el equilibrio de carga global** si desea cargar equilibrar el tráfico global entre todos los puntos de entrada. En **escriba el FQDN que usarán todos los puntos de entrada de equilibrio de carga global**, escriba el FQDN, de equilibrio de carga global y en **escriba la dirección IP para este punto de entrada de equilibrio de carga global** que contiene la primera Servidor de acceso remoto, escriba la dirección IP para este punto de entrada de equilibrio de carga global y, a continuación, haga clic en **siguiente**.  
  
7.  En el **compatibilidad con clientes** , realice una de las siguientes acciones:  
  
    -   Para limitar el acceso a los equipos cliente que ejecutan Windows 8 o sistemas operativos posteriores, haga clic en **limitar el acceso a equipos cliente que ejecutan Windows 8 o un sistema operativo posterior**y, a continuación, haga clic en **siguiente**.  
  
    -   Para permitir que los equipos que ejecutan Windows 7 para tener acceso a este punto de entrada de cliente, haga clic en **permitir cliente en equipos que ejecutan Windows 7 para tener acceso a este punto de entrada**y haga clic en **agregar**. En el **seleccionar grupos** cuadro de diálogo, seleccione los grupos de seguridad que contenga los equipos cliente de Windows 7, haga clic en **Aceptar**y, a continuación, haga clic en **siguiente**.  
  
8.  En el **configuración de GPO de cliente** página, acepte los equipos de cliente de GPO para Windows 7 de forma predeterminada para este punto de entrada, escriba el nombre del GPO que desea tener acceso remoto a crear automáticamente, o haga clic en **examinar** a Busque los equipos cliente de GPO para Windows 7 y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > -   El **configuración de GPO de cliente** página aparece solo al configurar el punto de entrada a los equipos cliente de Windows 7 pueden tener acceso al punto de entrada.  
    > -   Opcionalmente, puede hacer clic en **validar GPO** para asegurarse de que tiene los permisos adecuados para el GPO seleccionado o los GPO para este punto de entrada. Si el GPO no existe y se crearán automáticamente, a continuación, crea y vincula los permisos son necesarios. En caso de que los GPO se han creado manualmente, a continuación, edite, modificar la seguridad y se requieren permisos de eliminación.  
  
9. En el **resumen** página, haga clic en **confirmar**.  
  
10. En el **Habilitar implementación multisitio** cuadro de diálogo, haga clic en **cerrar** y, a continuación, haga clic en el Asistente para habilitar implementación multisitio, **cerrar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para habilitar una implementación multisitio denominada "Contoso" en el primer punto de entrada denominado "Edge1-US". La implementación permite a los clientes seleccionar manualmente el punto de entrada y no utiliza un equilibrador de carga global.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Para permitir el acceso de los equipos cliente de Windows 7 mediante el primer punto de entrada a través del grupo de seguridad DA_Clients_US y el uso de la GPO DA_W7_Clients_GPO_US.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="BKMK_EntryPoint"></a>3.7. Agregar puntos de entrada a la implementación multisitio  
Después de habilitar el multisitio en su implementación, puede agregar puntos de entrada adicionales mediante el punto de entrada Asistente para agregar un. Antes de agregar puntos de entrada, asegúrese de que tiene la siguiente información:  
  
-   Direcciones IP del equilibrador de carga global para cada nueva entrada punto si usa equilibrio de carga global.  
  
-   Los grupos de seguridad que contiene los equipos cliente de Windows 7 para cada punto de entrada que se agregará si desea permitir que los equipos cliente de acceso remoto para Windows 7.  
  
-   Los nombres de objeto de directiva de grupo si se requiere para usar objetos de directiva de grupo no predeterminado, que se aplican en los equipos cliente de Windows 7 para cada punto de entrada que se agregará, si necesita soporte técnico para los equipos cliente de Windows 7.  
  
-   En el caso donde se implementa IPv6 en la red de su organización deberá preparar el prefijo IP-HTTPS para el nuevo punto de entrada.  
  
### <a name="AddEP"></a>Para agregar puntos de entrada a la implementación multisitio  
  
1.  En el servidor de acceso remoto existente: En el **iniciar** , escriba **RAMgmtUI.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **configuración**y, a continuación, en el **tareas** panel, haga clic en **agregar un punto de entrada**.  
  
3.  En el agregar un punto de entrada asistente, en el **detalles del punto de entrada** página **servidor de acceso remoto**, escriba el nombre de dominio completo (FQDN) del servidor para agregar. En **nombre del punto de entrada**, escriba el nombre del punto de entrada y, a continuación, haga clic en **siguiente**.  
  
4.  En el **parámetros de equilibrio de carga Global** página, escriba la dirección IP de este punto de entrada de equilibrio de carga global y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > El **parámetros de equilibrio de carga Global** página aparece solo cuando la configuración multisitio usa un equilibrador de carga global.  
  
5.  En el **topología de red** página, haga clic en la topología que se corresponde con la topología de red del servidor de acceso remoto que se va a agregar y, a continuación, haga clic en **siguiente**.  
  
6.  En el **nombre de red o la dirección IP** página **tipo en el nombre público o dirección IP usada por los clientes para conectarse al servidor de acceso remoto**, escriba el nombre público o dirección IP usada por los clientes para conectarse a la Servidor de acceso remoto. El nombre público se corresponde con el nombre de sujeto del certificado IP-HTTPS. En el caso donde se implementó la topología de red perimetral, la dirección IP es que el adaptador externo del servidor de acceso remoto. Haz clic en **Siguiente**.  
  
7.  En el **adaptadores de red** , realice una de los siguientes:  
  
    -   Si va a implementar una topología con dos adaptadores de red en **adaptador externo**, seleccione el adaptador que está conectado a la red externa. En **adaptador interno**, seleccione el adaptador que está conectado a la red interna.  
  
    -   Si va a implementar una topología con un adaptador de red, en **adaptador de red**, seleccione el adaptador que está conectado a la red interna.  
  
8.  En el **adaptadores de red** página **seleccione el certificado utilizado para autenticar conexiones IP-HTTPS**, haga clic en **examinar** para buscar y seleccionar el certificado IP-HTTPS. Haz clic en **Siguiente**.  
  
9. Si IPv6 está configurado en la red corporativa, en el **configuración de prefijo** página **prefijo IPv6 asignado a los equipos cliente**, escriba un prefijo IP-HTTPS para asignar direcciones IPv6 para el cliente de DirectAccess los equipos y haga clic en **siguiente**.  
  
10. En el **compatibilidad con clientes** , realice una de las siguientes acciones:  
  
    -   Para limitar el acceso a los equipos cliente que ejecutan Windows 8 o sistemas operativos posteriores, haga clic en **limitar el acceso a equipos cliente que ejecutan Windows 8 o un sistema operativo posterior**y, a continuación, haga clic en **siguiente**.  
  
    -   Para permitir que los equipos que ejecutan Windows 7 para tener acceso a este punto de entrada de cliente, haga clic en **permitir cliente en equipos que ejecutan Windows 7 para tener acceso a este punto de entrada**y haga clic en **agregar**. En el **seleccionar grupos** cuadro de diálogo, seleccione los grupos de seguridad que contienen los equipos cliente de Windows 7 que se conecten a este punto de entrada, haga clic en **Aceptar**y haga clic en **siguiente**.  
  
11. En el **configuración de GPO de cliente** página, acepte los equipos de cliente de GPO para Windows 7 de forma predeterminada para este punto de entrada, escriba el nombre del GPO que desee tener acceso remoto para crear automáticamente, o haga clic en **examinar** a Busque los equipos cliente de GPO para Windows 7 y haga clic en **siguiente**.  
  
    > [!NOTE]  
    > -   El **configuración de GPO de cliente** página aparece solo al configurar el punto de entrada a los equipos cliente de Windows 7 pueden tener acceso al punto de entrada.  
    > -   Opcionalmente, puede hacer clic en **validar GPO** para asegurarse de que tiene los permisos adecuados para el GPO seleccionado o los GPO para este punto de entrada. Si el GPO no existe y se crearán automáticamente, a continuación, crea y vincula los permisos son necesarios. En caso de que los GPO se han creado manualmente, a continuación, edite, modificar la seguridad y se requieren permisos de eliminación.  
  
12. En el **configuración de GPO de servidor** página, acepte el GPO de forma predeterminada para este servidor de acceso remoto, escriba el nombre del GPO que desee tener acceso remoto para crear automáticamente, o haga clic en **examinar** para buscar el GPO para Este servidor y, a continuación, haga clic en **siguiente**.  
  
13. En el **Network Location Server** página, haga clic en **examinar** para seleccionar el certificado para el sitio Web servidor de ubicación de red que se ejecutan en el servidor de acceso remoto y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > El **Network Location Server** página aparece sólo cuando se ejecuta el sitio Web servidor de ubicación de red en el servidor de acceso remoto.  
  
14. En el **resumen** , revise la configuración de punto de entrada y, a continuación, haga clic en **confirmar**.  
  
15. En el **agregar punto de entrada** cuadro de diálogo, haga clic en **cerrar** y, a continuación, el agregar un Asistente para puntos de entrada, haga clic en **cerrar**.  
  
    > [!NOTE]  
    > Si el punto de entrada que se ha agregado está en un bosque diferente que los puntos de entrada existentes o los equipos cliente, es necesario hacer clic en **actualizar servidores de administración** en el **tareas** panel para detectar el controladores de dominio y System Center Configuration Manager en el nuevo bosque.  
  
16. Repita este procedimiento en el paso 2 para cada punto de entrada que desea agregar a la implementación multisitio.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para agregar el edge2 de equipo del dominio corp2 como un segundo punto de entrada denominado Edge2 Europa. Es la configuración del punto de entrada: un prefijo IPv6 del cliente ' 2001:db8:2:2000:: / 64', conectar a la dirección (el certificado IP-HTTPS en el equipo edge2) 'edge2.contoso.com', un GPO de servidor denominado "DirectAccess Server configuración - Edge2-Europa" y los internos y interfaces externas denominan Internet y Corpnet2 respectivamente:  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Para permitir el acceso de los equipos cliente de Windows 7 mediante el segundo punto de entrada a través del grupo de seguridad DA_Clients_Europe y el uso de la GPO DA_W7_Clients_GPO_Europe.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 2: Configurar la infraestructura de multisitio](Step-2-Configure-the-Multisite-Infrastructure.md)
