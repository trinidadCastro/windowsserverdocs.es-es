---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configuración del acceso condicional basado en dispositivos a nivel local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7646144b591fd7327f881cb54489201140e9287
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358151"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configuración del acceso condicional local mediante dispositivos registrados


El siguiente documento le guiará a través de la instalación y configuración del acceso condicional local con dispositivos registrados.

![Acceso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Requisitos previos de la infraestructura
Se requieren los siguientes requisitos por cada requisito para poder comenzar con el acceso condicional local. 

|Requisitos|Descripción
|-----|-----
|Una suscripción de Azure AD con Azure AD Premium | Para habilitar la reescritura de dispositivos para el acceso condicional local: [una evaluación gratuita es correcta](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Suscripción a Intune|solo es necesario para la integración de MDM para escenarios de cumplimiento de dispositivos:[una evaluación gratuita es correcta](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0) .
|Azure AD Connect|QFE de 2015 de noviembre o posterior.  Obtenga la versión más reciente [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Compilación 10586 o posterior para AD FS  
|Esquema de Active Directory de Windows Server 2016|Se requiere el nivel de esquema 85 o superior.
|Controlador de dominio de Windows Server 2016|Esto solo es necesario para las implementaciones de clave de confianza Hello for Business.  Encontrará información adicional en [este artículo](https://aka.ms/whfbdocs).  
|Cliente de Windows 10|La compilación 10586 o posterior, unida al dominio anterior, solo es necesaria para los escenarios de unión a un dominio de Windows 10 y Microsoft Passport for Work  
|Azure AD cuenta de usuario con la licencia de Azure AD Premium asignada|Para registrar el dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Actualización del esquema de Active Directory
Para poder usar el acceso condicional local con dispositivos registrados, primero debe actualizar el esquema de AD.  Deben cumplirse las siguientes condiciones:
    - El esquema debe ser de la versión 85 o posterior.
    - Esto solo es necesario para el bosque al que está unido AD FS

> [!NOTE]
> Si ha instalado Azure AD Connect antes de actualizar a la versión de esquema (nivel 85 o superior) en Windows Server 2016, tendrá que volver a ejecutar la instalación de Azure AD Connect y actualizar el esquema de AD local para asegurarse de que la regla de sincronización para msDS-KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Comprobar el nivel de esquema
Para comprobar el nivel de esquema, haga lo siguiente:

1.  Puede usar ADSIEdit o LDP y conectarse al contexto de nomenclatura del esquema.  
2.  Con ADSIEdit, haga clic con el botón derecho en "CN = Schema, CN = Configuration, DC = <domain>, DC = <com> y seleccione Propiedades.  Relpace dominio y las partes com con la información del bosque.
3.  En el editor de atributos, busque el atributo objectVersion y le indicará la versión.  

![Editor ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

También puede usar el siguiente cmdlet de PowerShell (Reemplace el objeto con la información de contexto de nomenclatura de esquema):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obtener información adicional sobre la actualización, vea [actualizar controladores de dominio a Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar Registro de dispositivos de Azure AD  
Para configurar este escenario, debe configurar la funcionalidad de registro de dispositivos en Azure AD.  

Para ello, siga los pasos descritos en [configuración de Azure ad JOIN en su organización](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/) .  

## <a name="setup-ad-fs"></a>AD FS de instalación  
1. Cree una [nueva granja AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  O [migrar](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) una granja de servidores a AD FS 2016 desde AD FS 2012 R2  
4. Implemente [Azure ad Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) mediante la ruta de acceso personalizada para conectarse AD FS a Azure ad.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configuración de la reescritura de dispositivos y la autenticación de dispositivos  
> [!NOTE]
> Si ejecutó Azure AD Connect con la configuración rápida, se han creado los objetos de AD correctos.  Sin embargo, en la mayoría de los escenarios de AD FS, Azure AD Connect se ejecutó con una configuración personalizada para configurar AD FS, por lo que es necesario realizar los pasos siguientes.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Crear objetos de AD para la autenticación de dispositivos AD FS  
Si la granja de AD FS ya no está configurada para la autenticación de dispositivos (puedes ver esto en la consola de Administración de AD FS en Servicio -> Registro de dispositivos), usa los siguientes pasos para crear la configuración y objetos de AD DS correctos.  

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Nota: Los comandos siguientes requieren las herramientas de administración de Active Directory, de modo que si el servidor de federación no es también un controlador de dominio, primero debes instalar las herramientas siguiendo las indicaciones del paso 1.  De lo contrario, puedes omitir el paso 1.  

1.  Ejecuta el asistente para **Agregar roles y características** y selecciona la característica **Herramientas de administración remota del servidor** -> **Herramientas de administración de roles** -> **Herramientas de AD DS y AD LDS** -> Elige **Módulo de Active Directory para Windows PowerShell** y **Herramientas de AD DS**.

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. En el servidor principal de AD FS, asegúrese de que ha iniciado sesión como AD DS usuario con privilegios de administrador de empresa (EA) y abra un símbolo del sistema de PowerShell con privilegios elevados.  A continuación, ejecute los siguientes comandos de PowerShell:  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. En la ventana emergente, presione sí.

>Nota: Si el servicio de AD FS está configurado para usar una cuenta de GMSA, escribe el nombre de la cuenta en el formato "domain\accountname$"

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

El PSH anterior crea los siguientes objetos:  


- Contenedor RegisteredDevices en la partición de dominio de AD  
- Contenedor de servicio de registro de dispositivos y objeto en Configuración --> Servicios --> Configuración de registro de dispositivos  
- Contenedor de DKM de servicio de registro de dispositivos y objeto en Configuración --> Servicios --> Configuración de registro de dispositivos  

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. Una vez realizado este paso, aparecerá un mensaje de finalización correcta.

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Crear punto de conexión de servicio (SCP) en AD  
Si tienes previsto usar la unión de dominios de Windows 10 (con registro automático en Azure AD) tal y como se describe aquí, ejecuta los siguientes comandos para crear un punto de conexión de servicios en AD DS  
1.  Abre Windows PowerShell y ejecuta lo siguiente:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Nota: si es necesario, copie el archivo AdSyncPrep. psm1 del servidor de Azure AD Connect.  Este archivo está ubicado en Archivos de programa\Microsoft Azure Active Directory Connect\AdPrep

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Indicar las credenciales de administrador global de Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro de dispositivos](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. Ejecutar el siguiente comando de PowerShell 

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


- objeto de tipo msDS-DeviceRegistrationServiceContainer en CN = Device registration Services, CN = Device registration Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- objeto de tipo msDS-DeviceRegistrationService en el contenedor anterior  

### <a name="see-it-work"></a>Vea cómo funciona  
Para evaluar las nuevas notificaciones y directivas, registre primero un dispositivo.  Por ejemplo, puede Azure AD unir un equipo Windows 10 mediante la aplicación de configuración en System-> Acerca de, o bien puede configurar la Unión a un dominio de Windows 10 con el registro automático de dispositivos siguiendo los pasos adicionales que se describen [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obtener información sobre cómo unir dispositivos Windows 10 Mobile, consulte el documento [aquí](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para una evaluación más sencilla, inicie sesión en AD FS con una aplicación de prueba que muestre una lista de notificaciones. Podrá ver las nuevas notificaciones, como IsManaged (, isCompliant y trusttype.  Si habilita Microsoft Passport for work, también verá la afirmación de PRT.  
 

## <a name="configure-additional-scenarios"></a>Configuración de escenarios adicionales  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Registro automático para equipos Unidos a un dominio de Windows 10  
Para habilitar el registro automático de dispositivos para equipos Unidos a un dominio de Windows 10, siga los pasos 1 [y 2.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
Esto le ayudará a lograr lo siguiente:  

1. Asegúrese de que el punto de conexión de servicio de AD DS existe y de que tiene los permisos adecuados (hemos creado este objeto anteriormente, pero no perjudica a la comprobación doble).  
2. Asegúrese de que AD FS esté configurado correctamente.  
3. Asegúrese de que el sistema AD FS tiene habilitados los puntos de conexión correctos y las reglas de notificaciones configuradas   
4. Configurar las opciones de directiva de grupo necesarias para el registro automático de dispositivos de equipos Unidos a un dominio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport para el trabajo   
Para obtener información sobre cómo habilitar Windows 10 con Microsoft Passport for Work, consulte [habilitar Microsoft Passport for work en su organización.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscripción automática de MDM   
Siga los pasos que se indican aquí para habilitar la inscripción automática de MDM de dispositivos registrados de forma que pueda usar la declaración de isCompliant en su Directiva de control de acceso [.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solución de problemas  
1.  Si recibe un error en `Initialize-ADDeviceRegistration` que indica que un objeto ya existe en un estado incorrecto, como "el objeto de servicio DRS se ha encontrado sin todos los atributos necesarios", es posible que haya ejecutado Azure AD Connect comandos de PowerShell previamente y que tenga un configuración parcial en AD DS.  Intente eliminar manualmente los objetos en **CN = Device registration Configuration, CN = Services, CN = Configuration, DC = &lt;domain @ no__t-2** y vuelva a intentarlo.  
2.  Para clientes Unidos a un dominio de Windows 10  
    1. Para comprobar que la autenticación del dispositivo funciona, inicie sesión en el cliente unido a un dominio como una cuenta de usuario de prueba. Para desencadenar el aprovisionamiento rápidamente, bloquee y desbloquee el escritorio al menos una vez.   
    2. Instrucciones para comprobar el vínculo de la credencial de clave de STK en AD DS objeto (¿la sincronización todavía tiene que ejecutarse dos veces?)  
3.  Si se produce un error al intentar registrar un equipo Windows que el dispositivo ya estaba inscrito, pero no puede o ya ha anulado la inscripción del dispositivo, es posible que tenga un fragmento de configuración de inscripción de dispositivos en el registro.  Para investigar y quitar esto, siga estos pasos:  
    1. En el equipo Windows, abra regedit y vaya a **HKLM\Software\Microsoft\Enrollments**   
    2. Bajo esta clave, habrá muchas subclaves en el formato de GUID.  Navegue hasta la subclave, que tiene aproximadamente 17 valores en ella y tiene el valor "EnrollmentType" de "6" [unjoined MDM] o "13" (Azure AD Unidos)  
    3. Modificar **EnrollmentType** a **0** 
    4. Vuelva a intentar la inscripción o el registro del dispositivo  

### <a name="related-articles"></a>Artículos relacionados  
* [Proteger el acceso a Office 365 y otras aplicaciones conectadas a Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Directivas de dispositivo de acceso condicional para Office 365 Services](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configuración del acceso condicional local mediante Registro de dispositivos de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conexión de dispositivos Unidos a un dominio a Azure AD para experiencias de Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
