---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: "Los procedimientos recomendados para la seguridad de AD FS y Proxy de aplicación Web"
description: "Este documento proporciona los procedimientos recomendados para el diseño seguro y la implementación de servicios de federación de Active Directory (AD FS) y Proxy de aplicación Web."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Procedimientos recomendados para proteger los servicios de federación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento proporciona los procedimientos recomendados para el diseño seguro y la implementación de servicios de federación de Active Directory (AD FS) y Proxy de aplicación Web.  Contiene información sobre los comportamientos predeterminados de estos componentes y recomendaciones para las configuraciones de seguridad adicional para una organización con casos de uso específicos y los requisitos de seguridad.

Este documento se aplica a AD FS y WAP en Windows Server 2012 R2 y Windows Server 2016 (vista previa).  Estas recomendaciones pueden usarse si se ha implementado la infraestructura de una red local en o en un entorno hospedado en la nube como Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topología de implementación estándar
Para la implementación en entornos locales, te recomendamos que consta de uno o varios servidores de AD FS en la red corporativa interna, con uno o varios servidores Proxy de aplicación Web (WAP) en una red de extranet o DMZ una topología de implementación estándar.  En cada nivel, AD FS y WAP, un equilibrado de carga de hardware o software se coloca delante de la granja de servidores y controla el enrutamiento de tráfico.  Los servidores de seguridad se colocan según sea necesarios en la parte frontal de la dirección IP externa de la carga de equilibrado delante de cada conjunto (FS y proxy).

