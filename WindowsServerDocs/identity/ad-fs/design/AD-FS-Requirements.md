---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: "Federación granja de servidores con SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS

>Se aplica a: Windows Server 2012 R2

Estos son los distintos requisitos que deben cumplir con durante la implementación de AD FS:  
  
-   [Requisitos de certificados](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisitos de software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisitos de la base de datos de configuración](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisitos del explorador](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisitos de extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisitos de la red](AD-FS-Requirements.md#BKMK_7)  
  
-   [Requisitos de la tienda de atributo](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisitos de la aplicación](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisitos de autenticación](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisitos de unión a un área de trabajo](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisitos de criptografía](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisitos de permisos](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificados  
Certificados de reproducción el rol más importante en proteger las comunicaciones entre servidores de federación, proxy de aplicación Web, aplicaciones con reconocimiento de claims\ y clientes Web. Los requisitos para certificados varían en función de si estás configurando un servidor de federación o un equipo de proxy, como se describe en esta sección.  
  
**Certificados de servidor de federación**  
  
|||  
|-|-|  
|**Tipo de certificado**|**Requisitos, soporte técnico y cosas que debes saber**|  
|**Certificado \(SSL\) capa de Sockets segura:** se trata de un certificado SSL estándar que se usa para proteger las comunicaciones entre clientes y servidores de federación.|-Este certificado debe ser un públicamente trusted\ * X509 certificado v3.<br />-Todos los clientes que tienen acceso a cualquier extremo de AD FS deben confiar en este certificado. Se recomienda encarecidamente usar certificados emitidos por una entidad de certificación públicas \(third\-party\) \(CA\). Puedes usar un certificado SSL firmado self\ correctamente en los servidores de federación en un entorno de laboratorio de prueba. Sin embargo, para un entorno de producción, te recomendamos que obtener el certificado de una CA pública.<br />: Admite cualquier tamaño de la clave compatible con Windows Server 2012 R2 para certificados SSL.<br />-No no los certificados de soporte técnico que usan claves CNG.<br />-Cuando se usa junto con el servicio de registro de empresa Join\ o dispositivo, el nombre alternativo del firmante del certificado SSL para el servicio de AD FS debe contener el enterpriseregistration de valor que sigue el sufijo \(UPN\) nombre Principal de usuario de la organización, por ejemplo, enterpriseregistration.contoso.com.<br />-Se admiten los certificados carácter comodín. Al crear el conjunto de AD FS, se te pedirá para proporcionar el nombre de servicio para el servicio de AD FS \ (por ejemplo, **adfs.contoso.com**.<br />-Se recomienda encarecidamente usar el mismo certificado SSL para el Proxy de aplicación Web. Sin embargo esto es **requiere** a ser el mismo cuando se admiten los extremos de la autenticación integrada de Windows a través del Proxy de aplicación Web y cuando se activa \(default setting\) extendido autenticación de protección.<br />-El nombre del sujeto del certificado se usa para representar el nombre de servicios de federación de cada instancia de AD FS que implementas. Por este motivo, quieres considera la posibilidad de elegir un nombre de asunto en los certificados emitidos CA\ nuevo que mejor represente el nombre de tu empresa u organización para los partners.<br />    La identidad del certificado debe coincidir con el nombre de servicio de federación \ (por ejemplo, fs.contoso.com\). La identidad es una extensión de nombre alternativas de asunto de dNSName de tipo, o bien, si no hay ninguna entrada de nombre alternativo del firmante, el nombre de firmante especificado como un nombre común. Varias entradas de nombre alternativo del firmante pueden estar presentes en el certificado, siempre que uno de ellos coincide con el nombre de servicio de federación.<br />-   **Importante:** se recomienda usar el mismo certificado SSL en todos los nodos de la granja de servidores de AD FS, así como todos los servidores proxy de aplicación Web en el conjunto de AD FS.|  
|**Certificado de comunicación de servicio:** este certificado permite seguridad para proteger las comunicaciones entre servidores de federación de WCF los mensajes.|-De manera predeterminada, el certificado SSL se usa como el certificado de comunicaciones de servicio.  Pero también tienes la opción de configurar otro certificado como el certificado de comunicación de servicio.<br />-   **Importante:** si utilizas el certificado SSL como el certificado de comunicación de servicio, cuando expira el certificado SSL, asegúrate de que configurar el certificado SSL renovado como el certificado de comunicación de servicio. Esto no se produce automáticamente.<br />-Este certificado debe ser de confianza para los clientes de AD FS que usan la seguridad de mensajes de WCF.<br />-Te recomendamos que uses un certificado de autenticación de servidor emitido por una entidad de certificación públicas \(third\-party\) \(CA\).<br />-El certificado de comunicación de servicio no puede ser un certificado que usa las claves CNG.<br />-Este certificado puede administrarse mediante la consola de administración de AD FS.|  
|**Certificado de firma de Token\:** este es un estándar X509 certificado que se usa para firmar todos los tokens que emite el servidor de federación de forma segura.|-De manera predeterminada, AD FS crea un certificado de firma de self\ con claves de 2048 bits.<br />-Emitida certificados también son compatibles y pueden modificarse mediante la administración de AD FS snap\ en<br />-CA certificados emitido se debe almacena y accede a través de un proveedor de criptografía de CSP.<br />-El token de certificado de firma no puede ser un certificado que usa las claves CNG.<br />-AD FS no requiere externamente inscritos certificados de firma de token.<br />    AD FS renueva automáticamente estos certificados de firma de self\ antes de que expiren, primero configurar los certificados nuevos como certificados secundarios para permitir que los socios consumirlos y, luego, voltear para principal en un proceso denominado renovación automática de certificados. Te recomendamos que uses el valor predeterminado, generados automáticamente certificados de firma de tokens.<br />    Si tu organización tiene directivas que requieren diferentes certificados estén configurados para la firma de tokens, puedes especificar los certificados en tiempo de instalación mediante Powershell \ (usa el parámetro: SigningCertificateThumbprint de la cmdlet\ Install\ AdfsFarm).  Después de la instalación, puede ver y administrar certificados de firma de tokens mediante la consola de administración de AD FS o los cmdlets de Powershell Set AdfsCertificate y Get\ AdfsCertificate.<br />    Cuando se usan externamente inscritos certificados de firma de tokens, AD FS no realiza la renovación automática de certificados o sustitución.  Este proceso se debe realizar un administrador.<br />    Para permitir que para la sustitución del certificado cuando un certificado está a punto de expirar –, un certificado de firma de token secundario puede configurarse en AD FS. De manera predeterminada, todos los certificados de firma de tokens se publican en los metadatos de federación, pero se usa solo el certificado de firma token\ principal por AD FS firmar tokens.|  
|**Certificado de cifrado de decryption\ de Token\:** esto es un estándar de X509 de certificados es usa para decrypt\ o cifrar los tokens entrantes. También se publica en los metadatos de federación.|-De manera predeterminada, AD FS crea un certificado de firma de self\ con claves de 2048 bits.<br />-Emitida certificados también son compatibles y pueden modificarse mediante la administración de AD FS snap\ en<br />-CA certificados emitido se debe almacena y accede a través de un proveedor de criptografía de CSP.<br />-El certificado de cifrado de decryption\ de token\ no puede ser un certificado que usa las claves CNG.<br />-De manera predeterminada, AD FS genera y usa sus propio, certificados de firma self\ e internamente generados para el descifrado de token.  AD FS no requiere certificados inscritos externamente para este propósito.<br />    Además, AD FS renueva automáticamente estos certificados de firma de self\ antes de que expiren.<br />    **Te recomendamos que uses el valor predeterminado, certificados generados automáticamente para el descifrado de token.**<br />    Si tu organización tiene directivas que requieran certificados diferentes esté configurado para el descifrado de token, puedes especificar los certificados en tiempo de instalación mediante Powershell \ (usa el parámetro: DecryptionCertificateThumbprint de la cmdlet\ Install\ AdfsFarm).  Después de la instalación, puede ver y administrar certificados de descifrado token mediante la consola de administración de AD FS o los cmdlets de Powershell Set AdfsCertificate y Get\ AdfsCertificate.<br />    **Cuando se usan certificados inscritos externamente para el descifrado de token, AD FS no realiza la renovación automática de certificados. Este proceso se debe realizar un administrador**.<br />-La cuenta de servicio de AD FS debe tener acceso a la clave privada del certificado de firma de token\ en el almacén personal del equipo local. Esto se ocupa de instalación. También puedes usar la administración de AD FS en snap\ para asegurarte de este acceso si posteriormente cambias el certificado de firma token\.|  
  
> [!CAUTION]  
> Certificados que se usan para firmar token\ y token\-decrypting\ o cifrar son esenciales para la estabilidad del servicio de federación. Los clientes administrar sus propios certificados de firma de token\ & token\-decrypting\ o cifrar deben asegurarse de que estos certificados están disponibles independientemente durante un evento de recuperación o copian de seguridad.  
  
> [!NOTE]  
> En AD FS puede cambiar el nivel de algoritmo Hash seguro \(SHA\) que se usa para las firmas digitales para \(more secure\) SHA\-1 o SHA\-256. AD FS no admite el uso de certificados con otros métodos de hash, como MD5 \ (el algoritmo hash predeterminado que se usa con la tool\ Web\ línea Makecert.exe). Como procedimiento recomendado de seguridad, te recomendamos que uses SHA\-256 \ (que se establece por default\) para todas las firmas. Se recomienda SHA\-1 para uso exclusivo en escenarios en los que deben interoperar con un producto que no es compatible con las comunicaciones mediante SHA\-256, como un non\-producto o heredado las versiones de Microsoft AD FS.  
  
> [!NOTE]  
> Después de recibir un certificado de una entidad de certificación, asegúrate de que todos los certificados se importan en el almacén de certificados personales del equipo local. Puedes importar certificados al almacén personal con MMC snap\ en certificados.  
  
## <a name="BKMK_2"></a>Requisitos de hardware  
Los siguientes requisitos mínimos de hardware se aplican a los servidores de federación de AD FS de Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisitos de hardware**|**Requisito mínimo**|**Requisito recomendado**|  
|Velocidad de CPU|Procesador de 1,4 GHz 64\ bits|Quad\ núcleo, 2 GHz|  
|RAM|512 MB|4 GB|  
|Espacio en disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisitos de software  
Los siguientes requisitos de AD FS son para la funcionalidad del servidor que está integrada en el sistema operativo de Windows Server® 2012 R2:  
  
-   Obtener acceso a la extranet, debes implementar el servicio de rol de Proxy de aplicación Web \ - parte del rol de servidor de acceso remoto de Windows Server® 2012 R2. No se admiten las versiones anteriores de un proxy de servidor de federación con AD FS en Windows Server® 2012 R2.  
  
-   No se puede instalar un servidor de federación y el servicio de rol de Proxy de aplicación Web en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
Controladores de dominio de todos los dominios de usuario y el dominio al que se unen los servidores de AD FS deben ejecutar Windows Server 2008 o posterior.  
  
> [!NOTE]  
> Toda la ayuda para entornos con controladores de dominio de Windows Server 2003 finalizará después extendido soporte final fecha para Windows Server 2003. Los clientes se recomiendan actualizar los controladores de dominio tan pronto como sea posible. Visita [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener más información sobre el ciclo de vida de soporte técnico de Microsoft. Para los problemas detectados, que son específicos para entornos de controlador de dominio de Windows Server 2003, correcciones se emitirá solo para los problemas de seguridad y si se puede emitir una corrección antes de la expiración de los Extended soporte técnico para Windows Server 2003.  
  
**Requisitos de nivel de functional\ de dominio**  
  
Todos los dominios de cuentas de usuario y el dominio al que se unen los servidores de AD FS deben funcionar en el nivel funcional de dominio de Windows Server 2003 o posterior.  
  
La mayoría de características de AD FS no requieren modificaciones en el nivel de functional\ AD DS para que funcionen correctamente. Sin embargo, funcional del dominio de Windows Server 2008 nivel o superior es necesario para la autenticación de certificado de cliente para que funcionen correctamente si el certificado se asigne explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquema**  
  
-   AD FS no requiere cambios de esquema o modificaciones en el nivel de functional\ en AD DS.  
  
-   Para usar la funcionalidad al área de trabajo, el esquema del bosque que los servidores de AD FS están unidos a debe establecerse en Windows Server 2012 R2.  
  
**Requisitos de la cuenta de servicio**  
  
-   Cualquier cuenta de servicio estándar puede usarse como una cuenta de servicio de AD FS. También se admiten las cuentas de servicio administrado de grupo. Esto requiere al menos un controlador de dominio \ (se recomienda implementar dos o more\) que ejecuta Windows Server 2012 o superior.  
  
-   Para la autenticación de Kerberos a función entre clientes unido domain\ y AD FS, la ' HOST\ / < adfs\_service\_name >' debe registrarse como un SPN en la cuenta de servicio. De manera predeterminada, AD FS configurar esto al crear un nuevo conjunto de AD FS si tiene los permisos necesarios para realizar esta operación.  
  
-   La cuenta de servicio de AD FS debe ser de confianza en cada dominio del usuario que contiene los usuarios autenticarse en el servicio de AD FS.  
  
**Requisitos de dominio.**  
  
-   Todos los servidores de AD FS deben ser un han unido a un dominio de AD DS.  
  
-   Todos los servidores de AD FS dentro de una granja de servidores deben implementarse en un solo dominio.  
  
-   Cada dominio de la cuenta de usuario que contiene los usuarios autenticarse en el servicio de AD FS debe confiar en que los servidores de AD FS están unidos al dominio.  
  
**Requisitos de bosques múltiples**  
  
-   Debe confiar en que los servidores de AD FS están unidos al dominio cada dominio de la cuenta de usuario o el bosque que contiene los usuarios autenticarse en el servicio de AD FS.  
  
-   La cuenta de servicio de AD FS debe ser de confianza en cada dominio del usuario que contiene los usuarios autenticarse en el servicio de AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de la base de datos de configuración  
Estos son los requisitos y restricciones que se aplican en función del tipo de almacén de configuración:  
  
**WID**  
  
-   Una granja de servidores WID tiene un límite de 30 servidores de federación si tienes 100 confianzas de terceros de confianza.  
  
-   Perfil de resolución artefacto en SAML 2.0 no se admite en la base de datos de configuración WID.  Detección de reproducción token no se admite en la base de datos de configuración WID. Esta funcionalidad solo se usa únicamente en escenarios donde es que actúa como el proveedor de federación y consumir tokens de seguridad de proveedores externos reclamaciones AD FS.  
  
-   Implementación de servidores de AD FS en datos diferentes recursos para conmutación por error o equilibrio de carga geográfica es compatible, como el número de servidores no superar los 30.  
  
La siguiente tabla proporciona un resumen de uso de una granja de servidores WID.  Úsalo para planear la implementación.  
  
||||  
|-|-|-|  
||1 \-100 confianzas de punto de reunión|Más de 100 confianzas de punto de reunión|  
|1 \-30 AD FS nodos|WID compatible|No se admite con WID \-SQL necesario|  
|Más de 30 AD FS nodos|No se admite con WID \-SQL necesario|No se admite con WID \-SQL necesario|  
  
**SQL Server**  
  
AD FS en Windows Server 2012 R2, puedes usar SQL Server 2008 y versiones posterior  
  
## <a name="BKMK_6"></a>Requisitos del explorador  
Cuando se realiza la autenticación de AD FS a través de un explorador o un control de explorador, el explorador debe cumplir los siguientes requisitos:  
  
-   JavaScript debe estar habilitado  
  
-   Las cookies deben estar activadas  
  
-   Servidor indicación del nombre \(SNI\) debe ser compatible  
  
-   Para la autenticación de certificado de usuario certificado y dispositivo \ (functionality\ de unión a un área de trabajo), el explorador debe admitir la autenticación de certificado de cliente SSL  
  
Varias plataformas y claves exploradores han sufrido validación de representación y la funcionalidad de los detalles de los cuales se enumeran a continuación. Aún se admiten los exploradores y dispositivos que no se tratan en esta tabla si cumplen los requisitos enumerados anteriormente:  
  
|||  
|-|-|  
|**Exploradores**|**Plataformas**|  
|IE 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Agente de autenticación Web de Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\ X 10.7|  
|Cromo \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7.|  
  
> [!IMPORTANT]  
> Problema conocido \-Firefox: funcionalidad al área de trabajo que identifica el dispositivo con el certificado de dispositivo no es funcional en plataformas de Windows. Firefox no admite actualmente realizar la autenticación de certificado de cliente SSL mediante certificados aprovisionados en el almacén de certificados de usuario en los clientes de Windows.  
  
**Cookies**  
  
AD FS crea cookies persistentes y en-based session\ que deben almacenarse en los equipos cliente a proporcionar en sign\, sign\ de salida, solo \(SSO\) sign\ en y otras funcionalidades. Por lo tanto, el explorador del cliente debe configurarse para que acepte cookies. Las cookies que se usan para la autenticación siempre son las cookies de sesión de protocolo seguro de transferencia de hipertexto \(HTTPS\) que se han escrito para el servidor de origen. Si el explorador del cliente no está configurado para permitir que estas cookies, AD FS no puede funcionar correctamente. Las cookies persistentes se usan para conservar la selección del usuario de que el proveedor de notificaciones. Para deshabilitarlas mediante el uso de una opción de configuración en el archivo de configuración para las páginas de sign\ de AD FS. Compatibilidad con SSL/TLS\ es necesario por motivos de seguridad.  
  
## <a name="BKMK_extranet"></a>Requisitos de extranet  
Para proporcionar acceso a la extranet al servicio de AD FS, debes implementar el servicio de rol de Proxy de aplicación Web que la función frontal extranet que solicitudes de autenticación de proxy de forma segura para el servicio de AD FS. Esto proporciona el aislamiento de los extremos de servicios de AD FS, así como el aislamiento de todas las claves de seguridad \ (por ejemplo, el token certificates\ firma) de las solicitudes que proceden de internet. Además, características, como el bloqueo de cuenta Extranet suave requieren el uso de los Proxy de aplicación Web. Para obtener más información acerca de Proxy de aplicación Web, consulta [Proxy de aplicación Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Si quieres usar un proxy third\ terceros para acceder extranet, este proxy third\ terceros debe admitir el protocolo definido en [http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisitos de la red  
Correctamente la configuración de los siguientes servicios de red es fundamental para la correcta implementación de AD FS de la organización:  
  
**Configuración de Firewall corporativa**  
  
Encuentra entre el Proxy de aplicación Web y de la granja de servidores de federación de firewall y el firewall entre los clientes y el Proxy de aplicación Web deben tener el puerto TCP 443 habilitado entrante.  
  
Además, si la autenticación de certificados de usuario de cliente \ (autenticación clientTLS con X509 certificates\ del usuario) es necesario, AD FS en Windows Server 2012 R2 requiere que esté habilitado el puerto TCP 49443 entrante en el firewall entre los clientes y el Proxy de aplicación Web. Esto no es necesario en el firewall entre el Proxy de aplicación Web y la servidores\ federación).  
  
**Configuración de DNS**  
  
-   Obtener acceso a la intranet, tener acceso al servicio de AD FS dentro de la red corporativa interna de \(intranet\) de todos los clientes deben poder resolver el nombre de servicio de AD FS \ (nombre proporcionado por el certificate\ SSL) para el equilibrado de carga de los servidores de AD FS o del servidor de AD FS.  
  
-   Obtener acceso a la extranet, tener acceso al servicio de AD FS desde fuera de la red corporativa de \(extranet\/internet\) de todos los clientes deben poder resolver el nombre de servicio de AD FS \ (nombre proporcionado por el certificate\ SSL) para el equilibrado de carga de los servidores Proxy de aplicación Web o el servidor Proxy de aplicación Web.  
  
-   Para que extranet acceso funcionar correctamente, cada servidor Proxy de aplicación Web en la DMZ debe ser capaz de resolver el nombre de servicio de AD FS \ (nombre proporcionado por el certificate\ SSL) para el equilibrado de carga de los servidores de AD FS o del servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo de la red DMZ o cambiando la resolución de servidor local mediante el archivo de HOSTS.  
  
-   Para que la autenticación integrada de Windows trabajar dentro de la red y fuera de la red para un subconjunto de extremos que se exponen a través del Proxy de aplicación Web, debes usar un un registro \(not CNAME\) para apuntar a los equilibradores de carga.  
  
Para obtener información sobre la configuración corporativa DNS para el servicio de federación y servicio de registro del dispositivo, consulta [configurar DNS corporativa para los servicios de federación y DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Para obtener información sobre la configuración de DNS corporativa para los servidores proxy de aplicación Web, consulta la sección "Configurar DNS" en [paso 1: configurar la infraestructura de Proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
¿Para obtener información acerca de cómo configurar una dirección IP o FQDN mediante NLB del clúster, vea Especificar los parámetros de clúster en [http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Requisitos de la tienda de atributo  
AD FS requiere al menos un almacén de atributo que se usará para autenticar usuarios y notificaciones de seguridad para los usuarios de la extracción. Para obtener una lista del atributo almacena que es compatible con AD FS, consulta [el rol de atributo almacena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crea automáticamente un almacén de atributo "Active Directory", de manera predeterminada. Requisitos de la tienda de atributo dependen de si la organización actúa como el asociado de cuenta \ (que aloja el users\ federados) o el asociado de recurso \ (que aloja el application\ federados).  
  
**Almacenes de atributo de LDAP**  
  
Cuando trabajas con otro protocolo ligero de acceso a directorios \ (LDAP\) \-based atributo tiendas, debe conectarse a un servidor LDAP que admite la autenticación integrada de Windows. La cadena de conexión de LDAP debe escribirse en el formato de una dirección URL de LDAP, como se describe en la RFC 2255.  
  
También es necesario que la cuenta de servicio para el servicio de AD FS tiene el derecho a recuperar información de usuario en la tienda de atributo LDAP.  
  
**Almacenes de atributo de SQL Server**  
  
Para AD FS en Windows Server 2012 R2 para que funcionen correctamente, los equipos que hospedar el almacén de atributo de SQL Server deben estar ejecutándose Microsoft SQL Server 2008 o una versión posterior. Cuando trabajas con el atributo basado en SQL\ tiendas, también debes configurar una cadena de conexión.  
  
**Almacenes de atributo personalizada**  
  
Puedes desarrollar almacenes atributo personalizado para habilitar escenarios avanzados.  
  
-   El lenguaje sobre directivas que está integrado en AD FS hacer referencia a los almacenes de atributo personalizado para que se puedan mejorar cualquiera de los siguientes escenarios:  
  
    -   Creación de notificaciones para un usuario autenticado localmente  
  
    -   Complementan reclamaciones por un usuario autenticado externamente  
  
    -   Autorización de un usuario para obtener un token  
  
    -   Autorización de un servicio para obtener un token en el comportamiento de un usuario  
  
    -   Emitir datos adicionales en los tokens de seguridad emitidos por AD FS para usuarios de confianza.  
  
-   Todos los almacenes del atributo personalizado deben crearse en la parte superior .NET 4.0 o superior.  
  
Cuando se trabaja con un almacén de atributo personalizado, es posible que debas configurar una cadena de conexión. En ese caso, puedes escribir un código personalizado de la opción que permite una conexión a la tienda de atributo personalizado. La cadena de conexión en esta situación es un conjunto de pares nombre-valor equipo\ que se interpretan como implementados por el desarrollador de la tienda de atributo personalizado. Para obtener más información acerca del desarrollo y usando el atributo personalizado tiendas, consulta [Introducción a la tienda atributo](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisitos de la aplicación  
AD FS admite claims\ aplicaciones que usan los siguientes protocolos:  
  
-   Federación de WS\  
  
-   WS\-Trust  
  
-   Protocolo de SAML 2.0 mediante perfiles IDPLite, SPLite y eGov1.5.  
  
-   Perfil de concesión de autorización de OAuth 2.0  
  
AD FS también admite la autenticación y autorización para cualquier non\ claims\-aplicaciones que son compatibles con el Proxy de aplicación Web.  
  
## <a name="BKMK_10"></a>Requisitos de autenticación  
**AD DS \(Primary Authentication\) autenticación**  
  
Obtener acceso a la intranet, se admiten los siguientes mecanismos de autenticación estándar de AD DS:  
  
-   Autenticación integrada en Windows mediante Negotiate para Kerberos y NTLM  
  
-   Autenticación de formularios mediante username\ y contraseñas  
  
-   Uso de certificados asignados a cuentas de usuario en AD DS de la autenticación de certificado  
  
Obtener acceso a la extranet, se admiten los siguientes mecanismos de autenticación:  
  
-   Autenticación de formularios mediante username\ y contraseñas  
  
-   Uso de certificados están asignados a cuentas de usuario en AD DS de la autenticación de certificado  
  
-   Autenticación integrada de Windows con Negotiate \(NTLM only\) para los extremos de confianza WS\ que aceptan la autenticación integrada de Windows.  
  
Para la autenticación de certificado:  
  
-   Se extiende a las tarjetas inteligentes que se pueden anclar protegido.  
  
-   La interfaz gráfica de usuario para el usuario escriba su pin no se proporciona por AD FS y es necesaria para formar parte del sistema operativo cliente que se muestra cuando se utiliza al cliente TLS.  
  
-   El lector y el proveedor de servicios criptográficos \(CSP\) para la tarjeta inteligente debe funcionar en el equipo donde se encuentra el explorador.  
  
-   El certificado de tarjeta inteligente debe encadenas a raíz de confianza en todos los servidores de AD FS y servidores Proxy de aplicación Web.  
  
-   El certificado debe asignar a la cuenta de usuario en AD DS por cualquiera de los siguientes métodos:  
  
    -   El nombre de firmante del certificado que se corresponde con el nombre completo de LDAP de una cuenta de usuario en AD DS.  
  
    -   La extensión de certificado asunto altname tiene la entidad de seguridad de usuario nombre \(UPN\) de una cuenta de usuario en AD DS.  
  
Para la autenticación integrada de Windows sin problemas con Kerberos en la intranet,  
  
-   Es necesario para el nombre de servicio que forman parte de los sitios de confianza o sitios de Intranet Local.  
  
-   Además, la HOST\ / < adfs\_service\_name > SPN debe establecerse en la cuenta de servicio que se ejecuta la granja de servidores de AD FS en.  
  
**Autenticación de Factor de Multi\**  
  
AD FS admite la autenticación adicional \ (más allá de autenticación principal compatible con AD DS\) usando un proveedor de modelo según el cual vendors\ o los clientes pueden crear su propios adaptador de autenticación de factor de multi\ que un administrador puede registrar y usar durante el inicio de sesión.  
  
Todos los adaptadores de MFA deben basarse en .NET 4.5.  
  
Para obtener más información acerca de MFA, consulta [administrar riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticación de dispositivo**  
  
AD FS admite la autenticación de dispositivo mediante certificados aprovisionados el servicio de registro del dispositivo durante la acción de un área de trabajo del usuario final unirse a su dispositivo.  
  
## <a name="BKMK_11"></a>Requisitos de unión a un área de trabajo  
Los usuarios finales pueden unirse a un área de trabajo de sus dispositivos para una organización con AD FS. Esto es compatible con el servicio de registro del dispositivo en AD FS. Como resultado, los usuarios finales aprovechar las ventajas adicionales de SSO en todas las aplicaciones compatibles con AD FS. Además, los administradores pueden administrar riesgo por restringir el acceso a las aplicaciones solo para dispositivos que se han unido a la organización de empresa. A continuación son los siguientes requisitos para habilitar este escenario.  
  
-   AD FS admite unirse a un área de trabajo para Windows 8.1 y dispositivos 5\ + iOS  
  
-   Para usar la funcionalidad al área de trabajo, el esquema del bosque que los servidores de AD FS están unidos a debe ser Windows Server 2012 R2.  
  
-   El nombre alternativo del firmante del certificado SSL para el servicio de AD FS debe contener el enterpriseregistration de valor que sigue el sufijo \(UPN\) nombre Principal de usuario de la organización, por ejemplo, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Requisitos de criptografía  
La siguiente tabla proporciona información de soporte técnico de criptografía adicionales en el token de AD FS firma, la funcionalidad de token encryption\ y descifrado:  
  
||||  
|-|-|-|  
|**Algoritmo**|**Longitudes de clave**|**Applications\/Protocols\/comentarios**|  
|DES triple – predeterminado 192 \ (256\ 192 – compatibles) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algoritmos compatibles para cifrar el token de seguridad.|  
|Aes128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|Algoritmos compatibles para cifrar el token de seguridad.|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|Algoritmos compatibles para cifrar el token de seguridad.|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Valor predeterminado**. Algoritmos compatibles para cifrar el token de seguridad.|  
|TripleDESKeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|Todos los tamaños de clave compatibles con .NET 4.0\+|Admite algoritmo para cifrar la clave simétrica que cifra un token de seguridad.|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Admite algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Admite algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Admite algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|Admite algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|De forma predeterminada. Admite algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|SHA1\ http: / / / \ / www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Servidor de AD FS utiliza en la generación de SourceId artefacto: en este escenario, el STS usa SHA1 \ (por la recomendación en el standard\ SAML 2.0) para crear un valor de bit 160 corto para sourceiD artefacto.<br /><br />También usa el agente de web ADFS \ (heredado componente desde 2003 timeframe\) para identificar los cambios en una hora de "última actualización" valor para que sepa cuándo se debe actualizar la información de STS.|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Usado en casos al servidor de AD FS valida la firma de SAML AuthenticationRequest, iniciar la solicitud de resolución artefacto o respuesta, crea el certificado de firma de token\.<br /><br />En estos casos, SHA256 es el valor predeterminado y SHA1 solo se usa si el \(relying party\) partner no es compatible con SHA256 y debe utilizar SHA1.|  
  
## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que realiza la instalación y la configuración inicial de AD FS debe tener permisos de administrador de dominio en el dominio local \ (en otras palabras, el dominio al que el servidor de federación está unido a. \)  
  
## <a name="see-also"></a>Consulta también  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

