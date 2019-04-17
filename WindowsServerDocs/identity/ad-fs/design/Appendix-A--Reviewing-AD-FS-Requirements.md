---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: "Apéndice a: - revisar los requisitos de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Apéndice A: revisar los requisitos de AD FS

>Se aplica a: Windows Server 2012

Para que los socios organizativos en la implementación de servicios de federación de Active Directory (AD FS) pueden colaborar correctamente, primero debes asegurarte de que la infraestructura de red corporativa está configurada para admitir los requisitos de AD FS de cuentas, la resolución de nombres y certificados. AD FS tiene los siguientes tipos de requisitos:  
  
> [!TIP]  
> Puedes encontrar vínculos adicionales de recursos de AD FS en la [AD FS contenido mapa](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) página en la Wiki de TechNet de Microsoft. Esta página es administrada por miembros de la Comunidad de AD FS y se supervisa regularmente por el equipo de AD FS.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
Los siguientes requisitos mínimos de hardware se aplican a los equipos de proxy de servidor de federación y servidor de federación.  
  
|Requisitos de hardware|Requisito mínimo|Requisito recomendado|  
|------------------------|-----------------------|---------------------------|  
|Velocidad de CPU|Núcleo único, 1 gigahercio (GHz)|2 GHz de cuatro núcleos|  
|RAM|1 GB|4 GB|  
|Espacio en disco|50 MB|100 MB|  
  
## <a name="software-requirements"></a>Requisitos de software  
AD FS se basa en la funcionalidad del servidor que está integrada en el sistema operativo de Windows Server® 2012.  
  
> [!NOTE]  
> Los servicios de rol de servicios de federación y Proxy de federación de servicio no pueden coexistir en el mismo equipo.  
  
## <a name="certificate-requirements"></a>Requisitos de certificados  
Certificados de reproducción el rol más importante en proteger las comunicaciones entre servidores de federación, proxies de servidor de federación, las aplicaciones para notificaciones y los clientes Web. Los requisitos para certificados varían en función de si estás configurando un servidor de federación o el equipo de proxy del servidor de federación, como se describe en esta sección.  
  
### <a name="federation-server-certificates"></a>Certificados de servidor de federación  
Servidores de federación requieren los certificados en la siguiente tabla.  
  
|Tipo de certificado|Descripción|Lo que necesitas saber antes de implementar|  
|--------------------|---------------|------------------------------------------|  
|Proteger el certificado de capa de Sockets (SSL)|Se trata de un certificado de capa de Sockets seguros (SSL) estándar que se usa para proteger las comunicaciones entre clientes y servidores de federación.|Este certificado debe estar enlazado en el sitio Web predeterminado en Internet Information Services (IIS) para un servidor de federación o un Proxy de federación de servidor.  Para un Proxy de servidor de federación, el enlace debe estar configurado en IIS antes de ejecutar al Asistente para configuración de Proxy de federación Server correctamente.<br /><br />**Recomendación:** porque este certificado debe ser de confianza para los clientes de AD FS, usa un certificado de autenticación de servidor emitido por una entidad de certificación públicas de (terceros) (CA), por ejemplo, VeriSign. **Sugerencia:** el nombre del sujeto del certificado que se usa para representar el nombre de servicios de federación de cada instancia de AD FS que implementas. Por este motivo, quieres considera la posibilidad de elegir un nombre de asunto en cualquier nuevos certificados emitidos que mejor represente el nombre de tu empresa u organización para los partners.|  
|Certificado de comunicación de servicio|Este certificado permite seguridad para proteger las comunicaciones entre servidores de federación de WCF los mensajes.|De manera predeterminada, el certificado SSL se usa como el certificado de comunicaciones de servicio.  Esto puede cambiarse mediante la consola de administración de AD FS.|  
|Certificado de firma de token|Este es un estándar X509 certificado que se usa para firmar todos los tokens que emite el servidor de federación de forma segura.|El certificado de firma de tokens debe contener una clave privada y debe encadenar a raíz de confianza en el servicio de federación. De manera predeterminada, AD FS crea un certificado autofirmado. Sin embargo, puedes cambiar esto más adelante a un certificado emitido por entidad emisora de certificados mediante el complemento de administración de AD FS, según las necesidades de la organización.|  
|Certificado de descifrado de token|Se trata de un certificado SSL estándar que se usa para descifrar los tokens de entrantes se cifran mediante un servidor de federación de partners. También se publica en los metadatos de federación.|De manera predeterminada, AD FS crea un certificado autofirmado. Sin embargo, puedes cambiar esto más adelante a un certificado emitido por entidad emisora de certificados mediante el complemento de administración de AD FS, según las necesidades de la organización.|  
  
