---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: 'Apéndice A: revisión de los requisitos de AD FS'
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ad3184dbe43cfa108aa1b178421102880b3ef4d7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994992"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Apéndice A: Revisión de los requisitos de AD FS

Para que los asociados organizativos de su implementación de Servicios de federación de Active Directory (AD FS) (AD FS) puedan colaborar correctamente, primero debe asegurarse de que la infraestructura de la red corporativa está configurada para admitir los requisitos de AD FS para las cuentas, la resolución de nombres y los certificados. AD FS tiene los tipos siguientes de requisitos:

> [!TIP]
> Puede encontrar vínculos de recursos de AD FS adicionales en la descripción de los [conceptos clave de AD FS](../technical-reference/understanding-key-ad-fs-concepts.md).

## <a name="hardware-requirements"></a>Requisitos de hardware
Los siguientes requisitos de hardware mínimos y recomendados se aplican a los equipos del servidor de Federación y del servidor proxy de Federación.

|Requisito de hardware|Requisito mínimo|Requisito recomendado|
|------------------------|-----------------------|---------------------------|
|Velocidad de CPU|Procesador de un núcleo, 1 GHz|Cuatro núcleos, 2 GHz|
|RAM|1 GB|4 GB|
|Espacio en disco|50 MB|100 MB|

## <a name="software-requirements"></a>Requisitos de software
AD FS se basa en la funcionalidad de servidor integrada en el &reg; sistema operativo Windows server 2012.

> [!NOTE]
> Los servicios de rol del servicio de federación y del proxy de servicio de federación no pueden coexistir en el mismo ordenador.

## <a name="certificate-requirements"></a>Requisitos de certificados
Los certificados desempeñan una función crucial a la hora de proteger las comunicaciones entre los servidores de federación, los servidores proxy de federación, las aplicaciones compatibles con notificaciones y los clientes web. Los requisitos de los certificados varían en función de si se configura un equipo de servidor de federación o de servidor proxy de federación, tal y como se describe en esta sección.

### <a name="federation-server-certificates"></a>Certificados del servidor de federación
Los servidores de federación necesitan los certificados de la siguiente tabla.

|Tipo de certificado|Descripción|Qué debes saber antes de la implementación|
|--------------------|---------------|------------------------------------------|
|certificado de Capa de sockets seguros (SSL)|Se trata de un certificado estándar de Capa de sockets seguros (SSL) utilizado para proteger las comunicaciones entre los servidores de federación y los clientes.|Este certificado debe estar enlazado al sitio web predeterminado en Internet Information Services (IIS) para un servidor de federación o un servidor proxy de federación.  En el caso de un servidor proxy de federación, el enlace debe configurarse en IIS antes de que pueda ejecutarse correctamente el Asistente para configuración de servidor proxy de federación.<p>**Recomendación:** Dado que este certificado debe ser de confianza para los clientes de AD FS, utiliza un certificado de autenticación de servidor emitido por una entidad de certificación (CA) pública (terceros), como VeriSign. **Sugerencia:** El nombre del firmante de este certificado se usa para representar el nombre del servicio de federación para cada instancia de AD FS que implementes. Por esta razón, tal vez te interese elegir un nombre de firmante en los certificados nuevos emitidos por CA que a los asociados les resulte más representativo del nombre de tu empresa u organización.|
|Certificado de comunicación de servicios|Este certificado habilita la seguridad de mensajes WCF para proteger las comunicaciones entre los servidores de federación.|De manera predeterminada, se utiliza el certificado SSL como certificado de comunicaciones de servicio.  Esto puede cambiarse a través de la consola de administración de AD FS.|
|Certificado de firma de tokens|Se trata de un certificado X509 estándar que se utiliza para firmar de manera segura todos los tokens que emite el servidor de federación.|El certificado de firma de tokens debe contener una clave privada y debería estar encadenado a una raíz de confianza en el servicio de federación. De manera predeterminada, AD FS crea un certificado autofirmado. Sin embargo, puedes cambiarlo más adelante a un certificado emitido por una CA a través del complemento de administración de AD FS, en función de las necesidades de tu organización.|
|Certificado de descifrado de token|Se trata de un certificado SSL estándar que se utiliza para descifrar los tokens entrantes que haya cifrado un servidor de federación asociado. También se publica en los metadatos de federación.|De manera predeterminada, AD FS crea un certificado autofirmado. Sin embargo, puedes cambiarlo más adelante a un certificado emitido por una CA a través del complemento de administración de AD FS, en función de las necesidades de tu organización.|

> [!CAUTION]
> Los certificados que se usan para la firma de tokens y el descifrado de tokens son cruciales para la estabilidad del servicio de federación. Dado que una pérdida o una eliminación accidental de los certificados configurados con este fin podría interrumpir el servicio, debes hacer copias de seguridad de todos los certificados configurados con este fin.

