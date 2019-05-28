---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Procedimientos recomendados para proteger AD FS y Proxy de aplicación Web
description: Este documento proporciona prácticas recomendadas para la planeación segura y la implementación de servicios de federación de Active Directory (AD FS) y Proxy de aplicación Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 958bf8455d03ddc04395fafe83e70a49c7659c96
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192445"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Procedimientos recomendados para proteger los servicios de federación de Active Directory


Este documento proporciona prácticas recomendadas para la planeación segura y la implementación de servicios de federación de Active Directory (AD FS) y Proxy de aplicación Web.  Contiene información sobre los comportamientos predeterminados de estos componentes y recomendaciones para las configuraciones de seguridad adicional para una organización con casos de uso específicos y los requisitos de seguridad.

Este documento se aplica a AD FS y WAP en Windows Server 2012 R2 y Windows Server 2016 (versión preliminar).  Estas recomendaciones se pueden usar si la infraestructura se implementa en una red local o en un entorno hospedado en la nube como Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topología de implementación estándar
Para la implementación en entornos locales, se recomienda una topología de implementación estándar que consta de uno o más servidores de AD FS en la red corporativa interna, con uno o varios servidores Proxy de aplicación Web (WAP) en una red Perimetral o red de extranet.  En cada capa, AD FS y WAP, un equilibrador de carga de hardware o software se coloca delante de la granja de servidores y controla el enrutamiento de tráfico.  Los firewalls se colocan según sea necesarios en la parte delantera de la dirección IP externa del equilibrador de carga delante de cada granja de servidores (FS y proxy).

