---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurar el acceso condicional basadas en el dispositivo local
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurar local acceso condicional con los dispositivos registrados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2  

El siguiente documento le guiará a través de la instalación y configuración de acceso condicional de local con los dispositivos registrados.

![Acceso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Requisitos previos de infraestructura
Los requisitos por siguientes son necesarios para que pueda comenzar con el acceso condicional de locales. 

|Requisito|Descripción
|-----|-----
|Una suscripción a Azure AD con Azure AD Premium | Para habilitar la escritura de dispositivo vuelve de acceso condicional de locales - [sirve una prueba gratuita](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Suscripción de Intune|sólo es necesario para la integración de MDM para escenarios de cumplimiento del dispositivo -[sirve una prueba gratuita](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|Noviembre de 2015 QFE o posterior.  Obtener la versión más reciente [aquí](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Compilación 10586 o posterior de AD FS  
|Esquema de Active Directory de Windows Server 2016|Se requiere nivel de esquema 85 o superior.
|Controlador de dominio de Windows Server 2016|Esto solo es necesario para las implementaciones de confianza de la clave de Hello para empresas.  Puede encontrar información adicional en [aquí](https://aka.ms/whfbdocs).  
|Cliente de Windows 10|Compilación 10586 o más reciente, unido al dominio anterior es necesaria para la unión de dominios de Windows 10 y Microsoft Passport para escenarios de trabajo solo  
|Cuenta de usuario de Azure AD con licencia de Azure AD Premium asignada|Para registrar el dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Actualizar el esquema de Active Directory
Para usar el acceso condicional de local con los dispositivos registrados, primero debes actualizar el esquema de AD.  Deben cumplirse las siguientes condiciones:
    - El esquema debe ser 85 o posterior
    - Esto solo es necesario para el bosque de AD FS está unido a

> [!NOTE]
> Si instalaste Azure AD Connect antes de actualizar a la versión del esquema (nivel de 85 o superior) en Windows Server 2016, tendrás que volver a ejecutar la instalación de Azure AD Connect y actualizar las instalaciones de esquema de Active Directory para garantizar la regla de sincronización para msDS KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Comprobar el nivel de esquema
Para comprobar el nivel de esquema, haz lo siguiente:

1.  Puedes usar ADSIEdit o LDP y conectar con el contexto de nomenclatura de esquema.  
2.  Utilizando ADSIEdit, haz clic en "CN = esquema, CN = Configuración, DC =<domain>, DC =<com> y selecciona Propiedades.  Dominio Relpace y las partes de com con la información de bosque.
3.  En el Editor de atributo encontrar el atributo objectVersion y te dirá, la versión.  

![Editor ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

También puedes usar el siguiente cmdlet de PowerShell (reemplazar el objeto con el esquema de información de contexto de nomenclatura):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obtener más información sobre la actualización, consulta [actualizar controladores de dominio a Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar el registro de dispositivo de Azure AD  
Para configurar este escenario, debes configurar la funcionalidad de registro del dispositivo en Azure AD.  

Para ello, sigue los pasos del tema [configurar unir a Azure AD en la organización](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>AD FS del programa de instalación  
1. Crear la un [nueva granja de servidores de AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  O [migrar](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) una granja de AD FS 2016 de AD FS 2012 R2  
4. Implementar [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) usando la ruta de acceso personalizados para conectar AD FS con Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurar dispositivo escritura atrás y la autenticación de dispositivo  
> [!NOTE]
> Si ejecutas Azure AD Connect mediante la configuración rápida, los objetos de AD correctos se haya creado para TI.  Sin embargo, en la mayoría de los escenarios de AD FS, Azure AD Connect se ejecutó con la configuración personalizada para configurar AD FS, por lo tanto, los pasos siguientes son necesarios.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Crear objetos de Active Directory para la autenticación de dispositivo de AD FS  
Si el conjunto de AD FS ya no está configurado para la autenticación de dispositivo (puedes ver esto en la consola de administración de AD FS en servicio -> registro del dispositivo), usa los siguientes pasos para crear los objetos correctos de AD DS y configuración.  

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Nota: La debajo de comandos, necesitan herramientas de administración de Active Directory, por lo tanto, si el servidor de federación no es también un controlador de dominio, instalar las herramientas con el siguiente paso 1.  De lo contrario, puedes omitir el paso 1.  

1.  Ejecutar el **agregar Roles y características** característica Asistente y selecciona **Remote Server Administration Tools** -> **herramientas de administración de roles** -> **AD DS y AD LDS herramientas** -> elegir tanto la **módulo de Active Directory para Windows PowerShell** y **herramientas de AD DS**.

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  En el servidor de AD FS principal, asegúrate de que se registra en el como usuario de AD DS con privilegios de administrador de empresa (EA) y abre un símbolo del sistema de powershell con privilegios elevados.  A continuación, ejecutar los siguientes comandos de PowerShell:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  En la ventana emergente de aciertos sí.

>Nota: Si tu servicio de AD FS está configurado para usar una cuenta GMSA, escribe el nombre de cuenta en el formato "Dominio\AccountName$"

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

La Psh: anterior crea los siguientes objetos:  


- Contenedor de RegisteredDevices en la partición de dominio de AD  
- Contenedor de servicio de registro del dispositivo y el objeto en Configuración >--> Servicios--> configuración de registro del dispositivo  
- Contenedor de dispositivo DKM de servicio de registro y el objeto en Configuración >--> Servicios--> configuración de registro del dispositivo  

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Una vez hecho esto, verás un mensaje de finalización correcta.

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Crear servicio conexión punto en AD  
Si tienes previsto usar unión a un dominio de Windows 10 (con el registro automático a Azure AD) como se describe aquí, ejecute los siguientes comandos para crear un punto de conexión de servicio en AD DS  
1.  Abre Windows PowerShell y ejecutar las siguientes acciones:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Nota: si es necesario, copia el archivo AdSyncPrep.psm1 desde el servidor de Azure AD Connect.  Este archivo se encuentra en el programa de programa\Microsoft Azure Active Directory Connect\AdPrep

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Proporcionar sus credenciales de administrador global de Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Ejecuta el siguiente comando de PowerShell 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Donde el [nombre de la cuenta de AD connector] es el nombre de la cuenta configurada en Azure AD Connect al agregar tu local en el directorio de AD DS.
  
Los comandos anteriores permiten que los clientes de Windows 10 buscar el correcto dominio de Azure AD al que unirse al crear el objeto serviceConnectionpoint en AD DS.  

### <a name="prepare-ad-for-device-write-back"></a>Preparar AD para la escritura de dispositivo nuevo   
Para asegurarse de contenedores y objetos de AD DS en el estado correcto para escritura posterior dispositivos de Azure AD, haz lo siguiente.

1.  Abre Windows PowerShell y ejecutar las siguientes acciones:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Donde el [nombre de la cuenta de AD connector] es el nombre de la cuenta configurada en Azure AD Connect al agregar tu local en el directorio de AD DS en formato dominio\nombreDeCuenta  

El comando anterior crea los siguientes objetos de dispositivo de escritura a AD DS, si no existen ya y permite el acceso al nombre de cuenta de AD connector especificado  

- Contenedor de RegisteredDevices en la partición de dominio de AD  
- Contenedor de servicio de registro del dispositivo y el objeto en Configuración >--> Servicios--> configuración de registro del dispositivo  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Habilitar la escritura de dispositivo en Azure AD Connect  
Si no ha hecho antes, habilitar la escritura de dispositivo en Azure AD Connect ejecutando el Asistente para la segunda vez y seleccionando **"Personalizar la configuración de sincronización"**, activando la casilla para la escritura de dispositivo nuevo y seleccionar el bosque en el que ha ejecutar los cmdlets anteriores  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurar la autenticación de dispositivo en AD FS  
Con una ventana de comandos de PowerShell con privilegios elevados, configurar la directiva de AD FS ejecutando el siguiente comando  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Comprobar la configuración  
Como referencia, a continuación se incluye una lista completa de los dispositivos AD DS, los contenedores y los permisos necesarios para que la autenticación y reescritura de dispositivo para trabajar
 


- objeto de tipo ms-DS-DeviceContainer en CN = RegisteredDevices, DC =&lt;dominio&gt;        
    - acceso de lectura a la cuenta de servicio de AD FS   
    - acceso de lectura y escritura para la cuenta de AD conector de sincronización de Azure AD Connect</br></br>

- Contenedor CN = Configuración de registro del dispositivo, CN = Services, CN = Configuración, DC =&lt;dominio&gt;  
- Servicio de registro de contenedor dispositivo DKM en el contenedor anterior

![Registro del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- objeto de tipo serviceConnectionpoint en CN =&lt;guid&gt;, CN = registro del dispositivo

- Configuración, CN = Services, CN = Configuración, DC =&lt;dominio&gt;  
 - acceso de lectura/escritura al nombre de cuenta de AD connector en el nuevo objeto especificado</br></br> 


- objeto de tipo msDS-DeviceRegistrationServiceContainer en CN = Servicios de registro del dispositivo, CN = Configuración de registro del dispositivo, CN = Services, CN = Configuración, DC = & ltdomain >  


- objeto de tipo msDS-DeviceRegistrationService en el contenedor anterior  

### <a name="see-it-work"></a>Ver funcionan  
Para evaluar las directivas y las reclamaciones nuevo, registrar un dispositivo.  Por ejemplo, puedes unión de Azure AD un equipo de Windows 10 con la aplicación configuración bajo sistema -> acerca de, o puede configurar unirse a un dominio de Windows 10 con el registro de dispositivo automática siguiendo los pasos adicionales [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obtener información sobre cómo combinar Windows 10, dispositivos móviles, consulta el documento [aquí](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para la evaluación más sencilla, inicia sesión AD FS con una aplicación de prueba que muestra una lista de una reclamación. Podrás ver notificaciones nuevas incluidos isManaged, isCompliant y trusttype.  Si habilitar Microsoft Passport para el trabajo, también verá la Imp reclamar.  
 

## <a name="configure-additional-scenarios"></a>Configurar escenarios adicionales  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Equipos de registro para Windows 10 Unidos a un dominio automáticos  
Para habilitar el registro automático del dispositivo para el dominio de Windows 10 equipos unidos a un, sigue los pasos 1 y 2 [aquí](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Esto ayudará a lograr lo siguiente:  

1. Garantizar el punto de conexión de servicio en AD DS existe y tiene los permisos adecuados (creamos este objeto anterior, pero no ha perjudicial doblemente).  
2. Asegúrate de que AD FS está correctamente configurado  
3. Asegúrate de que el sistema de AD FS tenga los extremos correctos habilitados y reclamar reglas configuradas   
4. Configurar la configuración de directiva de grupo necesaria para el registro de dispositivo automática de los equipos unidos a un dominio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Para obtener información sobre la activación de Windows 10 con Microsoft Passport for Work, consulta [habilitar Microsoft Passport for Work en la organización.](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscripción automática en MDM   
Para habilitar la inscripción de MDM automática de dispositivos registrados para que puedan usar la reclamación isCompliant en la directiva de control de acceso, sigue los pasos [aquí.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solución de problemas  
1.  Si se produce un error `Initialize-ADDeviceRegistration` que se queja sobre un objeto ya existente en un estado incorrecto, como "se ha encontrado el objeto de servicio de drs sin todos los atributos necesarios", que pueda haber ejecutado Azure AD Connect powershell comandos anteriormente y tiene una configuración parcial en AD DS.  Intenta eliminar manualmente los objetos en **CN = Configuración de registro del dispositivo, CN = Services, CN = Configuración, DC =&lt;dominio&gt; ** y volver a intentarlo.  
2.  Para Windows 10 dominio unido a los clientes  
    1. Para comprobar que la autenticación de dispositivo está en funcionamiento, inicia sesión el dominio unido a un cliente, como una cuenta de usuario de prueba. Para desencadenar el aprovisionamiento rápidamente, bloquear y desbloquear el escritorio al menos una vez.   
    2. Instrucciones para buscar stk clave credenciales vinculación en objeto de AD DS (¿sincronizar aún tiene que ejecutar dos veces?)  
3.  Si recibes un error al intentar registrar un equipo de Windows que ya se inscribe el dispositivo, pero no puede o ya has anula el dispositivo, puede que un fragmento de la configuración de inscripción del dispositivo en el registro.  Para investigar y quitar esto, usa los siguientes pasos:  
    1. En el equipo de Windows, abre Regedit y navega a **HKLM\Software\Microsoft\Enrollments**   
    2. En esta clave, habrá muchos subclaves en forma de GUID.  Navegar a la subclave que tiene los valores de ~ 17 en él y tiene "EnrollmentType" de "6" [MDM Unido] o "13" (se unido a Azure AD)  
    3. Modificar **EnrollmentType** a **0** 
    4. Inténtalo de nuevo la inscripción del dispositivo o el registro  

### <a name="related-articles"></a>Artículos relacionados  
* [Proteger el acceso a Office 365 y otras aplicaciones conectadas a Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Directivas de dispositivo de acceso condicional para servicios de Office 365](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurar el acceso condicional local mediante el registro de dispositivo de Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conectar dispositivos Unidos a un dominio a Azure AD para experiencias de Windows 10](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