> [!CAUTION]  
> Certificados que se usan para la firma de token y el descifrado de token son esenciales para la estabilidad del servicio de federación. Dado que una pérdida o imprevisto retirada de los certificados que están configurados para este propósito puede alterar el servicio, deben copias de seguridad de los certificados que están configurados para este propósito.  
  
Para obtener más información acerca de los certificados que utilizan los servidores de federación, consulta [requisitos de certificado para los servidores de federación](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificados de servidor proxy de federación  
Federación proxies de servidor los certificados en la siguiente tabla.  
  
|Tipo de certificado|Descripción|Lo que necesitas saber antes de implementar|  
|--------------------|---------------|------------------------------------------|  
|Certificado de autenticación de servidor|Se trata de un certificado de capa de Sockets seguros (SSL) estándar que se usa para proteger las comunicaciones entre un cliente de Internet y de proxy del servidor de federación equipos.|Este certificado debe estar enlazado en el sitio Web predeterminado en Internet Information Services (IIS) antes de ejecutar al Asistente para configuración de Proxy de federación de servidor de AD FS correctamente.<br /><br />**Recomendación:** porque este certificado debe ser de confianza para los clientes de AD FS, usa un certificado de autenticación de servidor emitido por una entidad de certificación públicas de (terceros) (CA), por ejemplo, VeriSign.<br /><br />**Sugerencia:** el nombre del sujeto del certificado que se usa para representar el nombre de servicios de federación de cada instancia de AD FS que implementas. Por este motivo, quieres considera la posibilidad de elegir un nombre de sujeto que mejor represente el nombre de tu empresa u organización para los partners.|  
  
Para obtener más información acerca de los certificados que usan proxy del servidor de federación, consulta [requisitos de certificado para Proxies de servidor de federación](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Requisitos del explorador  
Aunque los exploradores Web actuales con funcionalidad de JavaScript pueden realizarse para que funcione como un cliente de AD FS, las páginas Web que se proporcionan de manera predeterminada se han probado sólo con respecto a versiones de Internet Explorer 7.0, 8.0 y 9.0, Mozilla Firefox 3.0 y Safari 3.1 en Windows. JavaScript debe estar habilitado y las cookies deben estar habilitado para el inicio de sesión basado en explorador y cierre de sesión para que funcione correctamente.  
  
El equipo de producto de AD FS de Microsoft ha probado las configuraciones del explorador y el sistema operativo en la siguiente tabla.  
  
|Explorador|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|No se ha probado|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS admite tanto la 32 y 64 bits de las versiones de todos los exploradores que se muestra en la tabla anterior.  
  
### <a name="cookies"></a>Cookies  
AD FS crea cookies de sesión y persistentes que deben almacenarse en los equipos cliente para proporcionar el inicio de sesión único, cierre de sesión (SSO) y otras funciones. Por lo tanto, el explorador del cliente debe configurarse para que acepte cookies. Las cookies que se usan para la autenticación siempre son las cookies de sesión de protocolo seguro de transferencia de hipertexto (HTTPS) que se han escrito para el servidor de origen. Si el explorador del cliente no está configurado para permitir que estas cookies, AD FS no puede funcionar correctamente. Las cookies persistentes se usan para conservar la selección del usuario de que el proveedor de notificaciones. Para deshabilitarlas mediante el uso de una opción de configuración en el archivo de configuración para las páginas de inicio de sesión de AD FS.  
  
Compatibilidad con TLS/SSL es necesario por motivos de seguridad.  
  
## <a name="network-requirements"></a>Requisitos de la red  
Correctamente la configuración de los siguientes servicios de red es fundamental para la correcta implementación de AD FS de la organización.  
  
### <a name="tcpip-network-connectivity"></a>Conectividad de red TCP/IP  
Para que AD FS funcione, debe existir conectividad de red TCP/IP entre el cliente. un controlador de dominio; y los equipos que hospedan el servicio de federación, el Proxy de servicios de federación (cuando se usa) y el agente de Web de AD FS.  
  
### <a name="dns"></a>DNS  
El servicio de red principal que es fundamental para el funcionamiento de AD FS, que no sean los servicios de dominio de Active Directory (AD DS), es el sistema de nombres de dominio (DNS). Cuando se implementa DNS, los usuarios pueden usar nombres de equipo descriptivo que son fáciles de recordar para conectarse a equipos y otros recursos en redes IP.  
  
 Windows Server 2008 utiliza DNS para resolver el nombre en lugar de la resolución de nombre NetBIOS del servicio de nombres de Windows Internet (WINS) que se usó en redes basadas en Windows NT 4.0. Es posible usar WINS para las aplicaciones que lo necesitan. Sin embargo, AD DS y AD FS requieren una resolución de nombre DNS.  
  
El proceso de configuración de DNS para admitir AD FS varía en función de si:  
  
-   La organización ya tiene una infraestructura de DNS. En la mayoría de los escenarios DNS ya está configurado en toda la red para que los clientes del explorador Web en la red corporativa tengan acceso a Internet. Dado que Internet acceso y resolución de nombres son los requisitos de AD FS, esta infraestructura se supone que en su lugar para la implementación de AD FS.  
  
-   Se va a agregar un servidor federado a la red corporativa. Con el fin de autenticar a los usuarios de la red corporativa, los servidores DNS internos en el bosque de la red corporativa deben configurarse para devolver el registro CNAME del servidor interno que se está ejecutando el servicio de federación. Para obtener más información, consulta [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Se va a agregar a un servidor proxy server federados a la red de perímetro. Cuando quieras autentican cuentas de usuario que se encuentran en la red corporativa de la organización de partner de identidad, los servidores DNS internos en el bosque de la red corporativa deben configurarse para devolver el registro CNAME de proxy del servidor de federación interno. Para obtener información sobre cómo configurar DNS para dar cabida a la adición de proxies de servidor de federación, vea [requisitos de resolución de nombre de Proxies de servidor de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Estás configurando DNS para un entorno de laboratorio de prueba. Si tienes previsto usar AD FS en un entorno de laboratorio de prueba donde ningún servidor DNS de raíz está autorizado, es probable que tendrás que configurar los servidores de reenvío DNS para que se reenviará consultas a los nombres de entre dos o más bosques correctamente. Para obtener información general sobre cómo configurar un entorno de laboratorio de prueba de AD FS, consulta [AD FS paso a paso y cómo a las guías](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Requisitos de la tienda de atributo  
AD FS requiere al menos un almacén de atributo que se usará para autenticar usuarios y notificaciones de seguridad para los usuarios de la extracción. Para obtener una lista del atributo almacena que es compatible con AD FS, consulta [el rol de atributo almacena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) en la Guía de diseño de AD FS.  
  
> [!NOTE]  
> AD FS crea automáticamente un almacén de atributo de Active Directory, de manera predeterminada.  
  
Requisitos de la tienda de atributo dependen de si la organización actúa como el asociado de cuenta (aloja los usuarios federados) o el partner de recursos (hospeda la aplicación federada).  
  
### <a name="ad-ds"></a>AD DS  
De AD FS para que funcionen correctamente, controladores de dominio en la organización de partner de cuenta o la organización de partner de recursos deben ejecutar Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 o Windows Server 2012.  
  
Cuando AD FS está instalado y configurado en un equipo unido al dominio, el almacén de cuentas de usuario de Active Directory para ese dominio se pone a disposición como un almacén de atributo seleccionable.  
  
> [!IMPORTANT]  
> Dado que AD FS requiere la instalación de Internet Information Services (IIS), te recomendamos que no puedes instalar el software de AD FS en un controlador de dominio en un entorno de producción por motivos de seguridad. Sin embargo, esta configuración es compatible con soporte de servicio al cliente de Microsoft.  
  
#### <a name="schema-requirements"></a>Requisitos de esquema  
AD FS no requiere cambios de esquema o modificaciones de nivel funcional en AD DS.  
  
#### <a name="functional-level-requirements"></a>Requisitos de nivel funcional  
La mayoría de características de AD FS no requieren modificaciones de nivel funcional de AD DS para que funcionen correctamente. Sin embargo, funcional del dominio de Windows Server 2008 nivel o superior es necesario para la autenticación de certificado de cliente para que funcionen correctamente si el certificado se asigne explícitamente a una cuenta de usuario en AD DS.  
  
#### <a name="service-account-requirements"></a>Requisitos de la cuenta de servicio  
Si vas a crear una granja de servidores de federación, primero debes crear una cuenta de servicio basado en dominio dedicada en AD DS que puedes usar el servicio de federación. Más adelante, configuran cada servidor de federación de la granja para usar esta cuenta. Para obtener más información sobre cómo hacerlo, consulta [configurar manualmente una cuenta de servicio para una granja de servidores de federación](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) en la Guía de implementación de AD FS.  
  
### <a name="ldap"></a>LDAP  
Cuando trabajes con otros almacenes atributo basado en el protocolo de acceso ligero directorios LDAP, deberá conectarse a un servidor LDAP que admite la autenticación integrada de Windows. La cadena de conexión de LDAP debe escribirse en el formato de una dirección URL de LDAP, como se describe en la RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Para que AD FS para que funcionen correctamente, los equipos que hospedar el almacén de atributo de servidor de lenguaje de consulta estructurado (SQL) deben estar ejecutándose Microsoft SQL Server 2005 o SQL Server 2008. Cuando trabajas con el atributo basado en SQL tiendas, también debes configurar una cadena de conexión.  
  
### <a name="custom-attribute-stores"></a>Almacenes de atributo personalizado  
Puedes desarrollar almacenes atributo personalizado para habilitar escenarios avanzados. El lenguaje sobre directivas que está integrado en AD FS hacer referencia a los almacenes de atributo personalizado para que se puedan mejorar cualquiera de los siguientes escenarios:  
  
-   Creación de notificaciones para un usuario autenticado localmente  
  
-   Complementan reclamaciones por un usuario autenticado externamente  
  
-   Autorización de un usuario para obtener un token  
  
-   Autorización de un servicio para obtener un token en el comportamiento de un usuario  
  
Cuando se trabaja con un almacén de atributo personalizado, también puede tener que configurar una cadena de conexión. En esta situación, puedes escribir ningún código personalizado que quieras que habilita una conexión a la tienda de atributo personalizado. La cadena de conexión en esta situación es un conjunto de pares nombre-valor que se interpretan como implementados por el desarrollador de la tienda de atributo personalizado.  
  
Para obtener más información acerca del desarrollo y usando el atributo personalizado tiendas, consulta [Introducción a la tienda atributo](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Requisitos de la aplicación  
Los servidores de federación pueden comunicarse con y proteger las aplicaciones de federación, como las notificaciones.  
  
## <a name="authentication-requirements"></a>Requisitos de autenticación  
AD FS integra naturalmente con autenticación de Windows existente, por ejemplo, la autenticación Kerberos, NTLM, tarjetas inteligentes y los certificados de cliente X.509 v3. Los servidores de federación usan autenticación de Kerberos estándar para autenticar un usuario con un dominio. Los clientes pueden autenticar mediante autenticación basada en formularios, la autenticación con tarjeta inteligente y la autenticación integrada de Windows, dependiendo de cómo configurar la autenticación.  
  
La función de proxy del servidor de federación de AD FS hace posible en un escenario en el que el usuario autentica externamente mediante la autenticación de cliente SSL. También puedes configurar el rol de servidor de federación para requerir la autenticación de clientes SSL, aunque generalmente la mejor experiencia del usuario se logra mediante la configuración del servidor de federación de cuenta para la autenticación integrada de Windows. En esta situación, AD FS no tiene control sobre las credenciales que el usuario se emplea para el inicio de sesión de escritorio de Windows.  
  
### <a name="smart-card-logon"></a>Inicio de sesión de tarjeta inteligente  
Aunque AD FS puede aplicar el tipo de las credenciales que usa para la autenticación (contraseñas, autenticación de clientes SSL o autenticación integrada de Windows), no directamente obliga a la autenticación con tarjetas inteligentes. Por lo tanto, AD FS no proporciona una interfaz de usuario de cliente (interfaz de usuario) para obtener las credenciales de número (PIN) de identificación personal de tarjeta inteligente. Esto es porque intencionadamente clientes de Windows sí proporcionan detalles de la credencial de usuario a los servidores de federación o servidores Web.  
  
### <a name="smart-card-authentication"></a>Autenticación con tarjeta inteligente  
Autenticación con tarjeta inteligente, usa el protocolo Kerberos para autenticar a un servidor de federación de cuenta. AD FS no se puede extender para agregar nuevos métodos de autenticación. El certificado en la tarjeta inteligente no es necesario para la cadena de hasta una raíz de confianza en el equipo cliente. Uso de un certificado basados en tarjeta inteligente con AD FS requiere las siguientes condiciones:  
  
-   El lector y el proveedor de servicios criptográficos (CSP) de la tarjeta inteligente deben funcionar en el equipo donde se encuentra el explorador.  
  
-   El certificado de tarjeta inteligente debe encadenas a una raíz de confianza en el servidor de federación de cuenta y el proxy de servidor de federación de cuenta.  
  
-   El certificado debe asignar a la cuenta de usuario en AD DS por cualquiera de los siguientes métodos:  
  
    -   El nombre de firmante del certificado que se corresponde con el nombre completo de LDAP de una cuenta de usuario en AD DS.  
  
    -   La extensión de certificado asunto altname tiene el nombre principal de usuario (UPN) de una cuenta de usuario en AD DS.  
  
Para admitir ciertos requisitos de seguridad de autenticación en algunos escenarios, también es posible configurar AD FS para crear una notificación que indica cómo se ha autenticado al usuario. Un usuario de confianza, a continuación, puedes usar esta notificación para tomar una decisión de autorización.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