![Topología de AD FS estándar](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Puertos necesarios
El diagrama siguiente muestra los puertos del firewall que deben estar habilitados entre y entre los componentes de la implementación de AD FS y WAP.  Si la implementación no incluye Azure AD / Office 365, los requisitos de sincronización se puede ignorar.

>Ten en cuenta que el puerto 49443 solo es necesario si se usa la autenticación de certificado de usuario, que es opcional para Azure AD y Office 365.

![Topología de AD FS estándar](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect y los servidores de federación/WAP
Esta tabla describen los puertos y protocolos requeridos para la comunicación entre el servidor de Azure AD Connect y los servidores de federación/WAP.  

Protocolo |Puertos |Descripción
--------- | --------- |---------
HTTP|80 (TCP O UDP)|Se usa para descargar las CRL (certificado listas de revocación) para comprobar los certificados SSL.
HTTPS|443(TCP/UDP)|Usar para sincronizar con Azure AD.
WinRM|5985| Escucha WinRM

### <a name="wap-and-federation-servers"></a>Los servidores de federación y WAP
Esta tabla describen los puertos y protocolos requeridos para la comunicación entre los servidores de federación y WAP.

Protocolo |Puertos |Descripción
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Se usa para la autenticación.

### <a name="wap-and-users"></a>Los usuarios y WAP
Esta tabla describen los puertos y protocolos requeridos para la comunicación entre los usuarios y los servidores WAP.

Protocolo |Puertos |Descripción
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Se usa para la autenticación de dispositivo.
TCP|49443 (TCP)|Se usa para la autenticación de certificado.

Para obtener más información sobre puertos necesarios y protocolos requeridos para las implementaciones híbridas, consulta el documento [aquí](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/).

Para obtener información detallada acerca de los puertos y protocolos requeridos para Azure AD e implementación de Office 365, consulta el documento [aquí](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Extremos habilitados

Cuando se instalan AD FS y WAP, un conjunto predeterminado de los extremos de AD FS están habilitados en el servicio de federación y en el servidor proxy.  Estos valores predeterminados se eligieron en función de los más de los escenarios necesarios y que se usan y no es necesario cambiarlos.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Opcional] Conjunto mínimo de proxy de extremos habilitada para Azure AD y Office 365
Las organizaciones que implementan AD FS y WAP solo para Azure AD y Office 365 escenarios pueden limitar aún más el número de puntos de conexión de AD FS habilitado en el servidor proxy para lograr una superficie de ataque más mínima.
A continuación se incluye en la lista de los extremos con los que debe estar habilitado en el servidor proxy en estos escenarios:

|Extremo|Propósito
|-----|-----
|/ adfs/ls|Flujo de autenticación basada en el explorador y las versiones actuales de Microsoft Office usan este extremo de Azure AD y autenticación de Office 365
|/ADFS/Services/Trust/2005/usernamemixed|Usar Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes posterior usan el extremo \adfs\ls pasivo.
|/ADFS/Services/Trust/13/usernamemixed|Usar Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes posterior usan el extremo \adfs\ls pasivo.
|/ adfs/oauth2|Se utiliza para las aplicaciones modernas (en forma local o en la nube) hayas configurado para autenticar directamente a AD FS (es decir, no a través de AAD)
|/ADFS/Services/Trust/MEX|Usar Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes posterior usan el extremo \adfs\ls pasivo.
|/ADFS/ls/FederationMetadata/2007-06/federationmetadata.Xml |Requisito de los flujos de pasivos; y usa Office 365 y Azure AD para comprobar los certificados de AD FS


Los extremos de AD FS pueden deshabilitarse en el servidor proxy mediante el siguiente cmdlet de PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por ejemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protección extendida para la autenticación
Protección extendida para la autenticación es una característica que mitiga los ataques (MITM) central de y está habilitada de forma predeterminada con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
Se puede comprobar la configuración con el debajo de cmdlet de PowerShell.  
    
   `PS:\>Get-ADFSProperties`

La propiedad es `ExtendedProtectionTokenCheck`.  El valor predeterminado es permitir, para que las ventajas de seguridad pueden lograrse sin preocuparse por la compatibilidad con exploradores que no son compatibles con la funcionalidad.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Control de congestión para proteger el servicio de federación
El proxy de servicios de federación (parte de la WAP) proporciona control congestión para proteger el servicio de AD FS de una gran cantidad de solicitudes.  El Proxy de aplicación Web rechazará las solicitudes de autenticación de cliente externo si el servidor de federación está sobrecargado con la latencia entre el Proxy de aplicación Web y el servidor de federación.  Esta característica está configurada de forma predeterminada con un nivel de umbral de latencia recomendada.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
1.  En el equipo de Proxy de aplicación Web, inicia una ventana de comandos con privilegios elevados.
2.  Navega al directorio ADFS, en % WINDIR%\adfs\config.
3.  Cambiar la configuración de control de congestión desde sus valores predeterminados para '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Guarda y cierra el archivo.
5.  Reiniciar el servicio de AD FS ejecutando 'net stop adfssrv' y 'net start adfssrv'.
Como referencia, pueden encontrarse directrices en esta funcionalidad [aquí](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Comprobaciones de la solicitud HTTP estándar en el servidor proxy
El proxy también realiza las siguientes comprobaciones estándar contra todo el tráfico:

- FS-P sí se autentica en AD FS a través de un certificado de corta duración.  En un escenario de comprometer los servidores de dmz sospechoso, AD FS puede "revocar proxy confianza" para que ya no confíe en las solicitudes entrantes de potencialmente en peligro a servidores proxy. Revocar la confianza de proxy revoca cada certificado de proxy para que no puede autenticar correctamente para ningún fin en el servidor de AD FS
- La P FS finaliza todas las conexiones y crea una nueva conexión HTTP al servicio de AD FS en la red interna. Esto proporciona un búfer de nivel de sesión entre los dispositivos externos y el servicio de AD FS. El dispositivo externo nunca se conecta directamente al servicio de AD FS.
- La P FS realiza la validación de solicitudes HTTP que filtra específicamente los encabezados HTTP que no sean necesarias para el servicio de AD FS.

## <a name="recommended-security-configurations"></a>Configuraciones de seguridad recomendada
Asegúrese de todos los AD FS y servidores WAP reciban las actualizaciones más recientes, que la recomendación de seguridad más importante para la infraestructura de AD FS es para asegurarte de que tienes un medio para mantener los servidores de AD FS y WAP actual con todas las actualizaciones de seguridad, así como las actualizaciones opcionales especificadas como importante de AD FS en esta página.

La manera recomendada para los clientes de Azure AD supervisar y mantenerte al día su infraestructura es a través de Azure AD Connect salud de AD FS, una característica de Azure AD Premium.  Estado de conectar de Azure AD incluye monitores y alertas que desencadenan si un equipo de AD FS o WAP falta una de las actualizaciones importantes específicamente para AD FS y WAP.

Información sobre cómo instalar salud conectarse de Azure AD para AD FS se puede encontrar [aquí](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configuraciones de seguridad adicional
Las siguientes capacidades adicionales pueden configurarse, opcionalmente, para proporcionar protección adicional a los que se ofrecen en la implementación predeterminada.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protección de bloqueo "suaves" extranet para las cuentas
Con la característica de bloqueo extranet en Windows Server 2012 R2, un administrador de AD FS puede establecer un número máximo de solicitudes de autenticación erróneos (ExtranetLockoutThreshold) y una "ventana de observación período de tiempo (ExtranetObservationWindow). Cuando se alcanza el número máximo (ExtranetLockoutThreshold) de las solicitudes de autenticación, AD FS deje de intentar autenticar las credenciales de cuenta proporcionada con AD FS durante el período de tiempo establecido (ExtranetObservationWindow). Esta acción protege esta cuenta de un bloqueo de cuenta de AD, en otras palabras, protege esta cuenta de perder el acceso a recursos corporativos que dependen de AD FS para la autenticación del usuario. Esta configuración se aplicará a todos los dominios que puede autenticar el servicio de AD FS.

Puedes usar el siguiente comando de Windows PowerShell para establecer el bloqueo de AD FS extranet (ejemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Como referencia, la documentación pública de esta característica es [aquí](https://technet.microsoft.com/en-us/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar las directivas de acceso de intranet y extranet acceso
AD FS tiene la capacidad para diferenciar las directivas de acceso para las solicitudes que se originan en las solicitudes de red local, corporativa vs que proceden internet a través del proxy.  Esto puede hacerse por la aplicación o global.  Para aplicaciones de valor empresarial alto o aplicaciones con la información de identificación personal o confidencial, ten en cuenta que requiere la autenticación de factor de múltiples.  Esto puede hacerse mediante el complemento Administración de AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Requerir la autenticación de factor de Multi (AMF)
AD FS puede configurarse para requerir autenticación sólida (como la autenticación de factor de multi) específicamente para las solicitudes entrantes a través del proxy, para aplicaciones individuales y de acceso condicional a ambos Azure AD y Office 365 y en los recursos locales.  Los métodos admitidos de MFA incluyen proveedores de Microsoft Azure MFA y de terceros.  Se pide al usuario a información adicional (como un texto SMS que contiene un código de tiempo), y AD FS funciona con el complemento para permitir el acceso específico de proveedor.  

Los proveedores MFA externos compatibles incluyen los indicados en [esto](https://technet.microsoft.com/en-us/library/dn758113.aspx) página, así como HDI Global.

### <a name="hardware-security-module-hsm"></a>Módulo de seguridad de hardware (HSM)
En su configuración predeterminada, los usos de claves de AD FS para firmar los tokens nunca dejar los servidores de federación en la intranet.  Nunca están presentes en la DMZ o en los equipos de proxy.  Opcionalmente, para proporcionar protección adicional, estas claves se pueden proteger un módulo de seguridad de hardware adjunto a AD FS.  Microsoft no genera un producto HSM, pero existen varias en el mercado que admiten AD FS.  Para implementar esta recomendación, sigue las instrucciones de proveedor para crear el X509 certificados para la firma y cifrado, a continuación, usar el AD FS instalación los cmdlets de powershell, especificando los certificados personalizados del siguiente modo:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

donde:


- `CertificateThumbprint` es el certificado SSL
- `SigningCertificateThumbprint` es el certificado de firma (con clave HSM protegido)
- `DecryptionCertificateThumbprint` es el certificado de cifrado (con clave HSM protegido)