Para obtener más información sobre los certificados que utilizan los servidores de federación, consulta [Requisitos de los certificados para servidores de federación](Certificate-Requirements-for-Federation-Servers.md).

### <a name="federation-server-proxy-certificates"></a>Certificados del servidor proxy de federación
Los servidores proxy de federación necesitan los certificados de la siguiente tabla.

|Tipo de certificado|Descripción|Qué debes saber antes de la implementación|
|--------------------|---------------|------------------------------------------|
|Certificado de autenticación de servidor|Se trata de un certificado estándar de Capa de sockets seguros (SSL) utilizado para proteger las comunicaciones entre un servidor proxy de federación y equipos cliente de Internet.|Este certificado debe estar enlazado al sitio web predeterminado en Internet Information Services (IIS) para que pueda ejecutarse correctamente el Asistente para configuración de servidor proxy de federación de AD FS.<p>**Recomendación:** Dado que este certificado debe ser de confianza para los clientes de AD FS, utiliza un certificado de autenticación de servidor emitido por una entidad de certificación (CA) pública (terceros), como VeriSign.<p>**Sugerencia:** El nombre del firmante de este certificado se usa para representar el nombre del servicio de federación para cada instancia de AD FS que implementes. Por esta razón, tal vez te interese elegir un nombre de firmante que a los asociados les resulte más representativo del nombre de tu empresa u organización.|

Para obtener más información sobre los certificados que utilizan los servidores proxy de federación, consulta [Requisitos de los certificados para servidores proxy de federación](Certificate-Requirements-for-Federation-Server-Proxies.md).

## <a name="browser-requirements"></a>Requisitos del explorador
Aunque todos los exploradores web actuales con funcionalidad JavaScript pueden funcionar como cliente AD FS, las páginas web que se proporcionan de manera predeterminada solo se han probado con Internet Explorer versión 7.0, 8.0 y 9.0, Mozilla Firefox 3.0 y Safari 3.1 en Windows. JavaScript debe estar habilitado y las cookies deben estar habilitadas para que el inicio y cierre de sesión basado en el explorador funcione correctamente.

El equipo de AD FS producto de Microsoft probó correctamente las configuraciones del explorador y del sistema operativo en la tabla siguiente.

|Browser|Windows 7|Windows Vista|
|-----------|-------------|-----------------|
|Internet Explorer 7.0|X|X|
|Internet Explorer 8.0|X|X|
|Internet Explorer 9.0|X|No probado|
|Firefox 3.0|X|X|
|Safari 3.1|X|X|

> [!NOTE]
> AD FS es compatible con las versiones de 32 y 64 bits de todos los exploradores mostrados en la tabla anterior.

### <a name="cookies"></a>Cookies
AD FS crea cookies persistentes y basadas en sesión que deben almacenarse en equipos cliente para proporcionar funcionalidades de inicio de sesión, cierre de sesión, inicio de sesión único (SSO) y demás. Por lo tanto, el explorador cliente debe estar configurado para aceptar cookies. Las cookies usadas para la autenticación son siempre cookies de sesión de protocolo seguro de transferencia de hipertexto (HTTPS) escritas para el servidor de origen. Si el explorador cliente no está configurado para permitir estas cookies, AD FS no funcionará correctamente. Las cookies persistentes se usan para conservar la selección del usuario del proveedor de notificaciones. Puedes deshabilitarlas a través de una opción del archivo de configuración para las páginas de inicio de sesión de AD FS.

Es necesaria la compatibilidad con TLS/SSL por motivos de seguridad.

## <a name="network-requirements"></a>Requisitos de red
Configurar de manera apropiada los siguientes servicios de red es fundamental para una correcta implementación de AD FS en tu organización.

### <a name="tcpip-network-connectivity"></a>Conectividad de red TCP/IP
Para que AD FS funcione, debe existir una conectividad de red TCP/IP entre el cliente, un controlador de dominio y los equipos que hospedan el Servicio de federación, el proxy de Servicio de federación (si se usa) y el agente web de AD FS.

### <a name="dns"></a>DNS
El servicio de red principal que es fundamental para el funcionamiento de AD FS, que no sea Active Directory Domain Services (AD DS), es el sistema de nombres de dominio (DNS). Si se implementa DNS, los usuarios pueden utilizar nombres de equipo descriptivos que sean más fáciles de recordar a la hora de conectarse a equipos y otros recursos en redes IP.

 Windows Server 2008 usa DNS para la resolución de nombres en lugar de la resolución de nombres NetBIOS del servicio de nombres Internet de Windows (WINS) que se usaba en las redes basadas en Windows NT 4.0. Todavía es posible utilizar WINS para las aplicaciones que lo necesitan. Sin embargo, AD DS y AD FS requieren la resolución de nombres DNS.

