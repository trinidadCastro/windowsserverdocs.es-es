---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Prácticas recomendadas para proteger AD FS y el proxy de aplicación Web
description: En este documento se proporcionan prácticas recomendadas para el planeamiento y la implementación seguros de Servicios de federación de Active Directory (AD FS) (AD FS) y el proxy de aplicación Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 15b0c721b620e2891f4452fd54501f4970b7c177
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360002"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Prácticas recomendadas para proteger Servicios de federación de Active Directory (AD FS)


En este documento se proporcionan prácticas recomendadas para el planeamiento y la implementación seguros de Servicios de federación de Active Directory (AD FS) (AD FS) y el proxy de aplicación Web.  Contiene información sobre los comportamientos predeterminados de estos componentes y recomendaciones para las configuraciones de seguridad adicionales para una organización con requisitos de seguridad y casos de uso específicos.

Este documento se aplica a AD FS y WAP en Windows Server 2012 R2 y Windows Server 2016 (versión preliminar).  Estas recomendaciones se pueden usar tanto si la infraestructura está implementada en una red local como si se encuentra en un entorno hospedado en la nube, como Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topología de implementación estándar
Para la implementación en entornos locales, se recomienda una topología de implementación estándar que consta de uno o varios servidores de AD FS en la red corporativa interna, con uno o varios servidores de proxy de aplicación web (WAP) en una red perimetral o extranet.  En cada capa, AD FS y WAP, se coloca un equilibrador de carga de hardware o software delante de la granja de servidores y controla el enrutamiento del tráfico.  Los firewalls se colocan según sea necesario delante de la dirección IP externa del equilibrador de carga delante de cada granja de servidores (FS y proxy).

