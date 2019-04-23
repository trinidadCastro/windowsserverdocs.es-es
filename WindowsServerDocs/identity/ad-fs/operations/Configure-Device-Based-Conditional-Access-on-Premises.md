---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configuración del acceso condicional basado en dispositivos a nivel local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fa96fbeed1445b1add2e5de3aad45ad369a6cafa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847226"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurar un entorno local acceso condicional con dispositivos registrados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2  

El siguiente documento le guiará a través de la instalación y configuración de acceso condicional local con los dispositivos registrados.

![Acceso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Requisitos previos de la infraestructura
Los siguientes requisitos previos son necesarios antes de comenzar con el acceso condicional en el entorno local. 

|Requisitos|Descripción
|-----|-----
|Una suscripción de Azure AD con Azure AD Premium | Para habilitar la escritura de dispositivo por sobre el acceso condicional del entorno local - [una evaluación gratuita es suficiente](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Suscripción a Intune|solo es necesario para la integración de MDM para escenarios de cumplimiento de dispositivos -[una evaluación gratuita es suficiente](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE de noviembre de 2015 o posterior.  Obtener la última versión [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Compilación 10586 o posterior para AD FS  
|Esquema de Active Directory de Windows Server 2016|Se requiere el nivel de esquema 85 o superior.
|Controlador de dominio de Windows Server 2016|Esto solo es necesario para las implementaciones de confianza de la clave de Hello para empresas.  Puede encontrar información adicional en [aquí](https://aka.ms/whfbdocs).  
|Cliente de Windows 10|Compilación 10586 o posterior, unido al dominio anterior es necesario para la unión de dominios de Windows 10 y Microsoft Passport para escenarios de trabajo solo  
|Cuenta de usuario de Azure AD con licencia de Azure AD Premium asignada|Para registrar el dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Actualizar el esquema de Active Directory
Para usar el acceso condicional local con los dispositivos registrados, primero debe actualizar el esquema de AD.  Deben cumplirse las condiciones siguientes:
    - El esquema debe ser la versión 85 o posterior
    - Esto solo es necesario para el bosque de AD FS esté unido a

> [!NOTE]
> Si ha instalado Azure AD Connect antes de actualizar a la versión de esquema (nivel 85 o superior) en Windows Server 2016, deberá volver a ejecutar la instalación de Azure AD Connect y actualice la red local esquema de Active Directory para garantizar la regla de sincronización para msDS-KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Compruebe el nivel de esquema
Para comprobar el nivel de esquema, realice lo siguiente:

1.  Puede usar ADSIEdit o LDP y conectar con el contexto de nomenclatura de esquema.  
2.  Utilizando ADSIEdit, haga doble clic en "CN = Schema, CN = Configuration, DC =<domain>, DC =<com> y seleccione Propiedades.  Dominio Relpace y las partes de com con la información de bosque.
3.  En el Editor de atributos encontrar el atributo objectVersion y le indicará, su versión.  

![Editor ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

También puede usar el siguiente cmdlet de PowerShell (reemplazar el objeto con el esquema de información de contexto de nomenclatura):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obtener más información acerca de la actualización, consulte [actualizar controladores de dominio a Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar el registro de dispositivos de Azure AD  
Para configurar este escenario, debe configurar la funcionalidad de registro del dispositivo en Azure AD.  

Para ello, siga los pasos descritos en [configuración de Azure AD Join en su organización](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Programa de instalación de AD FS  
1. Crear el una [nueva granja de servidores de AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  O [migrar](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) una granja de AD FS 2016 de AD FS 2012 R2  
4. Implementar [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) mediante la ruta de acceso personalizada para conectarse a AD FS a Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configure la autenticación de dispositivo y escritura de dispositivo nuevo  
> [!NOTE]
> Si ejecuta Azure AD Connect mediante la configuración rápida, se crearon los objetos de AD correctos para usted.  Sin embargo, en la mayoría de los escenarios de AD FS, Azure AD Connect se ejecutó con configuración personalizada para configurar AD FS, por lo tanto, los pasos siguientes son necesarios.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Crear objetos de AD para la autenticación de dispositivos AD FS  
Si la granja de AD FS ya no está configurada para la autenticación de dispositivos (puedes ver esto en la consola de Administración de AD FS en Servicio -> Registro de dispositivos), usa los siguientes pasos para crear la configuración y objetos de AD DS correctos.  

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Nota: Los comandos siguientes requieren las herramientas de administración de Active Directory, de modo que si el servidor de federación no es también un controlador de dominio, primero debes instalar las herramientas siguiendo las indicaciones del paso 1.  De lo contrario, puedes omitir el paso 1.  

1.  Ejecuta el asistente para **Agregar roles y características** y selecciona la característica **Herramientas de administración remota del servidor** -> **Herramientas de administración de roles** -> **Herramientas de AD DS y AD LDS** -> Elige **Módulo de Active Directory para Windows PowerShell** y **Herramientas de AD DS**.

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  En el servidor principal AD FS, asegúrese de que está conectado como usuario de AD DS con privilegios de administrador de Enterprise (EA) y abra un símbolo del sistema de powershell con privilegios elevados.  A continuación, ejecute los siguientes comandos de PowerShell:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  En la ventana emergente, presione sí.

>Nota: Si el servicio de AD FS está configurado para usar una cuenta de GMSA, escribe el nombre de la cuenta en el formato "domain\accountname$"

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

El PSH anterior crea los siguientes objetos:  


- Contenedor RegisteredDevices en la partición de dominio de AD  
- Contenedor de servicio de registro de dispositivos y objeto en Configuración --> Servicios --> Configuración de registro de dispositivos  
- Contenedor de DKM de servicio de registro de dispositivos y objeto en Configuración --> Servicios --> Configuración de registro de dispositivos  

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Una vez realizado este paso, aparecerá un mensaje de finalización correcta.

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Crear punto de conexión de servicio (SCP) de AD  
Si tienes previsto usar la unión de dominios de Windows 10 (con registro automático en Azure AD) tal y como se describe aquí, ejecuta los siguientes comandos para crear un punto de conexión de servicios en AD DS  
1.  Abre Windows PowerShell y ejecuta lo siguiente:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Nota: si es necesario, copie el archivo AdSyncPrep.psm1 desde el servidor de Azure AD Connect.  Este archivo está ubicado en Archivos de programa\Microsoft Azure Active Directory Connect\AdPrep

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Indicar las credenciales de administrador global de Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Ejecutar el siguiente comando de PowerShell 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Donde el [nombre de la cuenta de AD Connector] es el nombre de la cuenta que has configurado en Azure AD Connect al agregar el directorio de AD DS local.
  
Los comandos anteriores permiten a los clientes de Windows 10 buscar el dominio correcto de Azure AD al que quieren unirse mediante la creación del objeto serviceConnectionpoint en AD DS.  

### <a name="prepare-ad-for-device-write-back"></a>Preparar AD para la reescritura de dispositivos   
Para asegurarte de que los contenedores y objetos de AD DS están en el estado correcto para la reescritura de dispositivos de Azure AD, realiza lo siguiente:

1.  Abre Windows PowerShell y ejecuta lo siguiente:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Donde el [nombre de la cuenta de AD Connector] es el nombre de la cuenta que has configurado en Azure AD Connect al agregar el directorio de AD DS local en el formato \accountname del dominio  

El comando anterior crea los siguientes objetos para la reescritura de dispositivos en AD DS, si no existen, y permite el acceso al nombre de la cuenta especificado de AD Connector  

- Contenedor RegisteredDevices en la partición de dominio de AD  
- Contenedor de servicio de registro de dispositivos y objeto en Configuración --> Servicios --> Configuración de registro de dispositivos  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Habilitar la reescritura de dispositivos en Azure AD Connect  
Si no lo has hecho anteriormente, habilitar la reescritura de dispositivos en Azure AD Connect ejecutando el asistente una segunda vez y seleccionando **"Personalizar las opciones de sincronización"** y luego marcando la casilla de reescritura de dispositivos y seleccionando el bosque en el que has ejecutado los cmdlets anteriores  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurar la autenticación de dispositivos en AD FS  
Con una ventana de comandos de PowerShell con privilegios elevados, configura la directiva de AD FS ejecutando el siguiente comando  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Comprobar la configuración  
Como referencia, a continuación se incluye una lista completa de los dispositivos AD DS, los contenedores y los permisos necesarios para que la autenticación y la reescritura de dispositivos funcionen
 


- objeto de tipo ms-DS-DeviceContainer en CN=RegisteredDevices,DC=&lt;domain&gt;        
    - acceso de lectura a la cuenta de servicio de AD FS   
    - acceso de lectura/escritura a la cuenta del conector AD de sincronización de Azure AD Connect</br></br>

- Contenedor CN=Configuración de registro de dispositivos,CN=Servicios,CN=Configuración,DC=&lt;dominio&gt;  
- DKM de servicio de registro de dispositivos de contenedor en el contenedor anterior

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- objeto de tipo serviceConnectionpoint en CN=&lt;guid&gt;, CN=Registro de dispositivos

- Configuración,CN=Servicios,CN=Configuración,DC=&lt;dominio&gt;  
 - acceso de lectura/escritura al nombre de cuenta de AD Connector en el nuevo objeto</br></br> 


- objeto de tipo msDS-DeviceRegistrationServiceContainer de CN = Servicios de registro del dispositivo, CN = Device Registration Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- objeto de tipo msDS-DeviceRegistrationService en el contenedor anterior  

### <a name="see-it-work"></a>Ver cómo funciona  
Para evaluar las nuevas notificaciones y las directivas, primero hay que registrar un dispositivo.  Por ejemplo, puede un equipo de Windows 10 mediante la aplicación de configuración en el sistema -> acerca de Azure AD Join, o puede configurar la unión a un dominio de Windows 10 con el registro automático de dispositivos siguiendo los pasos adicionales [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obtener información sobre la unión a Windows 10, dispositivos móviles, consulte el documento [aquí](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para su evaluación más sencilla, inicie sesión en AD FS mediante una aplicación de prueba que se muestra una lista de notificaciones. Podrá ver nuevas notificaciones incluyen isManaged isCompliant y trusttype.  Si habilita Microsoft Passport para el trabajo, también verá el prt de notificación.  
 

## <a name="configure-additional-scenarios"></a>Configurar escenarios adicionales  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Equipos automática de registro de Windows 10 Unidos a un dominio  
Para habilitar el registro automático de dispositivos para el dominio de Windows 10 Unidos a los equipos, siga los pasos 1 y 2 [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Esto le ayudará a lograr lo siguiente:  

1. Asegúrese de su punto de conexión de servicio en AD DS existe y tiene los permisos adecuados (se creó este objeto anterior, pero no es perjudicial para una comprobación doble).  
2. Asegúrese de que AD FS está configurado correctamente  
3. Asegúrese de que el sistema de AD FS tiene habilitados los puntos de conexión correctas y las reglas configuradas de notificación   
4. Configure la directiva de grupo necesarias para el registro automático de dispositivos de equipos unidos al dominio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport para el trabajo   
Para obtener información sobre la habilitación de Windows 10 con Microsoft Passport for Work, consulte [habilitar Microsoft Passport for Work en su organización.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscripción automática de MDM   
Para habilitar la inscripción automática de MDM de dispositivos registrados para que pueda usar la notificación isCompliant en la directiva de control de acceso, siga los pasos [aquí.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solución de problemas  
1.  Si se produce un error `Initialize-ADDeviceRegistration` que se queja sobre un objeto ya existente en un estado incorrecto, como "se ha encontrado el objeto de servicio drs sin todos los atributos necesarios", puede haber ejecutado comandos de powershell de Azure AD Connect previamente y tiene una configuración parcial en AD DS.  Intente eliminar manualmente los objetos situados debajo **CN = Device Registration Configuration, CN = Services, CN = Configuration, DC =&lt;dominio&gt;**  y volver a intentar.  
2.  Windows 10 Unidos a dominios clientes  
    1. Para comprobar que la autenticación de dispositivo está funcionando, inicie sesión en el cliente unido a dominio como una cuenta de usuario de prueba. Para activar rápidamente el aprovisionamiento, bloquear y desbloquear el escritorio al menos una vez.   
    2. Instrucciones para comprobar si la credencial de clave stk vinculan en el objeto de AD DS (¿sincronización todavía tiene que ejecutar dos veces?)  
3.  Si recibe un error al intentar registrar un equipo de Windows que ya se ha inscrito el dispositivo, pero no puede o ya ha anular la inscripción del dispositivo, puede tener un fragmento de configuración de inscripción del dispositivo en el registro.  Para investigar y quitar esto, siga estos pasos:  
    1. En el equipo de Windows, abra Regedit y vaya a **HKLM\Software\Microsoft\Enrollments**   
    2. Bajo esta clave, habrá muchos subclaves en el formato GUID.  Vaya a la subclave que contiene los valores de ~ 17 y tiene "EnrollmentType" de "6" [MDM Unido] o "13" (Unidos a Azure AD)  
    3. Modificar **EnrollmentType** a **0** 
    4. Inténtelo de nuevo la inscripción de dispositivos o el registro  

### <a name="related-articles"></a>Artículos relacionados  
* [Protección del acceso a Office 365 y otras aplicaciones conectadas a Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Directivas de dispositivos de acceso condicional para servicios de Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configuración del acceso condicional local mediante el registro de dispositivos de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conectar dispositivos Unidos a Azure AD para experiencias de Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
