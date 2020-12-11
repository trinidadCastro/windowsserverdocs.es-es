---
description: 'Más información acerca de: requisitos de AD FS para Windows Server'
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Requisitos de AD FS para Windows Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 03b6d03910b3d9cef0c886801e2018c62b0f04b3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043293"
---
# <a name="ad-fs-requirements-for-windows-server"></a>Requisitos de AD FS para Windows Server

A continuación se indican los diversos requisitos que se deben cumplir al implementar AD FS:

- [Requisitos de certificado](AD-FS-Requirements.md#BKMK_1)

- [Requisitos de hardware](AD-FS-Requirements.md#BKMK_2)

- [Requisitos de software](AD-FS-Requirements.md#BKMK_3)

- [Requisitos de AD DS](AD-FS-Requirements.md#BKMK_4)

- [Requisitos de bases de datos de configuración](AD-FS-Requirements.md#BKMK_5)

- [Requisitos de exploradores](AD-FS-Requirements.md#BKMK_6)

- [Requisitos de extranet](AD-FS-Requirements.md#BKMK_extranet)

- [Requisitos de red](AD-FS-Requirements.md#BKMK_7)

- [Requisitos de almacenes de atributos](AD-FS-Requirements.md#BKMK_8)

- [Requisitos de las aplicaciones](AD-FS-Requirements.md#BKMK_9)

- [Requisitos de autenticación](AD-FS-Requirements.md#BKMK_10)

- [Requisitos de la Unión al área de trabajo](AD-FS-Requirements.md#BKMK_11)

- [Requisitos de criptografía](AD-FS-Requirements.md#BKMK_12)

- [Requisitos de permisos](AD-FS-Requirements.md#BKMK_13)

## <a name="certificate-requirements"></a><a name="BKMK_1"></a>Requisitos de certificados
Los certificados desempeñan el rol más importante en la protección de las comunicaciones entre los servidores de Federación, los servidores proxy de aplicación Web, las aplicaciones compatibles con notificaciones y los clientes Web. Los requisitos de los certificados varían en función de si está configurando un servidor de Federación o un equipo proxy, como se describe en esta sección.

**Certificados del servidor de federación**

| **Tipo de certificado** | **Requisitos, compatibilidad & aspectos que se deben conocer** |
|--|--|
| **Certificado de capa de sockets seguros (SSL):** Se trata de un certificado SSL estándar que se usa para proteger las comunicaciones entre los servidores de Federación y los clientes. | -Este certificado debe ser un certificado * X509 V3 de confianza pública.<br>-Todos los clientes que tienen acceso a cualquier extremo de AD FS deben confiar en este certificado. Se recomienda encarecidamente usar certificados emitidos por una entidad de certificación (CA) pública (de terceros). Puede usar un certificado SSL autofirmado correctamente en los servidores de Federación en un entorno de laboratorio de pruebas. Sin embargo, para un entorno de producción, se recomienda obtener el certificado de una CA pública.<br>-Admite cualquier tamaño de clave compatible con Windows Server 2012 R2 para los certificados SSL.<br>-No admite certificados que usan claves CNG.<br>-Cuando se usa junto con Workplace Join/servicio de registro de dispositivos, el nombre alternativo del sujeto del certificado SSL para el servicio AD FS debe contener el valor enterpriseregistration seguido del sufijo de nombre principal de usuario (UPN) de su organización, por ejemplo, enterpriseregistration.contoso.com.<br>-Se admiten los certificados comodín. Al crear la granja de AD FS, se le pedirá que proporcione el nombre del servicio AD FS (por ejemplo, **ADFS.contoso.com**.<br>-Se recomienda encarecidamente utilizar el mismo certificado SSL para el proxy de aplicación Web. Sin embargo, esto es **necesario** para que sea el mismo cuando se admiten puntos de conexión de autenticación integrada de Windows a través del proxy de aplicación web y cuando se activa la autenticación de protección ampliada (configuración predeterminada).<br>-El nombre de sujeto de este certificado se usa para representar el nombre de Servicio de federación para cada instancia de AD FS que implemente. Por esta razón, tal vez te interese elegir un nombre de firmante en los certificados nuevos emitidos por CA que a los asociados les resulte más representativo del nombre de tu empresa u organización.<br> La identidad del certificado debe coincidir con el nombre del servicio de Federación (por ejemplo, fs.contoso.com). La identidad es una extensión de nombre alternativo del firmante del tipo dNSName o, si no hay ninguna entrada de nombre alternativo del firmante, el nombre de sujeto especificado como nombre común. Pueden existir varias entradas de nombre alternativo del sujeto en el certificado, siempre que una de ellas coincida con el nombre del servicio de Federación.<br>- **Importante:** se recomienda encarecidamente usar el mismo certificado SSL en todos los nodos de la granja de AD FS, así como todos los proxies de aplicación Web de la granja de AD FS. |
| **Certificado de comunicación de servicio:** Este certificado habilita la seguridad de mensajes WCF para proteger las comunicaciones entre los servidores de Federación. | : De forma predeterminada, el certificado SSL se usa como el certificado de comunicaciones de servicio.  Pero también tiene la opción de configurar otro certificado como certificado de comunicación de servicio.<br>- **Importante:** si utiliza el certificado SSL como certificado de comunicación de servicio, cuando expire el certificado SSL, asegúrese de configurar el certificado SSL renovado como certificado de comunicación de servicio. Esto no se produce automáticamente.<br>-Este certificado debe ser de confianza para los clientes de AD FS que utilizan la seguridad de mensajes de WCF.<br>-Se recomienda usar un certificado de autenticación de servidor emitido por una entidad de certificación (CA) pública (de terceros).<br>-El certificado de comunicación de servicio no puede ser un certificado que usa claves CNG.<br>: Este certificado se puede administrar mediante la consola de administración de AD FS. |
| **Certificado de firma de tokens:** Se trata de un certificado X509 estándar que se usa para firmar de forma segura todos los tokens que emite el servidor de Federación. | : De forma predeterminada, AD FS crea un certificado autofirmado con claves de 2048 bits.<br>-También se admiten certificados emitidos por CA y se pueden cambiar con el complemento Administración de AD FS.<br>-Los certificados emitidos por la CA deben almacenarse & tener acceso a través de un proveedor de cifrado de CSP.<br>-El certificado de firma de tokens no puede ser un certificado que usa claves CNG.<br>-AD FS no requiere certificados inscritos externamente para la firma de tokens.<br> AD FS renueva automáticamente estos certificados autofirmados antes de que expiren, primero configure los nuevos certificados como certificados secundarios para permitir que los asociados los consuman y, a continuación, se invierten a principal en un proceso denominado sustitución automática de certificados. Se recomienda usar los certificados generados automáticamente de forma predeterminada para la firma de tokens.<br> Si su organización tiene directivas que requieren que se configuren certificados diferentes para la firma de tokens, puede especificar los certificados en el momento de la instalación mediante PowerShell (use el parámetro – SigningCertificateThumbprint del cmdlet Install-AdfsFarm).  Después de la instalación, puede ver y administrar certificados de firma de tokens con la consola de administración de AD FS o los cmdlets de PowerShell Set-AdfsCertificate y Get-AdfsCertificate.<br> Cuando los certificados inscritos externamente se usan para la firma de tokens, AD FS no realiza la renovación o sustitución automática de certificados.  Este proceso lo debe realizar un administrador.<br> Para permitir la sustitución de certificados cuando un certificado está a la expiración, puede configurarse un certificado de firma de tokens secundario en AD FS. De forma predeterminada, todos los certificados de firma de tokens se publican en los metadatos de Federación, pero el certificado de firma de tokens principal lo usa AD FS para firmar los tokens. |
| **Certificado de descifrado o cifrado de tokens:** Se trata de un certificado X509 estándar que se usa para descifrar o cifrar los tokens entrantes. También se publica en los metadatos de federación. | : De forma predeterminada, AD FS crea un certificado autofirmado con claves de 2048 bits.<br>-También se admiten certificados emitidos por CA y se pueden cambiar con el complemento Administración de AD FS.<br>-Los certificados emitidos por la CA deben almacenarse & tener acceso a través de un proveedor de cifrado de CSP.<br>-El certificado de cifrado o descifrado de tokens no puede ser un certificado que use claves CNG.<br>: De forma predeterminada, AD FS genera y usa sus propios certificados autofirmados y generados internamente para el descifrado de tokens.  AD FS no requiere certificados inscritos externamente para este fin.<br> Además, AD FS renueva automáticamente estos certificados autofirmados antes de que expiren.<br> **Se recomienda usar los certificados generados automáticamente de forma predeterminada para el descifrado de tokens.**<br> Si su organización tiene directivas que requieren que se configuren certificados diferentes para el descifrado de tokens, puede especificar los certificados en el momento de la instalación mediante PowerShell (use el parámetro – DecryptionCertificateThumbprint del cmdlet Install-AdfsFarm).  Después de la instalación, puede ver y administrar los certificados de descifrado de tokens con la consola de administración de AD FS o los cmdlets de PowerShell Set-AdfsCertificate y Get-AdfsCertificate.<br> **Cuando los certificados inscritos externamente se usan para descifrar el token, AD FS no realiza la renovación automática del certificado.  Este proceso lo debe realizar un administrador**.<br>-La cuenta de servicio de AD FS debe tener acceso a la clave privada del certificado de firma de tokens en el almacén personal del equipo local. Esto se encarga del programa de instalación. También puede usar el complemento Administración de AD FS para garantizar este acceso si posteriormente cambia el certificado de firma de tokens. |

> [!CAUTION]
> Los certificados que se usan para la firma de tokens y el cifrado y descifrado de tokens son fundamentales para la estabilidad del Servicio de federación. Los clientes que administran su propia firma de tokens & los certificados de descifrado y cifrado de tokens deben asegurarse de que se realice una copia de seguridad de estos certificados y estén disponibles de forma independiente durante un evento de recuperación.

> [!NOTE]
> En AD FS puede cambiar el nivel del algoritmo hash seguro (SHA) que se usa para las firmas digitales a SHA-1 o SHA-256 (más seguro). AD FS no admite el uso de certificados con otros métodos hash, como MD5 (el algoritmo hash predeterminado que se usa con la herramienta de línea de comandos Makecert.exe). Para tu seguridad, te recomendamos que utilices SHA-256 (establecido de manera predeterminada) para todas las firmas. Se recomienda el uso de SHA-1 solo en los escenarios en los que se debe interoperar con un producto que no admite las comunicaciones que usan SHA-256, como un producto que no es de Microsoft o versiones heredadas de AD FS.

> [!NOTE]
> Después de recibir un certificado de una CA, asegúrate de que todos los certificados se importen al almacén de certificados personales del equipo local. Puedes importar certificados al almacén personal con el complemento Certificados de MMC.

## <a name="hardware-requirements"></a><a name="BKMK_2"></a>Requisitos de hardware
Los siguientes requisitos de hardware mínimos y recomendados se aplican a los servidores de Federación de AD FS en Windows Server 2012 R2:

|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|--|--|--|
|Velocidad de CPU|Procesador de 64 bits a 1,4 GHz|Cuatro núcleos, 2 GHz|
|RAM|512 MB|4 GB|
|Espacio en disco|32 GB|100 GB|

## <a name="software-requirements"></a><a name="BKMK_3"></a>Requisitos de software

Los siguientes requisitos de AD FS son para la funcionalidad de servidor integrada en el &reg; sistema operativo Windows server 2012 R2:

- Para el acceso de extranet, debe implementar el servicio de rol proxy de aplicación Web, que forma parte del rol de servidor de acceso remoto de Windows Server &reg; 2012 R2. Las versiones anteriores de un servidor proxy de Federación no se admiten con AD FS en Windows Server &reg; 2012 R2.

- No se puede instalar un servidor de federación y el servicio de rol de Proxy de aplicación web en el mismo equipo.

## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>Requisitos de AD DS

**Requisitos del controlador de dominio**

Los controladores de dominio de todos los dominios de usuario y el dominio al que se unen los servidores AD FS deben ejecutar Windows Server 2008 o posterior.

> [!NOTE]
> Toda la compatibilidad con entornos con controladores de dominio de Windows Server 2003 finalizará después de la fecha de finalización de soporte extendido de Windows Server 2003. Se recomienda encarecidamente a los clientes que actualicen los controladores de dominio lo antes posible. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener información adicional sobre el ciclo de vida de soporte técnico de Microsoft. En el caso de los problemas detectados que son específicos de los entornos de controlador de dominio de Windows Server 2003, solo se emitirán correcciones para los problemas de seguridad y si se puede emitir una corrección antes de la expiración del soporte extendido para Windows Server 2003.

>[!NOTE]
> AD FS requiere que un controlador de dominio de escritura completo funcione en lugar de un controlador de dominio de Read-Only. Si una topología planeada incluye un Read-Only controlador de dominio, el controlador de dominio de Read-Only se puede usar para la autenticación, pero el procesamiento de notificaciones LDAP requerirá una conexión al controlador de dominio de escritura.

**Requisitos de nivel funcional de dominio**

Todos los dominios de cuentas de usuario y el dominio al cual se unen los servidores de AD FS deben funcionar en el nivel funcional de dominio de Windows Server 2003 o posterior.

La mayoría de las características de AD FS no requieren modificaciones de niveles funcionales de AD DS para funcionar correctamente. Sin embargo, se requiere un nivel funcional de dominio de Windows Server 2008 o superior para que la autenticación del certificado de cliente funcione correctamente, si dicho certificado está asignado explícitamente a una cuenta de usuario en AD DS.

**Requisitos de esquemas**

- AD FS no requiere ningún cambio de esquema ni modificaciones en el nivel funcional de AD DS.

- Para usar la funcionalidad de Workplace Join, el esquema del bosque al que se unen AD FS servidores se debe establecer en Windows Server 2012 R2.

**Requisitos de cuentas de servicio**

- Se puede usar cualquier cuenta de servicio estándar como cuenta de servicio para AD FS. También se admiten cuentas de servicio administradas de grupo. Esto requiere al menos un controlador de dominio (se recomienda implementar dos o más) que ejecute Windows Server 2012 o posterior.

- Para que la autenticación Kerberos funcione entre clientes Unidos a un dominio y AD FS, el "HOST/<adfs_service_name>" se debe registrar como un SPN en la cuenta de servicio. De forma predeterminada, AD FS lo configurará al crear una nueva granja de AD FS si tiene permisos suficientes para realizar esta operación.

- La cuenta de servicio de AD FS debe ser de confianza en todos los dominios de usuario que contengan usuarios que se autentiquen en el servicio AD FS.

**Requisitos de dominio**

- Todos los servidores de AD FS deben estar unidos a un dominio AD DS.

- Todos los servidores de AD FS de una granja de servidores se deben implementar en un único dominio.

- El dominio al que se unen los servidores AD FS debe confiar en todos los dominios de cuentas de usuario que contengan usuarios que se autentiquen en el servicio AD FS.

**Requisitos de varios bosques**

- El dominio al que se unen los servidores AD FS debe confiar en todos los dominios o bosques de cuentas de usuario que contengan usuarios que se autentiquen en el servicio AD FS.

- La cuenta de servicio de AD FS debe ser de confianza en todos los dominios de usuario que contengan usuarios que se autentiquen en el servicio AD FS.

## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>Requisitos de bases de datos de configuración
A continuación se indican los requisitos y las restricciones que se aplican en función del tipo de almacén de configuración:

**WID**

- Una granja WID tiene un límite de 30 servidores de Federación si tiene 100 o menos relaciones de confianza para usuario autenticado.

- No se admite el perfil de resolución de artefactos en SAML 2,0 en la base de datos de configuración de WID.  No se admite la detección de reproducción de tokens en la base de datos de configuración de WID. Esta funcionalidad solo se usa en escenarios donde AD FS actúa como proveedor de Federación y consume tokens de seguridad de proveedores de notificaciones externos.

- Se admite la implementación de servidores de AD FS en centros de datos distintos para la conmutación por error o el equilibrio de carga geográfica, siempre que el número de servidores no supere los 30.

En la tabla siguiente se proporciona un resumen del uso de una granja de servidores WID.  Úselo para planear la implementación.

| 1-100 relaciones de confianza para usuario autenticado | Más de 100 relaciones de confianza para usuario autenticado |
|--|--|
| **1-30 nodos de AD FS:** Compatible con WID | **1-30 nodos de AD FS:** No compatible con WID, se requiere SQL |
| **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL | **Más de 30 nodos de AD FS:** No compatible con WID, se requiere SQL |

**SQL Server**

Por AD FS en Windows Server 2012 R2, puede usar SQL Server 2008 y versiones posteriores.

## <a name="browser-requirements"></a><a name="BKMK_6"></a>Requisitos de exploradores
Cuando la autenticación de AD FS se hace a través de un explorador o un control de explorador, el explorador tiene que cumplir los siguientes requisitos:

- JavaScript debe estar habilitado.

- Las cookies deben estar activadas

- Se debe admitir Indicación de nombre de servidor (SNI)

- Para el certificado de usuario & la autenticación de certificado de dispositivo (funcionalidad de unión al área de trabajo), el explorador debe ser compatible con la autenticación de certificados de cliente SSL

Varios exploradores y plataformas clave se han sometido a la validación de la representación y la funcionalidad que se enumeran a continuación. Todavía se admiten los exploradores y dispositivos que no se cubren en esta tabla si cumplen los requisitos mencionados anteriormente:

| **Exploradores** | **Plataformas** |
|--|--|
| IE 10,0 | Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 |
| IE 11,0 | Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 |
| Agente de autenticación Web de Windows | Windows 8.1 |
| Firefox [V21] | Windows 7, Windows 8.1 |
| Safari [V7] | iOS 6, Mac OS-X 10,7 |
| Chrome [V27] | Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2 Mac OS-X 10,7 |

> [!IMPORTANT]
> Problema conocido: Firefox: Workplace Join funcionalidad que identifica el dispositivo con el certificado de dispositivo no funciona en las plataformas Windows. Firefox no admite actualmente la autenticación de certificados de cliente SSL mediante certificados aprovisionados en el almacén de certificados de usuario en los clientes Windows.

**Cookies**

AD FS crea cookies persistentes y basadas en sesión que deben almacenarse en equipos cliente para proporcionar funcionalidades de inicio de sesión, cierre de sesión, inicio de sesión único (SSO) y demás. Por lo tanto, el explorador cliente debe estar configurado para aceptar cookies. Las cookies usadas para la autenticación son siempre cookies de sesión de protocolo seguro de transferencia de hipertexto (HTTPS) escritas para el servidor de origen. Si el explorador cliente no está configurado para permitir estas cookies, AD FS no funcionará correctamente. Las cookies persistentes se usan para conservar la selección del usuario del proveedor de notificaciones. Puedes deshabilitarlas a través de una opción del archivo de configuración para las páginas de inicio de sesión de AD FS. Es necesaria la compatibilidad con TLS/SSL por motivos de seguridad.

## <a name="extranet-requirements"></a><a name="BKMK_extranet"></a>Requisitos de extranet
Para proporcionar acceso a la extranet al servicio AD FS, debe implementar el servicio de rol de proxy de aplicación web como el rol orientado a extranet que pone en proxy las solicitudes de autenticación de forma segura para el servicio AD FS. Esto proporciona aislamiento de los puntos de conexión del servicio AD FS, así como el aislamiento de todas las claves de seguridad (como los certificados de firma de tokens) de las solicitudes que se originan en Internet. Además, características como el bloqueo de la cuenta de extranet de software requieren el uso del proxy de aplicación Web. Para obtener más información sobre el proxy de aplicación Web, vea [proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn584107(v=ws.11)).

Si desea usar un proxy de terceros para el acceso a Extranet, este proxy de terceros debe admitir el protocolo definido en [http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf) .

## <a name="network-requirements"></a><a name="BKMK_7"></a>Requisitos de red
La configuración adecuada de los siguientes servicios de red es fundamental para la correcta implementación de AD FS en la organización:

**Configuración del Firewall corporativo**

Tanto el firewall que se encuentra entre el Proxy de aplicación web y la granja de servidores de federación como el firewall entre los clientes y el Proxy de aplicación web deben tener el puerto 443 TCP habilitado para la entrada.

Además, si se requiere la autenticación de certificados de usuario de cliente (autenticación clientTLS mediante certificados de usuario X509), AD FS en Windows Server 2012 R2 requiere que el puerto TCP 49443 esté habilitado en el Firewall entre los clientes y el proxy de aplicación Web. Esto no es necesario en el Firewall entre el proxy de aplicación web y los servidores de Federación.

> [!NOTE]
> Asegúrese también de que el puerto 49443 no lo use ningún otro servicio en el servidor proxy de aplicación web y AD FS.

**Configuring DNS** (Configuración de DNS)

- Para el acceso a la intranet, todos los clientes que tienen acceso a AD FS servicio dentro de la red corporativa interna (Intranet) deben poder resolver el nombre del servicio de AD FS (nombre proporcionado por el certificado SSL) en el equilibrador de carga para los servidores de AD FS o el servidor de AD FS.

- Para el acceso de extranet, todos los clientes que tienen acceso a AD FS servicio desde fuera de la red corporativa (extranet/Internet) deben poder resolver el nombre del servicio de AD FS (nombre proporcionado por el certificado SSL) en el equilibrador de carga para los servidores proxy de aplicación web o el servidor proxy de aplicación Web.

- Para que el acceso de extranet funcione correctamente, cada servidor proxy de aplicación Web de la red perimetral debe ser capaz de resolver AD FS nombre de servicio (nombre proporcionado por el certificado SSL) en el equilibrador de carga para los servidores de AD FS o el servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo en la red DMZ o cambiando la resolución del servidor local mediante el archivo de hosts.

- Para que la autenticación integrada de Windows funcione dentro de la red y fuera de la red para un subconjunto de puntos de conexión expuestos a través del proxy de aplicación Web, debe usar un registro A (no CNAME) para apuntar a los equilibradores de carga.

Para obtener información sobre cómo configurar el DNS corporativo para el servicio de Federación y el servicio de registro de dispositivos, consulte [configuración de DNS corporativo para el servicio de Federación y DRS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486786(v=ws.11)).

Para obtener información sobre la configuración de DNS corporativo para proxies de aplicación Web, consulte la sección "configuración de DNS" en [paso 1: configurar la infraestructura del proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11)).

Para obtener información sobre cómo configurar una dirección IP de clúster o un FQDN de clúster con NLB, vea especificar los parámetros de clúster en [http://go.microsoft.com/fwlink/?LinkId=75282](https://go.microsoft.com/fwlink/?LinkId=75282) .

## <a name="attribute-store-requirements"></a><a name="BKMK_8"></a>Requisitos de almacenes de atributos
AD FS requiere que se use al menos un almacén de atributos para autenticar a los usuarios y extraer notificaciones de seguridad para esos usuarios. Para obtener una lista de almacenes de atributos que AD FS admite, vea [el rol de almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).

> [!NOTE]
> De forma predeterminada AD FS crea automáticamente un almacén de atributos "Active Directory". Los requisitos de los almacenes de atributos dependen de si la organización actúa como asociado de cuenta (hospeda a los usuarios federados) o como asociado de recurso (hospeda la aplicación federada).

**Almacenes de atributos LDAP**

Cuando trabajes con otros almacenes de atributos basados en el Protocolo ligero de acceso a directorios (LDAP), debes conectarte a un servidor LDAP que sea compatible con la autenticación integrada de Windows. Además, la cadena de conexión de LDAP debe estar escrita con el formato de una dirección URL de LDAP, tal y como se describe en RFC 2255.

También es necesario que la cuenta de servicio para el servicio de AD FS tenga derecho a recuperar información de usuario en el almacén de atributos LDAP.

**SQL Server almacenes de atributos**

Para que AD FS en Windows Server 2012 R2 funcione correctamente, los equipos que hospedan el almacén de atributos de SQL Server deben ejecutar Microsoft SQL Server 2008 o posterior. Si trabajas con almacenes de atributos basados en SQL, también debes configurar una cadena de conexión.

**Almacenes de atributos personalizados**

Puedes desarrollar almacenes de atributos personalizados para habilitar escenarios avanzados.

- El lenguaje de directiva integrado en AD FS puede hacer referencia a almacenes de atributos personalizados, con el fin de permitir una mejora en los siguientes escenarios:

    - Crear notificaciones para un usuario autenticado localmente

    - Complementar notificaciones para un usuario autenticado externamente

    - Autorizar a un usuario a obtener un token

    - Autorizar a un servicio a obtener un token del comportamiento de un usuario

    - Emisión de datos adicionales en los tokens de seguridad emitidos por AD FS a los usuarios de confianza.

- Todos los almacenes de atributos personalizados se deben basar en .NET 4,0 o superior.

Al trabajar con un almacén de atributos personalizado, es posible que también tenga que configurar una cadena de conexión. En ese caso, puede escribir un código personalizado de su elección que permita una conexión al almacén de atributos personalizado. La cadena de conexión en esta situación es un conjunto de pares de nombre/valor que se interpretan como implementados por el desarrollador del almacén de atributos personalizado. Para obtener más información sobre el desarrollo y el uso de almacenes de atributos personalizados, vea [información general](https://go.microsoft.com/fwlink/?LinkId=190782)sobre el almacén de atributos.

## <a name="application-requirements"></a><a name="BKMK_9"></a>Requisitos de las aplicaciones
AD FS admite aplicaciones compatibles con notificaciones que utilizan los protocolos siguientes:

- El certificado del proveedor de identidades de WS-Federation

- WS-Trust

- Protocolo SAML 2,0 con IDPLite, SPLit & perfiles eGov 1.5.

- Perfil de concesión de autorización de OAuth 2,0

AD FS también admite la autenticación y autorización para las aplicaciones no compatibles con notificaciones que son compatibles con el proxy de aplicación Web.

## <a name="authentication-requirements"></a><a name="BKMK_10"></a>Requisitos de autenticación
**Autenticación AD DS (autenticación principal)**

Para el acceso a la intranet, se admiten los siguientes mecanismos de autenticación estándar para AD DS:

- Autenticación integrada de Windows con Negotiate para Kerberos & NTLM

- Autenticación de formularios mediante el nombre de usuario y contraseñas

- Autenticación de certificados mediante certificados asignados a cuentas de usuario en AD DS

Para el acceso a la extranet, se admiten los siguientes mecanismos de autenticación:

- Autenticación de formularios mediante el nombre de usuario y contraseñas

- Autenticación de certificados mediante certificados asignados a cuentas de usuario en AD DS

- Autenticación integrada de Windows mediante Negotiate (solo NTLM) para los puntos de conexión de WS-Trust que aceptan la autenticación integrada de Windows.

Para la autenticación de certificados:

- Se extiende a las tarjetas inteligentes que pueden estar protegidas mediante PIN.

- La GUI para que el usuario escriba su PIN no es proporcionada por AD FS y es necesario que forme parte del sistema operativo del cliente que se muestra al usar el cliente TLS.

- El lector y el proveedor de servicios de cifrado (CSP) de la tarjeta inteligente deben funcionar en el equipo donde está ubicado el explorador.

- El certificado de la tarjeta inteligente debe encadenarse a una raíz de confianza en todos los servidores de AD FS y servidores proxy de aplicación Web.

- El certificado debe asignarse a la cuenta de usuario de AD DS mediante uno de los siguientes métodos:

    - El nombre del firmante del certificado se corresponde con el nombre distintivo de LDAP de una cuenta de usuario en AD DS.

    - La extensión NombreAlt del firmante del certificado tiene el nombre principal de usuario (UPN) de una cuenta de usuario en AD DS.

Para la autenticación integrada de Windows con Kerberos en la intranet,

- Es necesario que el nombre del servicio forme parte de los sitios de confianza o de los sitios de la Intranet local.

- Además, el adfs_service_name de HOST/<> SPN debe establecerse en la cuenta de servicio en la que se ejecuta la granja de AD FS.

**Multi-Factor Authentication**

AD FS admite la autenticación adicional (más allá de la autenticación principal que admite AD DS) mediante un modelo de proveedor que permite a los proveedores o clientes crear su propio adaptador de multi-factor Authentication que un administrador puede registrar y usar durante el inicio de sesión.

Cada adaptador de MFA debe basarse en .NET 4,5.

Para obtener más información sobre MFA, consulte [Administración de riesgos con multi-factor Authentication adicionales para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

**Autenticación de dispositivos**

AD FS admite la autenticación de dispositivos mediante certificados aprovisionados por el servicio de registro de dispositivos durante la acción de un área de trabajo del usuario final que se une a su dispositivo.

## <a name="workplace-join-requirements"></a><a name="BKMK_11"></a>Requisitos de la Unión al área de trabajo
Los usuarios finales pueden unir sus dispositivos a una organización mediante AD FS. Esto es compatible con el servicio de registro de dispositivos en AD FS. Como resultado, los usuarios finales obtienen las ventajas adicionales del inicio de sesión único en las aplicaciones compatibles con AD FS. Además, los administradores pueden administrar el riesgo restringiendo el acceso a las aplicaciones solo a los dispositivos que se han unido al área de trabajo de la organización. A continuación se muestran los siguientes requisitos para habilitar este escenario.

- AD FS admite la Unión al área de trabajo para dispositivos Windows 8.1 y iOS 5 +

- Para usar la funcionalidad de Workplace Join, el esquema del bosque al que se unen los servidores AD FS debe ser Windows Server 2012 R2.

- El nombre alternativo del sujeto del certificado SSL para AD FS servicio debe contener el valor enterpriseregistration seguido del sufijo de nombre principal de usuario (UPN) de su organización, por ejemplo, enterpriseregistration.corp.contoso.com.

## <a name="cryptography-requirements"></a><a name="BKMK_12"></a>Requisitos de criptografía
En la tabla siguiente se proporciona información adicional sobre la compatibilidad con la criptografía en la AD FS la funcionalidad de firma de tokens, cifrado y descifrado de tokens:

|**Algoritmo**|**Longitudes de clave**|**Protocolos/aplicaciones/comentarios**|
|--|--|--|
|TripleDES: valor predeterminado 192 (compatible 192 – 256)- [http://www.w3.org/2001/04/xmlenc#tripledes-cbc](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>= 192|Algoritmo admitido para descifrar el token de seguridad. No se admite el cifrado del token de seguridad con este algoritmo.|
|AES128 [http://www.w3.org/2001/04/xmlenc#aes128-cbc](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|Algoritmo admitido para descifrar el token de seguridad. No se admite el cifrado del token de seguridad con este algoritmo.|
|AES192 [http://www.w3.org/2001/04/xmlenc#aes192-cbc](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|Algoritmo admitido para descifrar el token de seguridad. No se admite el cifrado del token de seguridad con este algoritmo.|
|AES256 [http://www.w3.org/2001/04/xmlenc#aes256-cbc](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**Default**. Algoritmo admitido para el cifrado del token de seguridad.|
|Predeterminado tripledeskeywrap [http://www.w3.org/2001/04/xmlenc#kw-tripledes](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|Todos los tamaños de clave admitidos por .NET 4.0 +|Algoritmo admitido para cifrar la clave simétrica que cifra un token de seguridad.|
|Predeterminado aes128keywrap [http://www.w3.org/2001/04/xmlenc#kw-aes128](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|Algoritmo admitido para cifrar la clave simétrica que cifra el token de seguridad.|
|Predeterminado aes192keywrap [http://www.w3.org/2001/04/xmlenc#kw-aes192](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|Algoritmo admitido para cifrar la clave simétrica que cifra el token de seguridad.|
|Predeterminado aes256keywrap [http://www.w3.org/2001/04/xmlenc#kw-aes256](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|Algoritmo admitido para cifrar la clave simétrica que cifra el token de seguridad.|
|RsaV15KeyWrap - [http://www.w3.org/2001/04/xmlenc#rsa-1_5](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|Algoritmo admitido para cifrar la clave simétrica que cifra el token de seguridad.|
|Predeterminado rsaoaepkeywrap [http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|Predeterminada. Algoritmo admitido para cifrar la clave simétrica que cifra el token de seguridad.|
|Ordenamiento[http://www.w3.org/PICS/DSig/SHA1_1_0.html](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N/D|Usado por AD FS Server en la generación de artefactos SourceId: en este escenario, el STS usa SHA1 (según la recomendación del estándar SAML 2,0) para crear un valor corto de 160 bits para el artefacto sourceiD.<p>También lo usa el agente Web de ADFS (componente heredado de WS2003) para identificar los cambios en un valor de hora de "última actualización", de modo que sepa cuándo actualizar la información del STS.|
|SHA1withRSA<p>[http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N/D|Se utiliza en casos en los que AD FS servidor valida la firma de AuthenticationRequest de SAML, firmar la solicitud de resolución de artefactos o la respuesta, crear un certificado de firma de tokens.<p>En estos casos, SHA256 es el valor predeterminado y SHA1 solo se usa si el asociado (usuario de confianza) no puede admitir SHA256 y debe usar SHA1.|

## <a name="permissions-requirements"></a><a name="BKMK_13"></a>Requisitos de permisos
El administrador que realiza la instalación y la configuración inicial de AD FS deben tener permisos de administrador de dominio en el dominio local (es decir, el dominio al que se une el servidor de Federación).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