![Topología estándar de AD FS](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Puertos necesarios
En el diagrama siguiente se muestran los puertos de firewall que se deben habilitar entre y entre los componentes de la implementación de AD FS y WAP.  Si la implementación no incluye Azure AD/Office 365, se pueden omitir los requisitos de sincronización.

>Tenga en cuenta que el puerto 49443 solo es necesario si se usa la autenticación de certificado de usuario, que es opcional para Azure AD y Office 365.

![Topología estándar de AD FS](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> El puerto 808 (Windows Server 2012R2) o el puerto 1501 (Windows Server 2016 +) es el puerto net. TCP AD FS usa para que el punto de conexión de WCF local transfiera los datos de configuración al proceso de servicio y PowerShell. Este puerto se puede ver mediante la ejecución de Get-AdfsProperties | Seleccione NetTcpPort. Se trata de un puerto local que no necesitará abrirse en el firewall, pero se mostrará en un examen de puerto. 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Servidores de Azure AD Connect y Federación/WAP
En esta tabla se describen los puertos y protocolos necesarios para la comunicación entre el servidor de Azure AD Connect y los servidores de Federación/WAP.  

Protocol |Puertos |Descripción
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Se usa para descargar CRL (listas de revocación de certificados) para comprobar los certificados SSL.
HTTPS|443 (TCP/UDP)|Se utiliza para sincronizar con Azure AD.
WinRM|5985| Agente de escucha de WinRM

### <a name="wap-and-federation-servers"></a>Servidores WAP y de Federación
En esta tabla se describen los puertos y protocolos necesarios para la comunicación entre los servidores de Federación y los servidores WAP.

Protocol |Puertos |Descripción
--------- | --------- |---------
HTTPS|443 (TCP/UDP)|Se utiliza para la autenticación.

### <a name="wap-and-users"></a>WAP y usuarios
En esta tabla se describen los puertos y protocolos necesarios para la comunicación entre los usuarios y los servidores WAP.

Protocol |Puertos |Descripción
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)|Se usa para la autenticación de dispositivos.
TCP|49443 (TCP)|Se utiliza para la autenticación de certificados.

Para obtener más información sobre los puertos y protocolos necesarios para las implementaciones híbridas, consulte el documento [aquí](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Para obtener información detallada sobre los puertos y protocolos necesarios para una Azure AD y la implementación de Office 365, consulte el documento [aquí](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Extremos habilitados

Cuando se instalan AD FS y WAP, se habilita un conjunto predeterminado de puntos de conexión de AD FS en el servicio de Federación y en el proxy.  Estos valores predeterminados se eligieron en función de los escenarios más necesarios y usados, y no es necesario cambiarlos.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>Opta Conjunto mínimo de servidores proxy de extremos habilitado para Azure AD/Office 365
Las organizaciones que implementan AD FS y WAP solo para escenarios de Azure AD y Office 365 pueden limitar aún más el número de puntos de conexión de AD FS habilitados en el proxy para lograr una superficie de ataque más mínima.
A continuación se muestra la lista de puntos de conexión que se deben habilitar en el proxy en estos escenarios:

|Extremo|Finalidad
|-----|-----
|/ADFS/LS|Flujos de autenticación basados en explorador y versiones actuales de Microsoft Office usar este punto de conexión para la autenticación de Azure AD y Office 365
|/adfs/services/trust/2005/usernamemixed|Se usa para Exchange Online con clientes de Office anteriores a Office 2013, 2015 de mayo de actualización.  Los clientes posteriores usan el punto de conexión \adfs\ls pasivo.
|/adfs/services/trust/13/usernamemixed|Se usa para Exchange Online con clientes de Office anteriores a Office 2013, 2015 de mayo de actualización.  Los clientes posteriores usan el punto de conexión \adfs\ls pasivo.
|/adfs/oauth2|Se usa para todas las aplicaciones modernas (locales o en la nube) que haya configurado para autenticarse directamente en AD FS (es decir, no a través de AAD).
|/adfs/services/trust/mex|Se usa para Exchange Online con clientes de Office anteriores a Office 2013, 2015 de mayo de actualización.  Los clientes posteriores usan el punto de conexión \adfs\ls pasivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito para los flujos pasivos; y lo usan Office 365/Azure AD para comprobar los certificados de AD FS


AD FS puntos de conexión se pueden deshabilitar en el proxy con el siguiente cmdlet de PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por ejemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protección ampliada para la autenticación
La protección ampliada para la autenticación es una característica que soluciona los ataques de tipo "Man in the Middle" (MITM) y está habilitada de forma predeterminada con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
La configuración se puede comprobar mediante el commandlet de PowerShell siguiente.  
    
   `PS:\>Get-ADFSProperties`

La propiedad es `ExtendedProtectionTokenCheck`.  La configuración predeterminada es permitir, de modo que se puedan lograr las ventajas de seguridad sin la preocupación de compatibilidad con los exploradores que no admiten la funcionalidad.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Control de congestión para proteger el servicio de Federación
El proxy del servicio de Federación (parte del WAP) proporciona control de congestión para proteger el servicio AD FS de una avalancha de solicitudes.  El proxy de aplicación web rechazará las solicitudes de autenticación de clientes externos si el servidor de Federación está sobrecargado como detectado por la latencia entre el proxy de aplicación web y el servidor de Federación.  Esta característica está configurada de forma predeterminada con un nivel de umbral de latencia recomendado.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para comprobar la configuración, puede hacer lo siguiente:
1.  En el equipo del proxy de aplicación Web, inicie una ventana de comandos con privilegios elevados.
2.  Navegue hasta el directorio de ADFS, en%WINDIR%\adfs\config.
3.  Cambie la configuración del control de congestión de sus valores<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />predeterminados a ' '.
4.  Guarde y cierre el archivo.
5.  Reinicie el servicio AD FS ejecutando ' net stop adfssrv ' y, a continuación, ' net start adfssrv '.
Para su referencia, puede encontrar instrucciones sobre esta funcionalidad [aquí](https://msdn.microsoft.com/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Comprobaciones de solicitud HTTP estándar en el proxy
El proxy también realiza las siguientes comprobaciones estándar en todo el tráfico:

- El propio FS-P se autentica en AD FS a través de un certificado de corta duración.  En un escenario en el que se sospeche el riesgo de los servidores de la red perimetral, AD FS puede "revocar la confianza del proxy" para que ya no confíe en las solicitudes entrantes de servidores proxy potencialmente comprometidos. La revocación de la confianza del proxy revoca el certificado propio de cada proxy para que no pueda autenticarse correctamente para ningún propósito en el servidor de AD FS
- FS-P finaliza todas las conexiones y crea una nueva conexión HTTP con el servicio de AD FS en la red interna. Esto proporciona un búfer de nivel de sesión entre dispositivos externos y el servicio de AD FS. El dispositivo externo nunca se conecta directamente al servicio AD FS.
- FS-P realiza la validación de solicitudes HTTP que filtra específicamente los encabezados HTTP que no son necesarios para el servicio de AD FS.

## <a name="recommended-security-configurations"></a>Configuraciones de seguridad recomendadas
Asegúrese de que todos los servidores AD FS y WAP reciban las actualizaciones más recientes. la recomendación de seguridad más importante para su infraestructura de AD FS es asegurarse de que dispone de un medio para mantener actualizados los servidores AD FS y WAP con todas las actualizaciones de seguridad, así como las opcionales las actualizaciones especificadas como importantes para AD FS en esta página.

La manera recomendada para que los clientes de Azure AD supervisen y mantengan su infraestructura actual es a través de Azure AD Connect Health para AD FS, una característica de Azure AD Premium.  Azure AD Connect Health incluye monitores y alertas que se activan si una máquina AD FS o WAP no tiene una de las actualizaciones importantes específicamente para AD FS y WAP.

Puede encontrar información sobre la instalación de Azure AD Connect Health para AD FS [aquí](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configuraciones de seguridad adicionales
Las siguientes funcionalidades adicionales se pueden configurar opcionalmente para proporcionar protecciones adicionales a las ofrecidas en la implementación predeterminada.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protección contra bloqueos "soft" de extranet para cuentas
Con la característica de bloqueo de la Extranet en Windows Server 2012 R2, un administrador de AD FS puede establecer un número máximo permitido de solicitudes de autenticación erróneas (ExtranetLockoutThreshold) y un período de tiempo de la ventana de observación (ExtranetObservationWindow). Cuando se alcanza este número máximo (ExtranetLockoutThreshold) de solicitudes de autenticación, AD FS deja de intentar autenticar las credenciales de la cuenta proporcionada con AD FS durante el período de tiempo establecido (ExtranetObservationWindow). Esta acción protege esta cuenta de un bloqueo de cuenta de AD. en otras palabras, evita que esta cuenta pierda el acceso a los recursos corporativos que confían en AD FS para la autenticación del usuario. Esta configuración se aplica a todos los dominios que puede autenticar el servicio AD FS.

Puede usar el siguiente comando de Windows PowerShell para establecer el AD FS el bloqueo de la extranet (ejemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Como referencia, la documentación pública de esta característica está [aquí](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>Deshabilitar los puntos de conexión de Windows de WS-Trust en el proxy, es decir, desde la extranet

Los puntos de conexión de Windows de WS-Trust ( */ADFS/Services/Trust/2005/windowstransport* y */ADFS/Services/Trust/13/windowstransport*) están diseñados únicamente para ser puntos de conexión orientados a la intranet que usan el enlace WIA en https. Exponerlos a la extranet podría permitir que las solicitudes a estos puntos de conexión omitan las protecciones de bloqueo. Estos extremos deben deshabilitarse en el proxy (es decir, deshabilitarse de la extranet) para proteger el bloqueo de cuentas de AD mediante los siguientes comandos de PowerShell. No hay ningún impacto en el usuario final conocido mediante la deshabilitación de estos puntos de conexión en el proxy.

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar las directivas de acceso para el acceso a la intranet y la extranet
AD FS tiene la capacidad de diferenciar las directivas de acceso para las solicitudes que se originan en la red corporativa local frente a las solicitudes que proceden de Internet a través del proxy.  Esto se puede hacer por aplicación o globalmente.  En el caso de aplicaciones de valor de gran negocio o aplicaciones con información confidencial o de identificación personal, considere la posibilidad de requerir autenticación multifactor.  Esto puede realizarse a través del complemento Administración de AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Requerir multi-factor Authentication (MFA)
AD FS puede configurarse para requerir una autenticación segura (como multi-factor Authentication) específicamente para las solicitudes que entran a través del proxy, para las aplicaciones individuales y para el acceso condicional a los recursos de Azure AD/Office 365 y locales.  Los métodos admitidos de MFA incluyen Microsoft Azure MFA y proveedores de terceros.  Se solicita al usuario que proporcione la información adicional (como un texto de SMS que contiene un código de un solo tiempo) y AD FS funciona con el complemento específico del proveedor para permitir el acceso.  

Entre los proveedores de MFA externos admitidos [se](https://technet.microsoft.com/library/dn758113.aspx) incluyen los que se enumeran en esta página, así como HDI global.

### <a name="hardware-security-module-hsm"></a>Módulo de seguridad de hardware (HSM)
En su configuración predeterminada, las claves AD FS usa para firmar los tokens nunca salen de los servidores de Federación en la intranet.  Nunca están presentes en la red perimetral o en las máquinas proxy.  Opcionalmente, para proporcionar protección adicional, estas claves se pueden proteger en un módulo de seguridad de hardware asociado a AD FS.  Microsoft no produce un producto HSM, pero hay varios en el mercado que admiten AD FS.  Para implementar esta recomendación, siga las instrucciones del proveedor para crear los certificados X509 para la firma y el cifrado y, a continuación, use el commandlets de PowerShell de instalación de AD FS, especificando los certificados personalizados como se indica a continuación:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

Donde:


- `CertificateThumbprint`es el certificado SSL
- `SigningCertificateThumbprint`es el certificado de firma (con clave protegida HSM)
- `DecryptionCertificateThumbprint`es el certificado de cifrado (con clave protegida HSM)



