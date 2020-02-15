---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos de AD FS de 2016
description: Requisitos para instalar los Servicios de federación de Active Directory.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c8ab160699bc6a961f4fbed6c58cf072a395a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407426"
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS



Los siguientes son los requisitos para implementar AD FS:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de servidores proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de bases de datos de configuración](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos de exploradores](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de red](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permisos](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificados  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada servidor de AD FS y Proxy de aplicación web tiene un certificado SSL para atender las solicitudes HTTPS al servicio de federación.  El Proxy de aplicación web puede tener certificados SSL adicionales para atender las solicitudes a las aplicaciones publicadas.

**Recomendación:** Usa el mismo certificado SSL para todos los servidores de federación de AD FS y de Proxy de aplicación web. 

**Requisitos:**

Los certificados SSL en servidores de federación tienen que cumplir los siguientes requisitos:
- El certificado es de confianza pública (para implementaciones de producción).
- El certificado contiene el valor de Uso mejorado de clave (EKU) de autenticación de servidores.
- El certificado contiene el nombre del servicio de federación, por ejemplo, "fs.contoso.com" en el Firmante o Nombre alternativo del firmante (SAN).
- Para la autenticación de certificados de usuario en el puerto 443, el certificado contiene "certauth.\<nombre del servicio de federación\>", por ejemplo: "certauth.fs.contoso.com"en el SAN.
- Para el registro de dispositivos o la autenticación moderna en recursos locales mediante clientes anteriores a Windows 10, el SAN debe contener "enterpriseregistration.\<sufijo upn\>"para cada sufijo UPN en uso en tu organización.

Los certificados SSL en el Proxy de aplicación web tienen que cumplir los siguientes requisitos:
- Si el proxy se usa para redirigir mediante proxy las solicitudes de AD FS que usan la autenticación integrada de Windows, el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de federación.
- Si la propiedad "ExtendedProtectionTokenCheck" de AD FS está habilitada (la configuración predeterminada en AD FS), el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de federación.
- De lo contrario, los requisitos para el certificado SSL del proxy son los mismos que para el certificado SSL del servidor de federación.

### <a name="service-communication-certificate"></a>Certificado de comunicación de servicios
Este certificado no es necesario para la mayoría de los escenarios de AD FS, incluidos Azure AD y Office 365. De manera predeterminada, AD FS configura el certificado SSL que se proporciona en la configuración inicial como certificado de comunicación de servicios.

**Recomendación:**
- Usa el mismo certificado que usas para SSL.  

### <a name="token-signing-certificate"></a>Certificado de firma de tokens
Este certificado se usa para firmar tokens emitidos para usuarios de confianza, por lo que las aplicaciones de usuario de confianza deben reconocer el certificado y su clave asociada como conocida y de confianza. Cuando cambia el certificado de firma de tokens, como cuando expira y configuras un nuevo certificado, se deben actualizar todos los usuarios de confianza.

**Recomendación:** Usa los certificados de firma de tokens de AD FS predeterminados, autofirmados y generados internamente.  

**Requisitos:**
- Si tu organización requiere que se usen certificados de la PKI de empresa para la firma de tokens, esto se puede hacer mediante el parámetro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Tanto si usas los certificados predeterminados generados internamente como los certificados inscritos externamente, cuando cambia el certificado de firma de tokens, tienes que asegurarte de que todos los usuarios de confianza se actualicen con la información del nuevo certificado.  De lo contrario, se producirá un error en los inicios de sesión en todos los usuarios de confianza no actualizados.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de cifrado y descifrado de tokens
Los proveedores de notificaciones que cifran los tokens emitidos para AD FS usan este certificado.

**Recomendación:** Usa los certificados de descifrado de tokens de AD FS predeterminados, autofirmados y generados internamente.  

**Requisitos:**
- Si tu organización requiere que se usen certificados de la PKI de empresa para la firma de tokens, esto se puede hacer mediante el parámetro DecryptingCertificateThumbprint del cmdlet Install-AdfsFarm.
- Tanto si usas los certificados predeterminados generados internamente como los certificados inscritos externamente, cuando cambia el certificado de descifrado de tokens, tienes que asegurarte de que todos los proveedores de notificaciones se actualicen con la información del nuevo certificado.  De lo contrario, se producirá un error en los inicios de sesión con los proveedores de notificaciones no actualizados.
  
> [!CAUTION]  
> Los certificados que se usan para la firma de tokens y el descifrado\/cifrado de tokens son cruciales para la estabilidad del servicio de federación. Los clientes que administran sus propios certificados de firma de tokens y de descifrado\/cifrado de tokens deben asegurarse de que estos certificados tengan una copia de seguridad y estén disponibles de forma independiente durante un evento de recuperación.  

### <a name="user-certificates"></a>Certificados de usuario
- Al usar la autenticación de certificados de usuario X509 con AD FS, todos los certificados de usuario deben encadenarse a una entidad de certificación raíz que sea de confianza para los servidores de AD FS y de Proxy de aplicación web.

## <a name="BKMK_2"></a>Requisitos de hardware  
Los requisitos de hardware (físico o virtual) de AD FS y Proxy de aplicación web dependen de la CPU, por lo que debes ajustar el tamaño de la granja para la capacidad de procesamiento.  
- Usa la [hoja de cálculo de planeamiento de capacidad de AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar el número de servidores de AD FS y de Proxy de aplicación web que necesitarás.

Los requisitos de memoria y disco para AD FS son bastante estáticos, consulta la tabla siguiente:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espacio en disco|32 GB|100 GB |

**Requisitos de hardware de SQL Server**

Si usas SQL Server para la base de datos de configuración de AD FS, ajusta el tamaño de SQL Server según las recomendaciones de SQL Server más básicas.  El tamaño de la base de datos de AD FS es muy pequeño, y AD FS no coloca una carga de procesamiento significativa en la instancia de bases de datos.  Sin embargo, AD FS se conecta a la base de datos varias veces durante una autenticación, por lo que la conexión de red debe ser sólida.  Desafortunadamente, SQL Azure no es compatible con la base de datos de configuración de AD FS.
  
## <a name="BKMK_3"></a>Requisitos de servidores proxy  
  
-   Para el acceso desde una extranet, tienes que implementar el servicio de rol de Proxy de aplicación web, parte del rol de servidor de Acceso remoto. 

-   Los servidores proxy de terceros deben admitir el protocolo [MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para que se admitan como proxy de AD FS.  Para obtener una lista de terceros proveedores, consulta las [preguntas más frecuentes](AD-FS-FAQ.md).

-   AD FS 2016 requiere servidores de Proxy de aplicación web en Windows Server 2016.  No se puede configurar un proxy de nivel inferior para una granja de AD FS 2016 que se ejecute en el nivel de comportamiento de la granja de 2016.
  
-   No se puede instalar un servidor de federación y el servicio de rol de Proxy de aplicación web en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
- AD FS requiere controladores de dominio que se ejecuten en Windows Server 2008 o posterior.

- Se requiere al menos un controlador de dominio de Windows Server 2016 para Microsoft Passport for Work.
  
> [!NOTE]  
> Ha finalizado todo el soporte técnico para los entornos con controladores de dominio de Windows Server 2003. Visita [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener información adicional sobre el ciclo de vida del Soporte técnico de Microsoft.  
  
**Requisitos de nivel funcional de dominio**  
  
 - Todos los dominios de cuentas de usuario y el dominio al cual se unen los servidores de AD FS deben funcionar en el nivel funcional de dominio de Windows Server 2003 o posterior.  
  
 - Se requiere un nivel funcional de dominio de Windows Server 2008 o superior para la autenticación del certificado de cliente, si dicho certificado está asignado explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquemas**  
  
-   Las nuevas instalaciones de AD FS 2016 requieren el esquema de Active Directory 2016 (versión mínima 85).

-   La elevación del nivel de comportamiento de la granja (FBL) de AD FS al nivel 2016 requiere el esquema de Active Directory 2016 (versión mínima 85).  

  
**Requisitos de cuentas de servicio**  
  
-   Se puede usar cualquier cuenta de dominio estándar como cuenta de servicio para AD FS. También se admiten cuentas de servicio administradas de grupo. Los permisos necesarios en tiempo de ejecución se agregarán automáticamente al configurar AD FS.

-   La asignación de derechos de usuario necesaria para la cuenta de servicio de AD es "Iniciar sesión como servicio".

-   Las asignaciones de derechos de usuario necesarias para "NT Service\adfssrv" y "NT Service\drs" son "Generar auditorías de seguridad" e "Iniciar sesión como servicio".

-   Las cuentas de servicio administradas de grupo requieren al menos un controlador de dominio que ejecute Windows Server 2012 o posterior.  GMSA debe residir en el contenedor "CN = Managed Service Accounts" predeterminado.  

-   Para la autenticación de Kerberos, el nombre de entidad de seguridad de servicio "`HOST/<adfs\_service\_name>`" debe estar registrado en la cuenta de servicio de AD FS. De forma predeterminada, AD FS lo configurará al crear una nueva granja de AD FS.  Si se produce un error, como en el caso de una colisión o permisos insuficientes, verás una advertencia y deberás agregarlo manualmente.  
   
**Requisitos de dominio**  
  
-   Todos los servidores de AD FS deben estar unidos a un dominio AD DS.  
  
-   Todos los servidores de AD FS de una granja deben implementarse en el mismo dominio.  
   
**Requisitos de varios bosques**  
  
-   El dominio al que se unan los servidores de AD FS debe confiar en cada dominio o bosque que contenga usuarios que se autentiquen en el servicio AD FS.  

-   El bosque del que es miembro la cuenta de servicio de AD FS debe confiar en todos los bosques de inicio de sesión de los usuarios. 
  
-   La cuenta de servicio de AD FS debe tener permisos para leer los atributos de usuario en todos los dominios que contengan usuarios que se autentiquen en el servicio de AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de bases de datos de configuración  
En esta sección se describen los requisitos y las restricciones de las granjas de AD FS que usan respectivamente Windows Internal Database (WID) o SQL Server como base de datos:  
  
**WID**  
  
-   El perfil de resolución de artefactos de SAML 2.0 no se admite en una granja de WID.    

-   No se admite la detección de reproducción de tokens en una granja WID. (Esta funcionalidad solo se usa en escenarios donde AD FS funciona como proveedor de federación y consume tokens de seguridad de proveedores de notificaciones externos).  
  
En la tabla siguiente se proporciona un resumen del número de servidores de AD FS que se admiten en una granja de WID frente a una de SQL Server.    
  
  
|| De 1 a 100 relaciones de confianza de usuarios de confianza (UC) configuradas en AD FS | Más de 100 relaciones de confianza de UC configuradas  |
| --- |--- | --- |
|De 1 a 30 servidores de AD FS|Compatible con WID|No se admite con WID, se requiere SQL Server |
|Más de 30 servidores de AD FS|No se admite con WID, se requiere SQL Server|No se admite con WID, se requiere SQL Server  
  
**SQL Server**  
  
- Para AD FS en Windows Server 2016, se admiten SQL Server 2008 y versiones posteriores.

- Se admite tanto la resolución de artefactos SAML como la detección de reproducción de tokens en una granja de SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos de exploradores  
Cuando la autenticación de AD FS se hace a través de un explorador o un control de explorador, el explorador tiene que cumplir los siguientes requisitos:  
  
- JavaScript debe estar habilitado.  
  
- Para el inicio de sesión único, el explorador cliente tiene que estar configurado para permitir cookies.  
  
- Se debe admitir Indicación de nombre de servidor \(SNI\).  
  
- Para la autenticación de certificados de usuario y certificados de dispositivo, el explorador tiene que admitir la autenticación de certificados de cliente SSL.  

- Para un inicio de sesión sin problemas con la Autenticación integrada de Windows, el nombre del servicio de federación (como https:\/\/fs.contoso.com) debe estar configurado en la zona de intranet local o en la zona de sitios de confianza.
  ## <a name="BKMK_7"></a>Requisitos de red  
 
**Requisitos de firewall**  
  
Tanto el firewall que se encuentra entre el Proxy de aplicación web y la granja de servidores de federación como el firewall entre los clientes y el Proxy de aplicación web deben tener el puerto 443 TCP habilitado para la entrada.  
  
Además, si se requiere la autenticación de certificados de usuario de cliente \(autenticación de clientTLS mediante certificados de usuario X509\) y el punto de conexión certauth en el puerto 443 no está habilitado, AD FS 2016 requiere que el puerto 49443 TCP esté habilitado en el firewall entre los clientes y el Proxy de aplicación web. (Esto no es necesario en el firewall entre el Proxy de aplicación web y los servidores de federación\). 

Para obtener información adicional sobre los requisitos de puerto híbrido, consulta [Puertos y protocolos requeridos para Identidad híbrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obtener más información, consulta [Procedimientos recomendados para proteger Servicios de federación de Active Directory (AD FS)](../deployment/Best-Practices-Securing-AD-FS.md).
  
**Requisitos de DNS**  
  
-   Para el acceso desde una intranet, todos los clientes que accedan al servicio de AD FS dentro de la red corporativa interna \(intranet\) deben poder resolver el nombre del servicio de AD FS en el equilibrador de carga para el o los servidores de AD FS.  
  
-   Para el acceso desde una extranet, todos los clientes que accedan al servicio de AD FS desde fuera de la red corporativa \(extranet\/Intranet\) deben poder resolver el nombre del servicio de AD FS en el equilibrador de carga para el o los servidores del Proxy de aplicación web.  
  
-   Cada servidor de Proxy de aplicación web de la red perimetral debe ser capaz de resolver el nombre de servicio de AD FS en el equilibrador de carga para el o los servidores de AD FS. Esto puede lograrse mediante un servidor DNS alternativo en la red perimetral o cambiando la resolución del servidor local mediante el archivo HOSTS.  
  
-   Para la Autenticación integrada de Windows, tienes que usar un registro A de DNS \(no CNAME\) para el nombre del servicio de federación.  

-   Para la autenticación de certificados de usuario en el puerto 443, "certauth.\<nombre del servicio de federación\>" debe estar configurado en DNS para resolver el servidor de federación o el Proxy de aplicación web.

-   Para el registro de dispositivos o la autenticación moderna en recursos locales mediante clientes anteriores a Windows 10, "enterpriseregistration.\<sufijo upn\>", para cada sufijo UPN en uso en tu organización, debe estar configurado para resolver el servidor de federación o el Proxy de aplicación web.

**Requisitos del equilibrador de carga**
- El equilibrador de carga NO DEBE finalizar SSL. AD FS admite varios casos de uso con autenticación de certificados que se interrumpirán al finalizar SSL. No se admite la finalización de SSL en el equilibrador de carga en ningún caso de uso. 
- Se recomienda usar un equilibrador de carga que admita SNI. En caso contrario, el uso del enlace de reserva 0.0.0.0 en el servidor de AD FS/Proxy de aplicación web debería ofrecer una solución alternativa.
- Se recomienda usar los puntos de conexión de sondeo de mantenimiento de HTTP (no HTTPS) para hacer comprobaciones de estado del equilibrador de carga para el enrutamiento del tráfico. Esto evita cualquier problema relacionado con SNI. La respuesta a estos puntos de conexión de sondeo es un estado HTTP 200 - Correcto y se atiende localmente sin depender de los servicios de back-end. Se puede acceder al sondeo HTTP a través de HTTP mediante la ruta de acceso "/ADFS/PROBE".
    - http://&lt;nombre del Proxy de aplicación web&gt;/adfs/probe
    - http://&lt;nombre del servidor de ADFS&gt;/adfs/probe
    - http://&lt;dirección IP del Proxy de aplicación web&gt;/adfs/probe
    - http://&lt;dirección IP de ADFS&gt;/adfs/probe
- NO se recomienda usar round robin de DNS como forma de equilibrar la carga. El uso de este tipo de equilibrio de carga no proporciona una manera automatizada de quitar un nodo del equilibrador de carga mediante sondeos de estado. 
- NO se recomienda usar la afinidad de sesión basada en IP ni sesiones permanentes para el tráfico de autenticación a AD FS dentro del equilibrador de carga. Esto puede producir una sobrecarga de ciertos nodos cuando se usa el protocolo de autenticación heredado para que los clientes de correo se conecten a los servicios de correo de Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que se encarga de la instalación y la configuración inicial de AD FS debe tener permisos de administrador local en el servidor de AD FS.  Si el administrador local no tiene permisos para crear objetos en Active Directory, primero debe hacer que un administrador de dominio cree los objetos de AD necesarios y, luego, configure la granja de AD FS con el parámetro AdminConfiguration.  
  
  