![Topología de AD FS estándar](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Puertos necesarios
El diagrama siguiente representa los puertos de firewall que deben habilitarse entre y entre los componentes de la implementación de AD FS y WAP.  Si la implementación no incluye Azure AD / Office 365, los requisitos de sincronización se puede omitir.

>Tenga en cuenta que el puerto 49443 solo es necesario si se usa la autenticación de certificados de usuario, que es opcional para Azure AD y Office 365.

![Topología de AD FS estándar](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect y servidores de federación/WAP
Esta tabla describen los puertos y protocolos que son necesarios para la comunicación entre el servidor de Azure AD Connect y servidores de federación/WAP.  

Protocol |Puertos |Descripción
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Se usa para descargar CRL (certificados listas de revocación) para comprobar los certificados SSL.
HTTPS|443(TCP/UDP)|Se utiliza para sincronizar con Azure AD.
WinRM|5985| Agente de escucha de WinRM

### <a name="wap-and-federation-servers"></a>Servidores de federación y WAP
Esta tabla describen los puertos y protocolos que son necesarios para la comunicación entre los servidores de federación y WAP.

Protocol |Puertos |Descripción
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Se usa para la autenticación.

### <a name="wap-and-users"></a>WAP y usuarios
Esta tabla describen los puertos y protocolos que son necesarios para la comunicación entre los usuarios y los servidores WAP.

Protocol |Puertos |Descripción
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Usar autenticación de dispositivos.
TCP|49443 (TCP)|Se usa para la autenticación de certificado.

Para obtener más información sobre puertos y protocolos requeridos para implementaciones híbridas, consulte el documento [aquí](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Para obtener información detallada sobre los puertos y protocolos requeridos para una instancia de Azure AD y la implementación de Office 365, consulte el documento [aquí](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Puntos de conexión habilitados

Cuando se instalan AD FS y WAP, un conjunto predeterminado de los extremos de AD FS están habilitadas en el servicio de federación y en el servidor proxy.  Estos valores predeterminados se eligieron basándose en los escenarios más se requiere y se utiliza y no es necesario cambiarlos.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Opcional] Conjunto mínimo de proxy de puntos de conexión habilitado para Azure AD / Office 365
Las organizaciones implementar AD FS y WAP solo para Azure AD y Office 365 escenarios pueden limitar aún más el número de puntos de conexión de AD FS habilitados en el servidor proxy para lograr una superficie de ataque más mínima.
A continuación es la lista de puntos de conexión que debe estar habilitada en el servidor proxy en estos escenarios:

|Extremo|Finalidad
|-----|-----
|/adfs/ls|Flujos de autenticación basada en explorador y las versiones actuales de Microsoft Office usan este punto de conexión para Azure AD y la autenticación de Office 365
|/adfs/services/trust/2005/usernamemixed|Se usan para Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes de versiones posteriores utilizan el extremo de \adfs\ls pasivo.
|/adfs/services/trust/13/usernamemixed|Se usan para Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes de versiones posteriores utilizan el extremo de \adfs\ls pasivo.
|/ adfs/oauth2|Se utiliza para las aplicaciones modernas (en su infraestructura local o en la nube) ha configurado para autenticarse directamente en AD FS (es decir, no a través de AAD)
|/adfs/services/trust/mex|Se usan para Exchange Online con los clientes de Office anteriores a la actualización de Office 2013 mayo de 2015.  Los clientes de versiones posteriores utilizan el extremo de \adfs\ls pasivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito para los flujos pasivos; y que utiliza Office 365 o Azure AD para comprobar los certificados de AD FS


Puntos de conexión de AD FS se pueden deshabilitar en el servidor proxy con el siguiente cmdlet de PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por ejemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protección ampliada para la autenticación
Protección ampliada para la autenticación es una característica que mitiga man ataques de intermediarios (MITM) y está habilitada de forma predeterminada con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
La configuración se pueda comprobar mediante el siguiente cmdlet de PowerShell.  
    
   `PS:\>Get-ADFSProperties`

La propiedad es `ExtendedProtectionTokenCheck`.  El valor predeterminado es permitir, por lo que se pueden lograr las ventajas de seguridad sin los problemas de compatibilidad con exploradores que no admiten la funcionalidad.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Control de congestión para proteger el servicio de federación
El proxy de servicio de federación (parte de lo WAP) proporciona control de congestión para proteger el servicio AD FS desde una avalancha de solicitudes.  El Proxy de aplicación Web rechazará las solicitudes de autenticación de clientes externos si el servidor de federación está sobrecargado, según lo detecte la latencia entre el Proxy de aplicación Web y el servidor de federación.  Esta característica se configura de forma predeterminada con un nivel de umbral de latencia recomendada.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
1.  En el equipo Proxy de aplicación Web, inicie una ventana de comandos con privilegios elevados.
2.  Navegue hasta el directorio de AD FS, en % WINDIR%\adfs\config.
3.  Cambiar la configuración de control de congestión de sus valores predeterminados para '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Guarde y cierre el archivo.
5.  Reinicie el servicio AD FS mediante la ejecución 'net stop adfssrv' y, a continuación, 'net start adfssrv'.
Como referencia, se puede encontrar orientación sobre esta funcionalidad [aquí](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Solicitud HTTP estándar que se comprueba en el servidor proxy
El proxy también realiza las siguientes comprobaciones estándares en todo el tráfico:

- FS-P, sí se autentica en AD FS a través de un certificado de corta duración.  En una situación de riesgo de los servidores de la red perimetral, AD FS puede "revocar confianza del proxy" para que ya no confía en todas las solicitudes entrantes de potencialmente en peligro a los servidores proxy. Al revocar la confianza del proxy impide que cada certificado de proxy para que no puede autenticarse correctamente para cualquier propósito para el servidor de AD FS
- El FS-P finaliza todas las conexiones y crea una nueva conexión de HTTP para el servicio AD FS en la red interna. Esto proporciona un búfer de nivel de sesión entre los dispositivos externos y el servicio AD FS. El dispositivo externo que nunca se conecta directamente al servicio de AD FS.
- El FS-P realiza la validación de solicitudes HTTP que filtra específicamente los encabezados HTTP que no sean necesarios para el servicio AD FS.

## <a name="recommended-security-configurations"></a>Configuraciones de seguridad recomendadas
Asegúrese de que todos los servidores de AD FS y WAP reciban las actualizaciones más recientes de que la recomendación de seguridad más importante para su infraestructura de AD FS consiste en asegurarse de que un medio para mantener los servidores de AD FS y WAP actual con todas las actualizaciones de seguridad, así como los opcionales actualizaciones especificadas como importantes para AD FS en esta página.

La manera recomendada para los clientes de Azure AD supervisar y mantener actualizada su infraestructura es a través de Azure AD Connect Health para AD FS, una característica de Azure AD Premium.  Azure AD Connect Health incluye monitores y alertas que se activan si una máquina de AD FS o WAP le falta una de las actualizaciones importantes específicamente para AD FS y WAP.

Información sobre cómo instalar Azure AD Connect Health para AD FS se encuentra [aquí](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configuraciones de seguridad adicionales
Las siguientes capacidades adicionales se pueden configurar opcionalmente para proporcionar protecciones adicionales a los que se ofrecen en la implementación predeterminada.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protección de bloqueo de la extranet "soft" para las cuentas
Con la característica de bloqueo de extranet en Windows Server 2012 R2, un administrador de AD FS puede establecer un número máximo permitido de solicitudes con error de autenticación (ExtranetLockoutThreshold) y una "ventana de observación (ExtranetObservationWindow) del período de tiempo. Cuando se alcanza este número máximo (ExtranetLockoutThreshold) de las solicitudes de autenticación, detiene AD FS intenta autenticar las credenciales de cuenta proporcionado en AD FS para el período de tiempo establecido (ExtranetObservationWindow). Esta acción protege esta cuenta de un bloqueo de cuenta de AD, en otras palabras, protege esta cuenta de perder el acceso a recursos corporativos que se basan en AD FS para la autenticación del usuario. Esta configuración se aplica a todos los dominios que puede autenticar el servicio AD FS.

Puede usar el siguiente comando de Windows PowerShell para establecer el bloqueo de extranet de AD FS (ejemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Como referencia, la documentación pública de esta característica es [aquí](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar las directivas de acceso para la intranet y extranet access
AD FS tiene la capacidad de diferenciar entre las directivas de acceso para las solicitudes que se originan en las solicitudes de vs de red corporativa local que provienen de internet a través del proxy.  Esto puede hacerse por aplicación o de forma global.  Para las aplicaciones empresariales alto valor o aplicaciones con información de identificación personal o confidencial, considere la posibilidad de requerir autenticación multifactor.  Esto puede hacerse mediante el complemento Administración de AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Requerir autenticación multifactor (MFA)
AD FS puede configurarse para requerir una autenticación segura (por ejemplo, autenticación multifactor) específicamente para las solicitudes entrantes a través del proxy, para las aplicaciones individuales y para el acceso condicional a Azure AD / Office 365 y los recursos locales.  Métodos admitidos para MFA incluyen proveedores de MFA de Azure de Microsoft y terceros.  Se le pide al usuario proporcionar la información adicional (por ejemplo, un texto SMS que contiene un código de tiempo), y AD FS funciona con el complemento para permitir el acceso específico del proveedor.  

Proveedores MFA externos compatibles incluyen los que figuran en [esto](https://technet.microsoft.com/library/dn758113.aspx) página, así como HDI Global.

### <a name="hardware-security-module-hsm"></a>Módulo de seguridad de hardware (HSM)
En su configuración predeterminada, los usos de claves de AD FS para firmar los tokens nunca deje los servidores de federación en la intranet.  Nunca están presentes en la red Perimetral o en los equipos de proxy.  Opcionalmente, para proporcionar protección adicional, se pueden proteger estas claves en un módulo de seguridad de hardware conectado a AD FS.  Microsoft no genera un producto HSM, sin embargo, hay varios en el mercado compatibles con AD FS.  Con el fin de implementar esta recomendación, siga las instrucciones del proveedor para crear el X509 certificados de firma y cifrado, a continuación, utilizar los commandlets de powershell de la instalación de AD FS, especificar los certificados personalizados como sigue:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

Donde:


- `CertificateThumbprint` es el certificado SSL
- `SigningCertificateThumbprint` es el certificado de firma (con clave HSM protegida)
- `DecryptionCertificateThumbprint` es el certificado de cifrado (con clave HSM protegida)



