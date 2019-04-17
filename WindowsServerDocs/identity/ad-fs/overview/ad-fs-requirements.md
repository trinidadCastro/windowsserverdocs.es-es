---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos de AD FS de 2016
description: "Requisitos para instalar servicios de federación de Active Directory."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS

>Se aplica a: Windows Server 2016

Estos son los requisitos para la implementación de AD FS:  
  
-   [Requisitos de certificados](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de la base de datos de configuración](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos del explorador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de la red](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permisos](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificados  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada servidor de AD FS y Proxy de aplicación Web tiene un certificado SSL para solicitudes de servicio HTTPS a los servicios de federación.  El Proxy de aplicación Web puede tener certificados SSL adicionales a las solicitudes de servicio de aplicaciones publicadas.

**Recomendación:** usan el mismo certificado SSL para todos los servidores de federación de AD FS y servidores proxy de aplicación Web. 

**Requisitos:**

Certificados SSL en los servidores de federación deben cumplir los requisitos siguientes
- Certificado es de confianza públicamente (para entornos de producción)
- Certificado contiene el valor de servidor autenticación mejorada clave (EKU)
- Certificado contiene el nombre de servicio de federación, como "fs.contoso.com" en el asunto o el asunto alternativa nombre (SAN)
- Para la autenticación de certificado de usuario en el puerto 443, el certificado contiene "certauth. \ < equipo\ de servicios de federación >", como "certauth.fs.contoso.com" en el entorno de SAN
- Registro del dispositivo o para que la autenticación moderna en los recursos locales con los clientes de versiones anteriores a Windows 10, SAN debe contener "enterpriseregistration. \ < upn suffix\ >" para cada sufijo UPN en uso en la organización.

Certificados SSL en el Proxy de aplicación Web deben cumplir los requisitos siguientes
- Si se usa el proxy para las solicitudes de proxy AD FS que usan la autenticación integrada de Windows, el certificado SSL de proxy deben ser el mismo (usa la misma clave) como el certificado SSL de servidor de federación
- Si la propiedad de AD FS "ExtendedProtectionTokenCheck" está habilitada (el valor predeterminado en AD FS), el certificado SSL de proxy debe ser el mismo (usa la misma clave) como el certificado SSL de servidor de federación
- De lo contrario, los requisitos para el certificado SSL de proxy son los mismos que para el certificado SSL de servidor de federación

### <a name="service-communication-certificate"></a>Certificado de comunicación de servicio
Este certificado no es necesario para la mayoría de los escenarios AD FS incluidos Azure AD y Office 365. De manera predeterminada, AD FS configura el certificado SSL proporcionado en la configuración inicial que el certificado de comunicación de servicio.

**Recomendación:**
- Usar el mismo certificado como parte del uso de SSL.  

### <a name="token-signing-certificate"></a>Certificado de firma de token
Este certificado forma tokens de inicio de sesión usadas emitido para usuarios de confianza, para aplicaciones de terceros de confianza deben reconocer el certificado y asociados clave como conocida y de confianza. Cuando el token cambios certificado firma, como cuando que expire y configura un nuevo certificado, se deben actualizar todos los usuarios de confianza.

**Recomendación:** utilice el AD FS generado de forma predeterminada, internamente, los certificados de firma de token autofirmado.  

**Requisitos:**
- Si la organización requiere que los certificados de la PKI de empresa puede usarse para firmar token, esto puede hacerse con el parámetro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Si va a usa los certificados internamente generado de forma predeterminada o externamente inscritos certificados, cuando se cambia el certificado de firma de token deben asegurarte de todos los usuarios de confianza se actualizan con la nueva información de certificado.  De lo contrario, se producirá un error en los inicios de sesión a los usuarios de confianza no actualizados.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de cifrado y descifrado de token
Este certificado es usado por los proveedores de notificaciones que cifrar tokens emitidos a AD FS.

**Recomendación:** usar los AD FS generado de forma predeterminada, internamente, autofirmado token de descifrado de certificados.  

**Requisitos:**
- Si la organización requiere que los certificados de la PKI de empresa puede usarse para firmar token, esto puede hacerse con el parámetro DecryptingCertificateThumbprint del cmdlet Install-AdfsFarm.
- Si usas los certificados internamente generado de forma predeterminada o externamente inscritos certificados, cuando se cambia el token de descifrado de certificado deben asegurarte de todos los proveedores de notificaciones se actualizan con la nueva información de certificado.  De lo contrario, los inicios de sesión usando cualquiera dice proveedores no actualizados se producirá un error.
  
> [!CAUTION]  
> Certificados que se usan para firmar token\ y token\-decrypting\ o cifrar son esenciales para la estabilidad del servicio de federación. Los clientes administrar sus propios certificados de firma de token\ & token\-decrypting\ o cifrar deben asegurarse de que estos certificados están disponibles independientemente durante un evento de recuperación o copian de seguridad.  

### <a name="user-certificates"></a>Certificados de usuario
- Al usar x509 debe encadenas autenticación de certificado de usuario con AD FS, todos los certificados de usuario a una entidad de certificación de raíz es de confianza para los servidores Proxy de aplicación Web y de AD FS.

## <a name="BKMK_2"></a>Requisitos de hardware  
Requisitos de hardware de AD FS y Proxy de aplicación Web (físicos o virtuales) son por puertas de enlace en la CPU, por lo que debe ajustar el tamaño de la granja de servidores para la capacidad de procesamiento.  
- Usa el [hoja de cálculo de diseño de la capacidad de 2016 de AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar el número de servidores de AD FS y Proxy Web de la aplicación necesitarás.

Los requisitos de memoria y el disco de AD FS son relativamente estáticos, consulta la siguiente tabla:


|**Requisitos de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espacio en disco|32 GB|100 GB |

**Requisitos de Hardware SQL Server**

Si estás usando SQL Server para la base de datos de configuración de AD FS, el tamaño de SQL Server según las recomendaciones de SQL Server más básicas.  El tamaño de la base de datos de AD FS es muy pequeño y AD FS no colocar una carga de procesamiento considerable en la instancia de base de datos.  AD FS, sin embargo, conéctate a la base de datos varias veces durante una autenticación, para que la conexión de red debe ser sólida.  Desafortunadamente, no se admite SQL Azure para la base de datos de configuración de AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Obtener acceso a la extranet, debes implementar el servicio de rol de Proxy de aplicación Web \ - parte del rol de servidor de acceso remoto. 

-   Servidores proxy de terceros deben admitir la [protocolo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) a soporte técnico como un proxy de AD FS.  Para obtener una lista de la parte 3 de proveedores de ven el [preguntas más frecuentes sobre](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 requiere servidores Proxy de aplicación Web en Windows Server 2016.  Un proxy de nivel inferior no se puede configurar para una granja de AD FS 2016 que se ejecuta en el nivel de comportamiento de la granja de 2016.
  
-   No se puede instalar un servidor de federación y el servicio de rol de Proxy de aplicación Web en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
- AD FS requiere controladores de dominio que ejecutan Windows Server 2008 o posterior.

- Al menos un controlador de dominio de Windows Server 2016 es necesario para Microsoft Passport for Work.
  
> [!NOTE]  
> Toda la ayuda para entornos con controladores de dominio de Windows Server 2003 ha finalizado. Visita [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener más información sobre el ciclo de vida de soporte técnico de Microsoft.  
  
**Requisitos de nivel de functional\ de dominio**  
  
 - Todos los dominios de cuentas de usuario y el dominio al que se unen los servidores de AD FS deben funcionar en el nivel funcional de dominio de Windows Server 2003 o posterior.  
  
 - Un dominio de Windows Server 2008 funcional nivel o superior es necesario para la autenticación de certificado de cliente si el certificado está asignado explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquema**  
  
-   Las nuevas instalaciones de AD FS 2016 requieren el esquema de Active Directory de 2016 (versión mínima 85).

-   Aumentar el nivel de comportamiento de granja de servidores de AD FS (FBL) en el nivel de 2016, requiere el esquema de Active Directory de 2016 (versión mínima 85).  

  
**Requisitos de la cuenta de servicio**  
  
-   Cualquier cuenta de dominio estándar puede usarse como una cuenta de servicio de AD FS. También se admiten las cuentas de servicio administrado de grupo. Los permisos necesarios en tiempo de ejecución se agregarán automáticamente al configurar AD FS.

-   Cuentas de servicio administrado de grupo requieren al menos un controlador de dominio que ejecutan Windows Server 2012 o superior.  La GMSA debe residir en el valor predeterminado "CN = contenedor de cuentas de servicio administradas.  

-   Para la autenticación de Kerberos, el nombre de entidad de seguridad de servicio '`HOST/<adfs\_service\_name>`' debe registrarse en la cuenta de servicio de AD FS. De manera predeterminada, AD FS configurar esto al crear un nuevo conjunto de AD FS.  Si se produce un error, como en el caso de una colisión o permisos suficientes, verás una advertencia y debe agregar manualmente.  
   
**Requisitos de dominio.**  
  
-   Todos los servidores de AD FS deben ser un han unido a un dominio de AD DS.  
  
-   Todos los servidores de AD FS dentro de una granja de servidores deben implementarse en el mismo dominio.  
   
**Requisitos de bosques múltiples**  
  
-   Debe confiar en el dominio al que se unen los servidores de AD FS cada dominio o bosque que contiene los usuarios autenticarse en el servicio de AD FS.  

-   El bosque, que la cuenta de servicio de AD FS es un miembro de, debe confiar en todos los bosques de inicio de sesión de usuario. 
  
-   La cuenta de servicio de AD FS debe tener permisos para leer los atributos de usuario en cada dominio que contiene los usuarios autenticarse en el servicio de AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de la base de datos de configuración  
Esta sección describe los requisitos y restricciones para conjuntos de AD FS que utilizan respectivamente el Windows Internal Database (WID) o SQL Server como la base de datos:  
  
**WID**  
  
-   El perfil de resolución artefacto de SAML 2.0 no se admite en una granja de servidores WID.    

-   Detección de reproducción token no es compatible una granja de servidores WID. (Esta funcionalidad solo se usa únicamente en escenarios donde es que actúa como el proveedor de federación y consumir tokens de seguridad de proveedores externos reclamaciones AD FS).  
  
La siguiente tabla proporciona un resumen de los servidores de AD FS cuántas son compatibles con un vs WID una granja de SQL Server.    
  
  
|| 1 - 100 confiar confianzas de terceros (punto de reunión) configuradas en AD FS | Configurar más de 100 confianzas de punto de reunión  |
| --- |--- | --- |
|Servidores de 1-30 AD FS|WID compatible|No se admite con WID - necesarios de SQL Server |
|Servidores de más de 30 AD FS|No se admite con WID - necesarios de SQL Server|No se admite con WID - necesarios de SQL Server  
  
**SQL Server**  
  
- De AD FS en Windows Server 2016, SQL Server 2008 y versiones posteriores son compatibles.

- Resolución de artefacto SAML y detección de reproducción token se admiten en un conjunto de SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos del explorador  
Cuando se realiza la autenticación de AD FS a través de un explorador o un control de explorador, el explorador debe cumplir los siguientes requisitos:  
  
-   JavaScript debe estar habilitado  
  
-   Para el inicio de sesión único, el explorador del cliente debe estar configurado para permitir cookies  
  
-   Servidor indicación del nombre \(SNI\) debe ser compatible  
  
-   Autenticación de certificado de usuario certificado y dispositivo, el explorador debe admitir la autenticación de certificado de cliente SSL  

-   Para el inicio de sesión transparente sobre el uso de autenticación integrada de Windows, el nombre de servicio de federación (por ejemplo, https:\/\/fs.contoso.com) debe estar configurado en zona de intranet local o sitios de confianza.
## <a name="BKMK_7"></a>Requisitos de la red  
 
**Requisitos del Firewall**  
  
Encuentra entre el Proxy de aplicación Web y de la granja de servidores de federación de firewall y el firewall entre los clientes y el Proxy de aplicación Web deben tener el puerto TCP 443 habilitado entrante.  
  
Además, si la autenticación de certificados de usuario de cliente \ (autenticación clientTLS con X509 certificates\ del usuario) es necesario y el extremo de certauth en el puerto 443 no está habilitado, AD FS 2016 requiere que esté habilitado el puerto TCP 49443 entrante en el firewall entre los clientes y el Proxy de aplicación Web. Esto no es necesario en el firewall entre el Proxy de aplicación Web y la servidores\ federación). 

Para obtener más información en el puerto híbrido consulta requisitos [protocolos y puertos de identidad híbrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obtener más información, consulta [los procedimientos recomendados para proteger los servicios de federación de Active Directory](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Obtener acceso a la intranet, tener acceso al servicio de AD FS dentro de la red corporativa interna de \(intranet\) de todos los clientes deben poder resolver el nombre de servicio de AD FS en el equilibrado de carga de los servidores de AD FS o del servidor de AD FS.  
  
-   Obtener acceso a la extranet, todos los clientes acceso a servicios de AD FS desde fuera de la red corporativa de \(extranet\/internet\) deben poder resolver el nombre de servicio de AD FS en el equilibrado de carga de los servidores Proxy de aplicación Web o el servidor Proxy de aplicación Web.  
  
-   Cada servidor Proxy de aplicación Web en la DMZ debe ser capaz de resolver el nombre de servicio de AD FS para el equilibrado de carga de los servidores de AD FS o del servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo de la red DMZ o cambiando la resolución de servidor local con el archivo de HOSTS.  
  
-   Para la autenticación integrada de Windows, debes usar un DNS A grabar \(not CNAME\) para el nombre de servicio de federación.  

-   Para la autenticación de certificados de usuario en el puerto 443 "certauth. \ < equipo\ de servicios de federación >" debe estar configurada en DNS para resolver en el servidor de federación o proxy de aplicación web.

-   Registro del dispositivo o para que la autenticación moderna en los recursos locales con los clientes de versiones anteriores a Windows 10, "enterpriseregistration. \ < upn suffix\ >", para cada sufijo UPN en uso en la organización, debe configurarse para resolver al servidor de federación o proxy de aplicación web.

**Requisitos de equilibrado de carga**
- El equilibrado de carga no debe finalizar SSL. AD FS admite varios casos de uso con la autenticación de certificado que se interrumpa cuando finalice SSL. No se admite termina SSL en el equilibrado de carga para cualquier caso de uso. 
- Se recomienda usar una que admita SNI equilibrado de carga. En el caso no es así, mediante el enlace de reserva de 0.0.0.0 en tu AD FS / servidor Proxy Web de la aplicación debe proporcionar una solución alternativa.
- Se recomienda usar los extremos de sondeo del estado HTTP (no HTTPS) para realizar comprobaciones de estado de carga equilibrado para enrutar el tráfico. Esto evita los problemas relacionados con SNI. La respuesta a los extremos de sondeo es HTTP 200 OK y sirve localmente sin ninguna dependencia en los servicios back-end. El sondeo HTTP puede tener acceso a través de HTTP con la ruta de acceso '/ adfs/sondeo'
    - http://&lt;nombre del Proxy de aplicación Web&gt;/adfs/sondeo
    - http://&lt;nombre del servidor ADFS&gt;/adfs/sondeo
    - http://&lt;dirección IP de Proxy de aplicación Web&gt;/adfs/sondeo
    - http://&lt;dirección IP ADFS&gt;/adfs/sondeo
- NO se recomienda usar DNS por turnos como una forma de equilibrio de carga. Usar este tipo de equilibrio de carga no proporciona una manera automatizada para quitar un nodo de la equilibrado de carga con comprobaciones de estado. 
- NO se recomienda usar afinidad IP en función de la sesión o sesiones permanentes para el tráfico de autenticación de AD FS dentro de la carga de equilibrado. Esto puede provocar una sobrecarga de determinados nodos al usar el protocolo de autenticación heredados para clientes de correo electrónico para conectarse a servicios de correo de Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que realiza la instalación y la configuración inicial de AD FS debe tener permisos de administrador local en el servidor de AD FS.  Si el administrador local no tiene permisos para crear objetos de Active Directory, primero debes tener un administrador de dominio crear los objetos necesarios de anuncios y, luego, configurar el conjunto de AD FS con el parámetro AdminConfiguration.  
  
  

