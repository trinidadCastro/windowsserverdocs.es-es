---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Granja de servidores de federación con SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6aa91956f4a90b32b82e6c970e68b3164c732f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191714"
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS

Los siguientes son los distintos requisitos que deben ajustarse al implementar AD FS:  
  
-   [Requisitos de certificado](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisitos de software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisitos de la base de datos de configuración](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisitos del explorador](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisitos de extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisitos de red](AD-FS-Requirements.md#BKMK_7)  
  
-   [Requisitos del almacén de atributos](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisitos de la aplicación](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisitos de autenticación](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisitos de unión al área de trabajo](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisitos de criptografía](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisitos de permisos](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificados  
Los certificados desempeñan el papel más importante en asegurar las comunicaciones entre federación notificaciones de servidores, servidores proxy de aplicación Web,\-aplicaciones compatibles y los clientes Web. Los requisitos de certificados varían en función de si está configurando un servidor de federación o un equipo proxy, como se describe en esta sección.  
  
**Certificados de servidor de federación**  
  
|||  
|-|-|  
|**Tipo de certificado**|**Los requisitos, soporte técnico y aspectos que debe conocer**|  
|**Capa de Sockets seguros \(SSL\) certificado:** Se trata de un certificado SSL estándar que se usa para proteger las comunicaciones entre clientes y servidores de federación.|-Este certificado debe ser de confianza pública\* X509 certificado v3.<br />-Todos los clientes que tienen acceso a cualquier punto de conexión de AD FS deben confiar en este certificado. Se recomienda encarecidamente usar certificados emitidos por una pública \(tercer\-entidad\) entidad de certificación \(CA\). Puede usar un autoservicio\-firmado SSL certificate correctamente en servidores de federación en un entorno de laboratorio de prueba. Sin embargo, para un entorno de producción, recomendamos que obtenga el certificado de una entidad de certificación pública.<br />-Admite cualquier tamaño de clave compatible con Windows Server 2012 R2 para los certificados SSL.<br />-No no admite certificados que utilizan claves CNG.<br />-Cuando se usa junto con unión\/Device Registration Service, el nombre alternativo del sujeto del certificado SSL para el servicio AD FS debe contener el enterpriseregistration de valor está seguido por el nombre Principal de usuario \(UPN\) sufijo de su organización, por ejemplo, registroempresa.contoso.com.<br />-Se admiten certificados comodín. Cuando se crea la granja de AD FS, se le pedirá que proporcione el nombre del servicio para el servicio AD FS \(por ejemplo, **adfs.contoso.com**.<br />-Se recomienda encarecidamente utilizar el mismo certificado SSL para el Proxy de aplicación Web. Sin embargo es **requiere** sea el mismo cuando se admiten puntos de conexión de autenticación integrada de Windows a través del Proxy de aplicación Web y cuando está activada la autenticación de protección extendida \(configuración predeterminada\).<br />-El nombre de asunto de este certificado se utiliza para representar el nombre de servicio de federación para cada instancia de AD FS que implementes. Por este motivo, es posible que desee considere la posibilidad de elegir un nombre de firmante en cualquier entidad de certificación nueva\-emite certificados que mejor representa el nombre de su compañía u organización a los socios comerciales.<br />    La identidad del certificado debe coincidir con el nombre del servicio de federación \(por ejemplo, fs.contoso.com\). La identidad es una extensión de nombre alternativo de sujeto de tipo dNSName, o bien, si no hay ninguna entrada de nombre alternativo de asunto, el nombre del sujeto especificado como un nombre común. Varias entradas de nombre alternativo del sujeto pueden estar presentes en el certificado, siempre que uno de ellos coincide con el nombre del servicio de federación.<br />-   **Importante:** se recomienda encarecidamente utilizar el mismo certificado SSL en todos los nodos de la granja de AD FS, así como todos los servidores proxy de aplicación Web en la granja de AD FS.|  
|**Certificado de comunicación de servicio:** Este certificado habilita la seguridad de mensajes WCF para proteger las comunicaciones entre los servidores de federación.|: De forma predeterminada, el certificado SSL se utiliza como el certificado de comunicaciones de servicio.  Pero también tiene la opción para configurar otro certificado como certificado de comunicación de servicio.<br />-   **Importante:** si usas el certificado SSL como certificado de comunicación de servicio, cuando expire el certificado SSL, asegúrese de configurar el certificado SSL renovado como su certificado de comunicación de servicio. Esto no se realiza automáticamente.<br />-En este certificado debe ser de confianza por los clientes de AD FS que utilizan la seguridad de mensaje de WCF.<br />-Se recomienda usar un certificado de autenticación de servidor emitido por una pública \(tercer\-entidad\) entidad de certificación \(CA\).<br />-El certificado de comunicación de servicio no puede ser un certificado que usa claves CNG.<br />-En este certificado se puede administrar mediante la consola de administración de AD FS.|  
|**Token\-certificado de firma:** Se trata de un certificado X509 estándar que se utiliza para firmar de manera segura todos los tokens que emite el servidor de federación.|: De forma predeterminada, AD FS crea un autoservicio\-certificado autofirmado con las claves de 2048 bits.<br />-Certificados CA emitida también se admiten y se puede cambiar mediante el complemento Administración de AD FS\-en<br />-CA emitida certificados debe almacenado & se tiene acceso a través de un proveedor criptográfico de CSP.<br />-El certificado de firma de tokens no pueden ser un certificado que usa claves CNG.<br />-AD FS no requiere certificados inscritos externamente de firma de tokens.<br />    AD FS renueva automáticamente estos self\-certificados autofirmados antes de que expiren, configurando primero los nuevos certificados como certificados secundarios para permitir asociados para utilizarlos, y luego Voltear a principal en un proceso llamado automática sustitución del certificado. Se recomienda que utilice el valor predeterminado, los certificados generados automáticamente para la firma de tokens.<br />    Si su organización tiene directivas que requieren certificados diferentes para configurarse para la firma de tokens, puede especificar los certificados durante la instalación con Powershell \(use el parámetro – SigningCertificateThumbprint de la instalación \-AdfsFarm cmdlet\).  Después de la instalación, puede ver y administrar certificados de firmas de tokens mediante la consola de administración de AD FS o el conjunto de cmdlets de Powershell\-AdfsCertificate y Get\-AdfsCertificate.<br />    Cuando se usan certificados inscritos externamente para la firma de tokens, AD FS no lleva a cabo la renovación automática de certificados o sustitución.  Este proceso debe realizarla un administrador.<br />    Para permitir que la sustitución de certificados cuando un certificado está a punto de expirar, se pueden configurar un certificado de firma de tokens secundario en AD FS. De forma predeterminada, todos los certificados de firmas de tokens se publican en los metadatos de federación, pero solo el token primario\-certificado de firma se usa para firmar los tokens de AD FS.|  
|**Token\-descifrado\/certificado de cifrado:** Se trata de un estándar X509 certificado que se utiliza para descifrar\/cifrar los tokens entrantes. También se publica en los metadatos de federación.|: De forma predeterminada, AD FS crea un autoservicio\-certificado autofirmado con las claves de 2048 bits.<br />-Certificados CA emitida también se admiten y se puede cambiar mediante el complemento Administración de AD FS\-en<br />-CA emitida certificados debe almacenado & se tiene acceso a través de un proveedor criptográfico de CSP.<br />-El token\-descifrado\/certificado de cifrado no puede ser un certificado que usa claves CNG.<br />: De forma predeterminada, AD FS genera y usa su propio, internamente generado y self\-firmado de certificados de descifrado de tokens.  AD FS no requiere certificados inscritos externamente para este propósito.<br />    Además, AD FS renueva automáticamente estos self\-certificados autofirmados antes de que caduquen.<br />    **Se recomienda que utilice el valor predeterminado, los certificados generados automáticamente para el descifrado de tokens.**<br />    Si su organización tiene directivas que requieren certificados diferentes para configurarse para el descifrado de tokens, puede especificar los certificados durante la instalación con Powershell \(use el parámetro – DecryptionCertificateThumbprint de la Instalar\-AdfsFarm cmdlet\).  Después de la instalación, puede ver y administrar certificados de descifrado de tokens mediante la consola de administración de AD FS o el conjunto de cmdlets de Powershell\-AdfsCertificate y Get\-AdfsCertificate.<br />    **Cuando se usan certificados inscritos externamente para el descifrado de tokens, AD FS no realiza la renovación automática de certificados.  Este proceso debe realizarla un administrador**.<br />-La cuenta de servicio de AD FS debe tener acceso al token\-firma de clave privada del certificado en el almacén personal del equipo local. Esto se ocupa del programa de instalación. También puede usar el complemento Administración de AD FS\-para asegurarse de este acceso si se cambia posteriormente el token\-certificado de firma.|  
  
> [!CAUTION]  
> Los certificados que se usan para el token\-token y firma\-descifrar\/cifrar son fundamentales para la estabilidad del servicio de federación. Los clientes que administran su propio token\-firma & token\-descifrar\/cifrar los certificados debe asegurarse de que estos certificados son una copia de seguridad y están disponibles por separado durante un evento de recuperación.  
  
> [!NOTE]  
> En AD FS puede cambiar el algoritmo Hash seguro \(SHA\) nivel que se usa para las firmas digitales para cualquier SHA\-1 o SHA\-256 \(más seguro\). AD FS no admite el uso de certificados con otros métodos hash, como MD5 \(el algoritmo hash predeterminado que se usa con el comando Makecert.exe\-herramienta de línea de\). Como práctica recomendada de seguridad, le recomendamos que use SHA\-256 \(que se establece de forma predeterminada\) para todas las firmas. SHA\-1 se recomienda para su uso solo en los casos en que se debe interoperar con un producto que no es compatible con las comunicaciones mediante SHA\-256, por ejemplo, una que no sean\-las versiones de producto o heredado de Microsoft de AD FS.  
  
> [!NOTE]  
> Después de recibir un certificado de una CA, asegúrate de que todos los certificados se importen al almacén de certificados personales del equipo local. Puede importar certificados al almacén personal con el complemento MMC certificados\-en.  
  
## <a name="BKMK_2"></a>Requisitos de hardware  
Los siguientes requisitos de hardware mínimos y recomendados se aplican a los servidores de federación de AD FS en Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|  
|Velocidad de CPU|64 de 1,4 GHz\-procesador de bits|Cuádruple\-núcleo, 2 GHz|  
|RAM|512 MB|4 GB|  
|Espacio en disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisitos de software  
Son los siguientes requisitos de AD FS para la funcionalidad del servidor que está integrada en el sistema operativo Windows Server® 2012 R2:  
  
-   Para el acceso de extranet, debe implementar el servicio de rol Proxy de aplicación Web \- forma parte de la función de servidor de acceso remoto de Windows Server® 2012 R2. No se admiten las versiones anteriores de un servidor proxy de federación con AD FS en Windows Server® 2012 R2.  
  
-   No se puede instalar un servidor de federación y el servicio de rol Proxy de aplicación Web en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
Controladores de dominio en todos los dominios de usuario y el dominio al que se unen los servidores de AD FS deben ejecutar Windows Server 2008 o posterior.  
  
> [!NOTE]  
> Compatibilidad con todos los entornos con controladores de dominio de Windows Server 2003 de finalizará después del extendidos admiten final fecha para Windows Server 2003. Los clientes se recomiendan encarecidamente actualizar sus controladores de dominio tan pronto como sea posible. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener más información sobre el ciclo de vida de soporte técnico de Microsoft. Para los problemas detectados que son específicas para entornos de controlador de dominio de Windows Server 2003, correcciones se emitirá solo para problemas de seguridad y si se puede emitir una corrección antes de la expiración de extendidos soporte técnico para Windows Server 2003.  
  
**Funcional del dominio\-requisitos de niveles**  
  
Todos los dominios de cuentas de usuario y el dominio al que se unen los servidores de AD FS deben estar funcionando en el nivel funcional del dominio de Windows Server 2003 o superior.  
  
La mayoría de las características de AD FS no necesitan AD DS funcional\-nivel modificaciones para que funcione correctamente. Sin embargo, se requiere un nivel funcional de dominio de Windows Server 2008 o superior para que la autenticación del certificado de cliente funcione correctamente, si dicho certificado está asignado explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquema**  
  
-   AD FS no requiere cambios de esquema o funcional\-nivel modificaciones en AD DS.  
  
-   Para usar la funcionalidad de unión, el esquema del bosque que se unen servidores de AD FS para debe establecerse en Windows Server 2012 R2.  
  
**Requisitos de la cuenta de servicio**  
  
-   Cualquier cuenta de servicio estándar se puede usar como una cuenta de servicio de AD FS. También se admiten cuentas de servicio administradas de grupo. Esto requiere al menos un controlador de dominio \(, se recomienda que implemente dos o más\) que ejecuta Windows Server 2012 o posterior.  
  
-   Para la autenticación Kerberos funcione entre dominio\-reunido a los clientes y AD FS, el "HOST\/< adfs\_servicio\_nombre >' debe estar registrado como un SPN en la cuenta de servicio. De forma predeterminada, AD FS para configurar esto al crear una nueva granja de AD FS si tiene permisos suficientes para realizar esta operación.  
  
-   La cuenta de servicio de AD FS debe ser de confianza en cada dominio del usuario que contiene los usuarios autenticarse en el servicio AD FS.  
  
**Requisitos de dominio**  
  
-   Todos los servidores de AD FS deben ser un unido a un dominio de AD DS.  
  
-   Todos los servidores de AD FS en una granja de servidores deben implementarse en un solo dominio.  
  
-   Cada dominio de la cuenta de usuario que contenga los usuarios autenticarse en el servicio AD FS debe confiar en que los servidores de AD FS están unidos al dominio.  
  
**Requisitos de varios bosques**  
  
-   Cada dominio de la cuenta de usuario o el bosque que contiene los usuarios autenticarse en el servicio AD FS debe confiar en que los servidores de AD FS están unidos al dominio.  
  
-   La cuenta de servicio de AD FS debe ser de confianza en cada dominio del usuario que contiene los usuarios autenticarse en el servicio AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de la base de datos de configuración  
Los siguientes son los requisitos y restricciones que se aplican en función del tipo de almacén de configuración:  
  
**WID**  
  
-   Una granja WID tiene un límite de 30 servidores de federación si tiene 100 o menos relaciones de confianza.  
  
-   Perfil de resolución de artefactos de SAML 2.0 no se admite en la base de datos de configuración de WID.  No se admite la detección de reproducción de tokens en la base de datos de configuración de WID. Esta funcionalidad solo se usa solo en escenarios donde se actúa como proveedor de federación de AD FS y consumir tokens de seguridad de proveedores de notificaciones externo.  
  
-   Implementación de servidores de AD FS en datos distintos centros de conmutación por error o equilibrio de carga geográfico se admite como el número de servidores no superar los 30.  
  
En la tabla siguiente proporciona un resumen de uso de una granja WID.  Usar para planear su implementación.  
  
||||  
|-|-|-|  
||1 \- 100 de RP de confianzas|Más de 100 de RP de confianzas|  
|1 \- 30 AD FS nodos|WID admitida|No se admite con WID \- SQL necesario|  
|Nodos de más de 30 AD FS|No se admite con WID \- SQL necesario|No se admite con WID \- SQL necesario|  
  
**SQL Server**  
  
Para AD FS en Windows Server 2012 R2, puede usar SQL Server 2008 y versiones posterior  
  
## <a name="BKMK_6"></a>Requisitos del explorador  
Cuando se realiza la autenticación de AD FS a través de un explorador o el control del explorador, el explorador debe ser compatible con los siguientes requisitos:  
  
-   JavaScript debe estar habilitado  
  
-   Deben estar activadas las cookies  
  
-   Indicación de nombre de servidor \(SNI\) deben ser compatibles  
  
-   Para la autenticación de certificados de dispositivo de & certificado de usuario \(funcionalidad para unirse a área de trabajo\), el explorador debe admitir la autenticación de certificado de cliente SSL  
  
Varias plataformas y exploradores principales han sido objeto de validación para la representación y funcionalidad de los detalles de los cuales se enumeran a continuación. Exploradores y dispositivos que no se tratan en esta tabla todavía se admiten si cumplen los requisitos descritos anteriormente:  
  
|||  
|-|-|  
|**Exploradores**|**Plataformas**|  
|IE 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11.0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Agente de autenticación Web de Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7.|  
  
> [!IMPORTANT]  
> Problema conocido \- Firefox: Funcionalidad de unión al área de trabajo que identifica el dispositivo mediante el certificado de dispositivo no es funcional en plataformas de Windows. Firefox no admite actualmente realizar la autenticación de certificado de cliente SSL mediante certificados de aprovisionamiento en el almacén de certificados de usuario en clientes de Windows.  
  
**Cookies**  
  
AD FS crea sesión\-cookies persistentes y función que se deben almacenar en los equipos cliente para proporcionar inicio de sesión\-, iniciar sesión\-, inicio de sesión único\-en \(SSO\)y otras funciones. Por lo tanto, el explorador cliente debe estar configurado para aceptar cookies. Las cookies que se usan para la autenticación siempre son Protocolo seguro de transferencia de hipertexto \(HTTPS\) las cookies de sesión que se escriben para el servidor de origen. Si el explorador cliente no está configurado para permitir estas cookies, AD FS no funcionará correctamente. Las cookies persistentes se usan para conservar la selección del usuario del proveedor de notificaciones. Puede deshabilitar mediante el uso de un valor de configuración en el archivo de configuración para el inicio de sesión de AD FS\-en páginas. Compatibilidad con TLS\/SSL es necesario por motivos de seguridad.  
  
## <a name="BKMK_extranet"></a>Requisitos de extranet  
Para proporcionar acceso a extranet para el servicio AD FS, debe implementar el servicio de rol Proxy de aplicación Web como el rol accesible desde extranet que las solicitudes de autenticación de servidores proxy de forma segura para el servicio AD FS. Esto proporciona aislamiento de los puntos de conexión de servicio de AD FS, así como el aislamiento de todas las claves de seguridad \(como certificados de firma de tokens\) de solicitudes que se originan desde internet. Además, características como el bloqueo de cuenta de Extranet temporal requieren el uso del Proxy de aplicación Web. Para obtener más información sobre el Proxy de aplicación Web, consulte [Proxy de aplicación Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Si desea utilizar una tercera\-terceros proxy para el acceso de extranet, este tercero\-proxy de entidad debe admitir el protocolo definido en [http:\/\/download.microsoft.com\/descargar\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisitos de red  
Configurar los siguientes servicios de red de forma adecuada es crítico para la correcta implementación de AD FS en su organización:  
  
**Configurar el Firewall corporativo**  
  
El firewall situado entre el Proxy de aplicación Web y la granja de servidores de federación y el firewall entre los clientes y el Proxy de aplicación Web debe tener el puerto TCP 443 habilitado entrante.  
  
Además, si la autenticación de certificado de usuario del cliente \(autenticación de clientTLS mediante X509 certificados de usuario\) es necesario, AD FS en Windows Server 2012 R2 requiere que el puerto TCP 49443 esté habilitado entrantes en el firewall entre el los clientes y el Proxy de aplicación Web. Esto no es necesario en el firewall entre el Proxy de aplicación Web y los servidores de federación\).  
  
**Configuración de DNS**  
  
-   Obtener acceso a la intranet, todos los clientes tener acceso a AD FS del servicio dentro de la red corporativa interna \(intranet\) debe ser capaz de resolver el nombre del servicio AD FS \(nombre proporcionado por el certificado SSL\) a la carga equilibrador para los servidores de AD FS o el servidor de AD FS.  
  
-   Para el acceso de extranet, todos los clientes tener acceso a AD FS del servicio desde fuera de la red corporativa \(extranet\/internet\) debe ser capaz de resolver el nombre del servicio AD FS \(nombre proporcionado por el certificado SSL\) al equilibrador de carga para los servidores Proxy de aplicación Web o el servidor de Proxy de aplicación Web.  
  
-   Para que obtener acceso a extranet funcionar correctamente, cada servidor Proxy de aplicación Web en la red Perimetral debe ser capaz de resolver el nombre del servicio AD FS \(nombre proporcionado por el certificado SSL\) al equilibrador de carga para los servidores de AD FS o el servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo en la red Perimetral o cambiando la resolución del servidor local mediante el archivo de HOSTS.  
  
-   Para que la autenticación integrada de Windows para que funcione dentro de la red y fuera de la red para un subconjunto de los extremos expuestos a través del Proxy de aplicación Web, debe usar un registro \(CNAME no\) para que apunte a los equilibradores de carga.  
  
Para obtener información sobre cómo configurar DNS corporativo para el servicio de federación y el servicio registro de dispositivos, consulte [configurar DNS corporativo para el servicio de federación y DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Para obtener información sobre cómo configurar DNS corporativo para servidores proxy de aplicación Web, vea la sección "Configuración de DNS" en [paso 1: Configurar la infraestructura del Proxy de aplicación Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
¿Para obtener información acerca de cómo configurar una dirección IP o FQDN de clúster con NLB, consulte Especificación de los parámetros de clúster en [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Requisitos del almacén de atributos  
AD FS requiere al menos un almacén de atributos que se usará para autenticar a los usuarios y extraer notificaciones de seguridad para esos usuarios. Para obtener una lista de atributos almacenes compatibles con AD FS, vea [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crea automáticamente un almacén de atributos "Active Directory", de forma predeterminada. Requisitos del almacén de atributos dependen de si la organización actúa como el asociado de cuenta \(hospedaje a los usuarios federados\) o el asociado de recurso \(hospeda la aplicación federada\).  
  
**Los almacenes de atributos LDAP**  
  
Cuando se trabaja con otros Lightweight Directory Access Protocol \(LDAP\)\-en función de los almacenes de atributos, debe conectarse a un servidor LDAP que admite la autenticación integrada de Windows. Además, la cadena de conexión de LDAP debe estar escrita con el formato de una dirección URL de LDAP, tal y como se describe en RFC 2255.  
  
También es necesario que la cuenta de servicio para el servicio AD FS tiene el derecho a recuperar la información de usuario en el almacén de atributos LDAP.  
  
**Almacenes de atributos de SQL Server**  
  
Para AD FS en Windows Server 2012 R2 para que funcione correctamente, los equipos que hospedan el almacén de atributos de SQL Server deben ejecutar Microsoft SQL Server 2008 o posterior. Cuando se trabaja con SQL\-en función de los almacenes de atributos, también debe configurar una cadena de conexión.  
  
**Los almacenes de atributos personalizados**  
  
Puedes desarrollar almacenes de atributos personalizados para habilitar escenarios avanzados.  
  
-   El lenguaje de directiva integrado en AD FS puede hacer referencia a almacenes de atributos personalizados, con el fin de permitir una mejora en los siguientes escenarios:  
  
    -   Crear notificaciones para un usuario autenticado localmente  
  
    -   Complementar notificaciones para un usuario autenticado externamente  
  
    -   Autorizar a un usuario a obtener un token  
  
    -   Autorizar a un servicio a obtener un token del comportamiento de un usuario  
  
    -   Emitir datos adicionales en los tokens de seguridad emitidos por AD FS para los usuarios autenticados.  
  
-   Todos los almacenes de atributos personalizados deben compilarse sobre .NET 4.0 o superior.  
  
Cuando se trabaja con un almacén de atributos personalizados, es posible que deba configurar una cadena de conexión. En ese caso, puede escribir un código personalizado de su elección que habilita una conexión a su almacén de atributos personalizados. La cadena de conexión en esta situación es un conjunto de nombre\/valor pares que se interpretan como implementados por el desarrollador del almacén de atributos personalizados. Para obtener más información sobre el desarrollo y uso de almacenes de atributos personalizados, consulte [Introducción a Store los atributos](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisitos de la aplicación  
AD FS admite notificaciones\-compatible con aplicaciones que usan los siguientes protocolos:  
  
-   WS\-federación  
  
-   WS\-de confianza  
  
-   Protocolo de SAML 2.0 mediante perfiles IDPLite, SPLite & eGov1.5.  
  
-   Perfil de concesión de autorización de OAuth 2.0  
  
AD FS también admite la autenticación y autorización para cualquiera que no sean\-notificaciones\-compatible con aplicaciones que son compatibles con el Proxy de aplicación Web.  
  
## <a name="BKMK_10"></a>Requisitos de autenticación  
**Autenticación de AD DS \(autenticación principal\)**  
  
Para el acceso a la intranet, se admiten los siguientes mecanismos de autenticación estándar para AD DS:  
  
-   Autenticación integrada de Windows mediante negociación de Kerberos y NTLM  
  
-   Autenticación mediante nombre de usuario de formularios\/contraseñas  
  
-   Autenticación de certificado mediante los certificados que se asignan a las cuentas de usuario en AD DS  
  
Para el acceso de extranet, se admiten los siguientes mecanismos de autenticación:  
  
-   Autenticación mediante nombre de usuario de formularios\/contraseñas  
  
-   Autenticación de certificado mediante los certificados que se asignan a las cuentas de usuario en AD DS  
  
-   Autenticación integrada de Windows utiliza Negotiate \(solo NTLM\) para WS\-confiar en los puntos de conexión que aceptan la autenticación integrada de Windows.  
  
Para la autenticación de certificado:  
  
-   Se extiende a las tarjetas inteligentes que pueden estar protegido de pin.  
  
-   La interfaz gráfica de usuario para el usuario que escriba su pin no se proporciona por AD FS y es necesario para formar parte del sistema operativo de cliente que se muestra cuando se usa TLS de cliente.  
  
-   El lector y el proveedor de servicios criptográficos \(CSP\) para la tarjeta inteligente debe trabajar en el equipo donde está ubicado el explorador.  
  
-   El certificado de tarjeta inteligente debe encadenarse a una raíz de confianza en todos los servidores de AD FS y servidores Proxy de aplicación Web.  
  
-   El certificado debe asignarse a la cuenta de usuario de AD DS mediante uno de los siguientes métodos:  
  
    -   El nombre del firmante del certificado se corresponde con el nombre distintivo de LDAP de una cuenta de usuario en AD DS.  
  
    -   El certificado extensión NombreAlt del tiene el nombre principal de usuario \(UPN\) de una cuenta de usuario en AD DS.  
  
Uso de Kerberos en la intranet, para la autenticación integrada de Windows  
  
-   Es necesario para el nombre del servicio formar parte de los sitios de confianza o los sitios de Intranet Local.  
  
-   Además, el HOST\/< adfs\_servicio\_nombre > se debe establecer el SPN en la cuenta de servicio que se ejecuta la granja de servidores de AD FS.  
  
**Múltiples\-Factor de autenticación**  
  
AD FS admite la autenticación adicional \(más allá de la autenticación principal compatible con AD DS\) mediante un modelo de proveedor mediante el cual los proveedores\/los clientes pueden crear su propios multi\-adaptador de autenticación de factor que un administrador puede registrar y usar durante el inicio de sesión.  
  
Cada adaptador de MFA debe se basa en .NET 4.5.  
  
Para obtener más información sobre MFA, consulte [administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticación de dispositivos**  
  
AD FS admite la autenticación de dispositivos mediante certificados aprovisionados por el servicio de registro de dispositivos durante la acción de un usuario final al área de trabajo unir su dispositivo.  
  
## <a name="BKMK_11"></a>Requisitos de unión al área de trabajo  
Los usuarios finales pueden unión sus dispositivos a una organización con AD FS. Esto es compatible con el servicio de registro de dispositivos en AD FS. Como resultado, los usuarios finales obtienen la ventaja adicional de inicio de sesión único en todas las aplicaciones compatibles con AD FS. Además, los administradores pueden administrar el riesgo al restringir el acceso a las aplicaciones solo a los dispositivos que se han unido a la organización. A continuación son los siguientes requisitos para habilitar este escenario.  
  
-   AD FS admite la unión para Windows 8.1 e iOS 5\+ dispositivos  
  
-   Para usar la funcionalidad de unión, el esquema del bosque que se unen servidores de AD FS a debe ser Windows Server 2012 R2.  
  
-   Nombre alternativo del sujeto del certificado SSL para el servicio AD FS debe contener el enterpriseregistration de valor está seguido por el nombre Principal de usuario \(UPN\) sufijo de su organización, por ejemplo, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Requisitos de criptografía  
En la tabla siguiente proporciona información de soporte técnico de criptografía adicionales en el token de AD FS firma, cifrado de tokens\/funcionalidad de descifrado:  
  
||||  
|-|-|-|  
|**algoritmo**|**Longitudes de clave**|**Protocolos\/aplicaciones\/comentarios**|  
|TripleDES – Default 192 \(admite 192, 256\) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\# TripleDES\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Se admite el algoritmo para descifrar el token de seguridad. El token de seguridad con este algoritmo de cifrado no es compatible.|  
|AES128 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes128\-cbc|128|Se admite el algoritmo para descifrar el token de seguridad. El token de seguridad con este algoritmo de cifrado no es compatible.|  
|AES192 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes192\-cbc|192|Se admite el algoritmo para descifrar el token de seguridad. El token de seguridad con este algoritmo de cifrado no es compatible.|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Predeterminado**. Se admite el algoritmo para cifrar el token de seguridad.|  
|TripleDESKeyWrap \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-tripledes|Todos los tamaños de clave admitidos por .NET 4.0\+|Admite el algoritmo para cifrar la clave simétrica que cifra un token de seguridad.|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Admite el algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Admite el algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Admite el algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|RsaV15KeyWrap \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5|1024|Admite el algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|Default. Admite el algoritmo para cifrar la clave simétrica que cifra el token de seguridad.|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Utilizado por el servidor de AD FS en la generación de SourceId artefacto:  En este escenario, el STS usa SHA1 \(por la recomendación en el estándar SAML 2.0\) para crear un valor de 160 bits corto para el identificador de origen del artefacto.<br /><br />También usa el agente web de ADFS \(componente heredado desde el período de tiempo de 2003\) para identificar cambios en un tiempo de "última actualización" de valor para que sepa cuándo se debe actualizar la información del STS.|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Se utiliza en los casos al servidor de AD FS valida la firma de SAML AuthenticationRequest, firmar la solicitud de resolución de artefacto o de respuesta, crear token\-certificado de firma.<br /><br />En estos casos, SHA256 es el valor predeterminado, y solo se utiliza SHA1 si el socio \(confianza\) no puede admitir SHA256 y deben usar SHA1.|  
  
## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que realiza la instalación y la configuración inicial de AD FS debe tener permisos de administrador de dominio en el dominio local \(en otras palabras, el dominio al que está unido el servidor de federación a.\)  
  
## <a name="see-also"></a>Vea también  
[Guía de diseño de AD FS en Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