El proceso de configuración de DNS para admitir AD FS varía en función de si:

-   La organización ya tiene una infraestructura DNS. En la mayoría de los casos, DNS ya está configurado en la red para que los clientes de explorador web de la red corporativa tengan acceso a Internet. Dado que el acceso a Internet y la resolución de nombres son requisitos de AD FS, se supone que esta infraestructura está en vigor para la implementación de AD FS.

-   Deseas agregar un servidor federado a la red corporativa. Para autenticar a los usuarios de la red corporativa, los servidores DNS internos del bosque de la red corporativa deben estar configurados para devolver el CNAME del servidor interno que ejecuta el servicio de federación. Para obtener más información, consulta [Requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).

-   Deseas agregar un servidor proxy federado a la red perimetral. Si desea autenticar las cuentas de usuario que se encuentran en la red corporativa de la organización del asociado de identidad, los servidores DNS internos del bosque de la red corporativa deben estar configurados para devolver el CNAME del servidor proxy de Federación interno. Para obtener información sobre cómo configurar DNS para dar cabida a la adición de servidores proxy de Federación, consulta [requisitos de resolución de nombres para servidores proxy de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).

-   Estás configurando DNS para un entorno de laboratorio de pruebas. Si planea usar AD FS en un entorno de laboratorio de pruebas en el que ningún servidor DNS de raíz única es autoritativo, es probable que tenga que configurar reenviadores DNS para que las consultas de nombres entre dos o más bosques se reenvíen adecuadamente. Para obtener información general sobre cómo configurar un entorno de laboratorio de prueba de AD FS, consulte [AD FS guía paso a paso y procedimientos](https://go.microsoft.com/fwlink/?LinkId=180357).

## <a name="attribute-store-requirements"></a>Requisitos de almacenes de atributos
AD FS requiere que se use al menos un almacén de atributos para autenticar a los usuarios y extraer notificaciones de seguridad para esos usuarios. Para ver una lista de los almacenes de atributos compatibles con AD FS, consulta [La función de los almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) en la guía de diseño de AD FS.

> [!NOTE]
> AD FS crea automáticamente un almacén de atributos de Active Directory de manera predeterminada.

Los requisitos de los almacenes de atributos dependen de si la organización actúa como asociado de cuenta (hospeda a los usuarios federados) o como asociado de recurso (hospeda la aplicación federada).

### <a name="adds"></a>AD DS
Para que AD FS funcione correctamente, los controladores de dominio de la organización del asociado de cuenta o de la organización del asociado de recurso deben ejecutar Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 o Windows Server 2012.

Si AD FS está instalado y configurado en un equipo unido a un dominio, el almacén de la cuenta de usuario de Active Directory de dicho dominio está disponible como almacén de atributos seleccionable.

> [!IMPORTANT]
> Como AD FS requiere la instalación de Internet Information Services (IIS), se recomienda no instalar el software AD FS en un controlador de dominio en un entorno de producción por motivos de seguridad. Sin embargo, esta configuración es compatible con el servicio de soporte y atención al cliente de Microsoft.

#### <a name="schema-requirements"></a>Requisitos de esquema
AD FS no requiere ningún cambio de esquema ni modificaciones en el nivel funcional de AD DS.

#### <a name="functional-level-requirements"></a>Requisitos de niveles funcionales
La mayoría de las características de AD FS no requieren modificaciones de niveles funcionales de AD DS para funcionar correctamente. Sin embargo, se requiere un nivel funcional de dominio de Windows Server 2008 o superior para que la autenticación del certificado de cliente funcione correctamente, si dicho certificado está asignado explícitamente a una cuenta de usuario en AD DS.

#### <a name="service-account-requirements"></a>Requisitos de cuentas de servicio
Si estás creando una granja de servidores de federación, primero debes crear una cuenta de servicio específica basada en dominio en AD DS que pueda utilizar el servicio de federación. Más adelante, configurarás cada servidor de federación de la granja para que use dicha cuenta. Para obtener más información sobre cómo hacerlo, consulta [Configurar manualmente una cuenta de servicio para una granja de servidores de federación](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) en la guía de implementación de AD FS.

### <a name="ldap"></a>LDAP
Cuando trabajes con otros almacenes de atributos basados en el Protocolo ligero de acceso a directorios (LDAP), debes conectarte a un servidor LDAP que sea compatible con la autenticación integrada de Windows. Además, la cadena de conexión de LDAP debe estar escrita con el formato de una dirección URL de LDAP, tal y como se describe en RFC 2255.

### <a name="sql-server"></a>SQL Server
Para que AD FS funcione correctamente, los equipos que hospedan el almacén de atributos de Lenguaje de consulta estructurado (SQL) Server deben ejecutar Microsoft SQL Server 2005 o SQL Server 2008. Si trabajas con almacenes de atributos basados en SQL, también debes configurar una cadena de conexión.

### <a name="custom-attribute-stores"></a>Almacenes de atributos personalizados
Puedes desarrollar almacenes de atributos personalizados para habilitar escenarios avanzados. El lenguaje de directiva integrado en AD FS puede hacer referencia a almacenes de atributos personalizados, con el fin de permitir una mejora en los siguientes escenarios:

-   Crear notificaciones para un usuario autenticado localmente

-   Complementar notificaciones para un usuario autenticado externamente

-   Autorizar a un usuario a obtener un token

-   Autorizar a un servicio a obtener un token del comportamiento de un usuario

Si trabajas con un almacén de atributos personalizado, también podrías tener que configurar una cadena de conexión. En este caso, escribe un código personalizado de tu gusto que permita la conexión a tu almacén de atributos personalizado. La cadena de conexión en este caso es un conjunto de pares nombre-valor que el desarrollador del almacén de atributos personalizados interpreta como implementados.

Para obtener más información sobre el desarrollo y el uso de almacenes de atributos personalizados, vea [información general](https://go.microsoft.com/fwlink/?LinkId=190782)sobre el almacén de atributos.

## <a name="application-requirements"></a>Requisitos de las aplicaciones
Los servidores de federación pueden proteger y comunicarse con aplicaciones de federación, como, por ejemplo, aplicaciones compatibles con notificaciones.

## <a name="authentication-requirements"></a>Requisitos de autenticación
AD FS se integra de forma natural con la autenticación de Windows existente, por ejemplo, la autenticación Kerberos, NTLM, tarjetas inteligentes y certificados X. 509 V3 del lado cliente. Los servidores de federación usan la autenticación Kerberos estándar para autenticar a un usuario con un dominio. Los clientes pueden autenticarse usando la autenticación basada en formularios, la autenticación mediante tarjeta inteligente y la autenticación integrada de Windows, en función de cómo configures la autenticación.

El rol de servidor proxy de federación de AD FS también permite que el usuario se autentique externamente mediante la autenticación de clientes SSL. También puedes configurar el rol de servidor de federación para que solicite la autenticación de clientes SSL, aunque por lo general se proporciona una mejor experiencia de usuario si se configura el servidor de federación de la cuenta para la autenticación integrada de Windows. En este caso, AD FS no controla qué credenciales utiliza el usuario para el inicio de sesión en el escritorio de Windows.

### <a name="smart-card-logon"></a>Inicio de sesión con tarjeta inteligente
Aunque AD FS puede aplicar el tipo de credenciales que usa para la autenticación (contraseñas, autenticación de cliente SSL o autenticación integrada de Windows), no aplica directamente la autenticación con tarjetas inteligentes. Por lo tanto, AD FS no proporciona una interfaz de usuario (UI) del lado cliente para obtener las credenciales del número de identificación personal (PIN) de la tarjeta inteligente. Esto se debe a que los clientes basados en Windows no proporcionan datos de credenciales de usuario intencionadamente a servidores de Federación o servidores Web.

### <a name="smart-card-authentication"></a>Autenticación con tarjeta inteligente
La autenticación mediante tarjeta inteligente usa el protocolo Kerberos para autenticarse en un servidor de Federación de cuentas. No se puede extender AD FS para agregar nuevos métodos de autenticación. El certificado de la tarjeta inteligente no es necesario para encadenarse a una raíz de confianza en el equipo cliente. El uso de un certificado basado en tarjeta inteligente con AD FS requiere las siguientes condiciones:

-   El lector y el proveedor de servicios de cifrado (CSP) de la tarjeta inteligente deben funcionar en el equipo donde está ubicado el explorador.

-   El certificado de la tarjeta inteligente debe encadenarse a una raíz de confianza en el servidor de Federación de la cuenta y el servidor proxy de Federación de la cuenta.

-   El certificado debe asignarse a la cuenta de usuario de AD DS mediante uno de los siguientes métodos:

    -   El nombre del firmante del certificado se corresponde con el nombre distintivo de LDAP de una cuenta de usuario en AD DS.

    -   La extensión NombreAlt del firmante del certificado tiene el nombre principal de usuario (UPN) de una cuenta de usuario en AD DS.

Para que sea compatible con ciertos requisitos de seguridad de autenticación en determinados casos, es posible configurar AD FS para crear una notificación que indique cómo se ha autenticado el usuario. Un usuario autenticado puede utilizar dicha notificación para decidir si permite la autorización.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)