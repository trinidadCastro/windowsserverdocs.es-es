---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos de AD FS de 2016
description: Requisitos para instalar Servicios de federación de Active Directory (AD FS).
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c330b5f65b2862628fd23e288c95e81653da5c5b
ms.sourcegitcommit: 4fa147d552481d8279a5390f458a9f7788061977
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70009078"
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS



A continuación se indican los requisitos para implementar AD FS:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de la base de datos de configuración](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos del explorador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de red](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permisos](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificado  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada AD FS y el servidor proxy de aplicación web tiene un certificado SSL para atender las solicitudes HTTPS al servicio de Federación.  El proxy de aplicación web puede tener certificados SSL adicionales para atender las solicitudes a las aplicaciones publicadas.

**Norma** Use el mismo certificado SSL para todos los servidores de Federación de AD FS y proxy de aplicación Web. 

**Requisitos:**

Los certificados SSL en los servidores de Federación deben cumplir los requisitos siguientes:
- El certificado es de confianza pública (para implementaciones de producción)
- El certificado contiene el valor de uso mejorado de clave (EKU) de autenticación de servidor
- El certificado contiene el nombre del servicio de Federación, como "fs.contoso.com" en el asunto o el nombre alternativo del firmante (SAN).
- Para la autenticación de certificados de usuario en el puerto 443, el certificado contiene "certauth. \<nombre\>del servicio de Federación ", como" certauth.FS.contoso.com "en la red San
- Para el registro de dispositivos o la autenticación moderna en recursos locales mediante clientes anteriores a Windows 10, la SAN debe contener "enterpriseregistration. \<sufijo\>UPN "para cada sufijo UPN en uso en su organización.

Los certificados SSL en el proxy de aplicación web deben cumplir los requisitos siguientes:
- Si el proxy se usa para AD FS solicitudes de proxy que usan la autenticación integrada de Windows, el certificado SSL de proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de Federación.
- Si está habilitada la propiedad AD FS "ExtendedProtectionTokenCheck" (la configuración predeterminada en AD FS), el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de Federación.
- De lo contrario, los requisitos para el certificado SSL de proxy son los mismos que para el certificado SSL del servidor de Federación.

### <a name="service-communication-certificate"></a>Certificado de comunicación de servicio
Este certificado no es necesario para la mayoría de los escenarios de AD FS, incluidos Azure AD y Office 365. De forma predeterminada, AD FS configura el certificado SSL que se proporciona en la configuración inicial como certificado de comunicación de servicio.

**Norma**
- Use el mismo certificado que usa para SSL.  

### <a name="token-signing-certificate"></a>Certificado de firma de tokens
Este certificado se usa para firmar tokens emitidos para usuarios de confianza, por lo que las aplicaciones de usuario de confianza deben reconocer el certificado y su clave asociada como conocida y de confianza. Cuando el certificado de firma de tokens cambia, como cuando expira y configura un nuevo certificado, se deben actualizar todos los usuarios de confianza.

**Norma** Use AD FS los certificados de firma de token autofirmados predeterminados y generados internamente.  

**Requisitos:**
- Si su organización requiere que se usen certificados de la PKI de empresa para la firma de tokens, esto se puede hacer mediante el parámetro SigningCertificateThumbprint del cmdlet install-AdfsFarm.
- Tanto si usa los certificados generados internamente como los certificados inscritos externamente, cuando se cambia el certificado de firma de tokens, debe asegurarse de que todos los usuarios de confianza se actualizan con la información del nuevo certificado.  De lo contrario, se producirá un error en los inicios de sesión en los usuarios de confianza no actualizados.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de cifrado y descifrado de tokens
Los proveedores de notificaciones que cifran los tokens emitidos para AD FS usan este certificado.

**Norma** Use AD FS los certificados de descifrado de token autofirmados predeterminados y generados internamente.  

**Requisitos:**
- Si su organización requiere que se usen certificados de la PKI de empresa para la firma de tokens, esto se puede hacer mediante el parámetro DecryptingCertificateThumbprint del cmdlet install-AdfsFarm.
- Tanto si usa los certificados generados internamente como los certificados inscritos externamente, cuando se cambia el certificado de descifrado de tokens, debe asegurarse de que todos los proveedores de notificaciones se actualizan con la información del nuevo certificado.  De lo contrario, se producirá un error en los inicios de sesión con proveedores de notificaciones no actualizados.
  
> [!CAUTION]  
> Los certificados que se usan para\-la firma de\-tokens y\/el cifrado de descifrado de tokens son fundamentales para la estabilidad del servicio de Federación. Los clientes que administran\-su propia firma\-de tokens\/& el descifrado de los certificados deben asegurarse de que se realiza una copia de seguridad de estos certificados y están disponibles de forma independiente durante un evento de recuperación.  

### <a name="user-certificates"></a>Certificados de usuario
- Al usar la autenticación de certificado de usuario X509 con AD FS, todos los certificados de usuario deben encadenarse a una entidad de certificación raíz que sea de confianza para los servidores de AD FS y del proxy de aplicación Web.

## <a name="BKMK_2"></a>Requisitos de hardware  
Los requisitos de hardware de AD FS y proxy de aplicación web (física o virtual) se ajustan a la CPU, por lo que debe ajustar el tamaño de la granja para la capacidad de procesamiento.  
- Use la [hoja de cálculo de planeación de capacidad de AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar el número de servidores de AD FS y proxy de aplicación web que necesitará.

Los requisitos de memoria y disco para AD FS son bastante estáticos; vea la tabla siguiente:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espacio en disco|32 GB|100 GB |

**Requisitos de hardware SQL Server**

Si usa SQL Server para la base de datos de configuración de AD FS, ajuste el tamaño de la SQL Server según las recomendaciones de SQL Server más básicas.  El tamaño de la base de datos AD FS es muy pequeño y AD FS no coloca una carga de procesamiento significativa en la instancia de base de datos.  Sin embargo, AD FS no se conectará a la base de datos varias veces durante una autenticación, por lo que la conexión de red debe ser robusta.  Desafortunadamente, SQL Azure no es compatible con la base de datos de configuración de AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para el acceso de extranet, debe implementar la parte del servicio \- de rol proxy de aplicación web del rol de servidor de acceso remoto. 

-   Los proxies de terceros deben admitir el [Protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para que se admita como proxy AD FS.  Para obtener una lista de proveedores de terceros, consulte las [preguntas más frecuentes](AD-FS-FAQ.md).

-   AD FS 2016 requiere servidores proxy de aplicación web en Windows Server 2016.  No se puede configurar un proxy de nivel inferior para una granja de AD FS 2016 que se ejecute en el nivel de comportamiento de la granja de 2016.
  
-   Un servidor de Federación y el servicio de rol de proxy de aplicación web no se pueden instalar en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
- AD FS requiere controladores de dominio que ejecuten Windows Server 2008 o posterior.

- Se requiere al menos un controlador de dominio de Windows Server 2016 para Microsoft Passport for Work.
  
> [!NOTE]  
> Ha finalizado toda la compatibilidad con entornos con controladores de dominio de Windows Server 2003. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener información adicional sobre el ciclo de vida de soporte técnico de Microsoft.  
  
**Requisitos de\-nivel funcional de dominio**  
  
 - Todos los dominios de cuentas de usuario y el dominio al que se unen los servidores AD FS deben funcionar en el nivel funcional de dominio de Windows Server 2003 o posterior.  
  
 - Se requiere un nivel funcional de dominio de Windows Server 2008 o superior para la autenticación de certificados de cliente si el certificado está asignado explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquema**  
  
-   Las nuevas instalaciones de AD FS 2016 requieren el esquema de Active Directory 2016 (versión mínima 85).

-   La elevación del nivel de comportamiento de la granja de AD FS (FBL) al nivel 2016 requiere el esquema de Active Directory 2016 (versión mínima 85).  

  
**Requisitos de la cuenta de servicio**  
  
-   Se puede usar cualquier cuenta de dominio estándar como cuenta de servicio para AD FS. También se admiten las cuentas de servicio administradas de grupo. Los permisos necesarios en tiempo de ejecución se agregarán automáticamente al configurar AD FS.

-   La asignación de derechos de usuario necesaria para la cuenta de servicio de AD es "iniciar sesión como servicio"

-   Las asignaciones de derechos de usuario necesarias para "NT Service\adfssrv" y "NT Service\drs" son "generar auditorías de seguridad" e "iniciar sesión como servicio".

-   Las cuentas de servicio administradas de grupo requieren al menos un controlador de dominio que ejecute Windows Server 2012 o posterior.  GMSA debe residir en el contenedor "CN = Managed Service Accounts" predeterminado.  

-   Para la autenticación Kerberos, el nombre de entidad`HOST/<adfs\_service\_name>`de seguridad de servicio ' ' debe estar registrado en la cuenta de servicio de AD FS. De forma predeterminada, AD FS lo configurará al crear una nueva granja de AD FS.  Si se produce un error, como en el caso de una colisión o permisos insuficientes, verá una advertencia y deberá agregarla manualmente.  
   
**Requisitos de dominio**  
  
-   Todos los servidores AD FS deben estar Unidos a un dominio AD DS.  
  
-   Todos los servidores de AD FS de una granja de servidores se deben implementar en el mismo dominio.  
   
**Requisitos de varios bosques**  
  
-   El dominio al que se unen los servidores de AD FS debe confiar en cada dominio o bosque que contenga usuarios que se autentiquen en el servicio AD FS.  

-   El bosque del que es miembro la cuenta de servicio de AD FS, debe confiar en todos los bosques de inicio de sesión de usuario. 
  
-   La cuenta de servicio de AD FS debe tener permisos para leer los atributos de usuario en todos los dominios que contengan usuarios que se autentiquen en el servicio AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de la base de datos de configuración  
En esta sección se describen los requisitos y las restricciones de las granjas de AD FS que utilizan respectivamente Windows Internal Database (WID) o SQL Server como base de datos:  
  
**WID**  
  
-   El perfil de resolución de artefactos de SAML 2,0 no se admite en una granja de WID.    

-   No se admite la detección de reproducción de tokens en una granja WID. (Esta funcionalidad solo se usa en escenarios donde AD FS actúa como proveedor de Federación y consume tokens de seguridad de proveedores de notificaciones externos).  
  
En la tabla siguiente se proporciona un resumen del número de servidores AD FS que se admiten en una granja de servidores de WID frente a SQL Server.    
  
  
|| 1-100 confianzas de usuario de confianza (RP) configuradas en AD FS | Más de 100 confianzas RP configuradas  |
| --- |--- | --- |
|1-30 servidores AD FS|WID compatible|No se admite con WID SQL Server es necesario |
|Más de 30 servidores AD FS|No se admite con WID SQL Server es necesario|No se admite con WID SQL Server es necesario  
  
**SQL Server**  
  
- Por AD FS en Windows Server 2016, se admiten SQL Server 2008 y versiones posteriores.

- Tanto la resolución de artefactos SAML como la detección de reproducción de tokens se admiten en una granja de SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos del explorador  
Cuando AD FS la autenticación se realiza a través de un explorador o un control de explorador, el explorador debe cumplir los siguientes requisitos:  
  
- JavaScript debe estar habilitado  
  
- Para el inicio de sesión único, el explorador cliente debe estar configurado para permitir cookies  
  
- Indicación de nombre de servidor \(SNI\) debe ser compatible  
  
- Para el certificado de usuario & la autenticación de certificados de dispositivo, el explorador debe ser compatible con la autenticación de certificados de cliente SSL  

- Para el inicio de sesión sin problemas con la autenticación integrada de Windows, el nombre del servicio\/de Federación (por ejemplo, https:\/FS.contoso.com) debe estar configurado en la zona de Intranet local o en la zona de sitios de confianza.
  ## <a name="BKMK_7"></a>Requisitos de red  
 
**Requisitos de Firewall**  
  
Tanto el firewall que se encuentra entre el proxy de aplicación web y la granja de servidores de Federación como el Firewall entre los clientes y el proxy de aplicación web deben tener el puerto TCP 443 habilitado de entrada.  
  
Además, si se requiere autenticación de certificado \(de usuario de cliente de clientTLS\) mediante certificados de usuario X509 y el punto de conexión certauth en el puerto 443 no está habilitado, AD FS 2016 requiere que esté habilitado el puerto TCP 49443 entrante en el Firewall entre los clientes y el proxy de aplicación Web. Esto no es necesario en el Firewall entre el proxy de aplicación web y los servidores\)de Federación. 

Para obtener información adicional sobre los requisitos de Puerto híbrido [, consulte protocolos y puertos de identidad híbridos](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obtener más información, consulte [procedimientos recomendados para proteger servicios de Federación de Active Directory (AD FS)](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Para el acceso a la intranet, todos los clientes que tienen acceso a AD FS \(servicio\) dentro de la intranet interna de la red corporativa deben poder resolver el nombre del servicio AD FS en el equilibrador de carga para los servidores de AD FS o el servidor de AD FS.  
  
-   Para el acceso de extranet, todos los clientes que tienen acceso a AD FS servicio \(desde\/fuera\) de la extranet de la red corporativa Internet deben poder resolver el nombre del servicio AD FS en el equilibrador de carga para los servidores proxy de aplicación web o el servidor proxy de aplicación Web.  
  
-   Cada servidor proxy de aplicación Web de la red perimetral debe ser capaz de resolver AD FS nombre de servicio en el equilibrador de carga para los servidores de AD FS o el servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo en la red DMZ o cambiando la resolución del servidor local mediante el archivo de hosts.  
  
-   Para la autenticación integrada de Windows, debe usar un registro \(a de DNS no CNAME\) para el nombre del servicio de Federación.  

-   Para la autenticación de certificados de usuario en el puerto 443, "certauth. \<nombre\>del servicio de Federación "se debe configurar en DNS para resolver el servidor de Federación o el proxy de aplicación Web.

-   Para el registro de dispositivos o para la autenticación moderna en recursos locales mediante clientes anteriores a Windows 10, "enterpriseregistration. \<sufijo\>UPN ", para cada sufijo UPN en uso en su organización, debe configurarse para que se resuelva en el servidor de Federación o en el proxy de aplicación Web.

**Requisitos de Load Balancer**
- El equilibrador de carga no debe finalizar SSL. AD FS admite varios casos de uso con autenticación de certificados que se interrumpirán al finalizar SSL. No se admite la terminación de SSL en el equilibrador de carga en ningún caso de uso. 
- Se recomienda usar un equilibrador de carga que admita SNI. En caso contrario, el uso del enlace de reserva 0.0.0.0 en el servidor de AD FS/Web Application proxy debe proporcionar una solución alternativa.
- Se recomienda usar los puntos de conexión de sondeo de mantenimiento de HTTP (no HTTPS) para realizar comprobaciones de estado del equilibrador de carga para el enrutamiento del tráfico. Esto evita cualquier problema relacionado con SNI. La respuesta a estos puntos de conexión de sondeo es una HTTP 200 correcta y se atiende localmente sin depender de los servicios back-end. Se puede acceder al sondeo HTTP a través de HTTP mediante la ruta de acceso '/ADFS/PROBE '
    - http://&lt;nombre&gt;del proxy de aplicación web/ADFS/Probe
    - http://&lt;nombre&gt;del servidor ADFS/ADFS/Probe
    - http://&lt;dirección&gt;IP del proxy de aplicación web/ADFS/Probe
    - http://&lt;dirección&gt;IP de ADFS/ADFS/Probe
- NO se recomienda usar round robin DNS como forma de equilibrar la carga. El uso de este tipo de equilibrio de carga no proporciona una manera automatizada de quitar un nodo del equilibrador de carga mediante sondeos de estado. 
- NO se recomienda usar la afinidad de sesión basada en IP o sesiones permanentes para el tráfico de autenticación a AD FS dentro del equilibrador de carga. Esto puede producir una sobrecarga de ciertos nodos cuando se usa el protocolo de autenticación heredado para que los clientes de correo se conecten a los servicios de correo de Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que realiza la instalación y la configuración inicial de AD FS deben tener permisos de administrador local en el servidor de AD FS.  Si el administrador local no tiene permisos para crear objetos en Active Directory, primero deben tener un administrador de dominio para crear los objetos de AD necesarios y, a continuación, configurar la granja de AD FS mediante el parámetro AdminConfiguration.  
  
  

