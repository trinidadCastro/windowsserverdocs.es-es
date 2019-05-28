---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisitos de AD FS de 2016
description: Requisitos para instalar servicios de federación de Active Directory.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 235030ea913f2fe1860efaa00bdb4641ac56750d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188684"
---
# <a name="ad-fs-requirements"></a>Requisitos de AD FS



Los siguientes son los requisitos para implementar AD FS:  
  
-   [Requisitos de certificado](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisitos de hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisitos de proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisitos de AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisitos de la base de datos de configuración](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisitos del explorador](ad-fs-requirements.md#BKMK_6)  

-   [Requisitos de red](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisitos de permisos](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisitos de certificados  
  
### <a name="ssl-certificates"></a>Certificados SSL

Cada servidor de AD FS y Proxy de aplicación Web tiene un certificado SSL para atender las solicitudes HTTPS para el servicio de federación.  El Proxy de aplicación Web puede tener los certificados SSL adicionales para atender las solicitudes a las aplicaciones publicadas.

**Recomendación:** Use el mismo certificado SSL para todos los servidores de federación de AD FS y proxy de aplicación Web. 

**Requisitos:**

Certificados SSL en los servidores de federación deben cumplir los siguientes requisitos
- Certificado es de confianza pública (para implementaciones de producción)
- Certificado contiene el valor de uso de clave mejorada de servidor autenticación (EKU)
- Certificado contiene el nombre de servicio de federación, como "fs.contoso.com" en el asunto o nombre alternativo de sujeto (SAN)
- Para la autenticación de certificado de usuario en el puerto 443, el certificado contiene "certauth. \<nombre_servicio_federación\>", por ejemplo,"certauth.fs.contoso.com"en la red SAN
- Para el registro de dispositivos o para la autenticación moderna a los recursos locales con los clientes de versiones anteriores de Windows 10, la SAN debe contener "enterpriseregistration. \<sufijo upn\>"para cada sufijo UPN en uso en su organización.

Certificados SSL en el Proxy de aplicación Web deben cumplir los siguientes requisitos
- Si se usa el proxy para las solicitudes de proxy AD FS que usan la autenticación integrada de Windows, el certificado SSL de proxy deben ser el mismo (usar la misma clave) que el certificado SSL de servidor de federación
- Si la propiedad de AD FS que es "ExtendedProtectionTokenCheck" habilitada (el valor predeterminado en AD FS), el certificado SSL de proxy debe ser el mismo (usar la misma clave) que el certificado SSL de servidor de federación
- En caso contrario, los requisitos para el certificado SSL de proxy son los mismos que para el certificado SSL de servidor de federación

### <a name="service-communication-certificate"></a>Certificado de comunicación de servicio
Este certificado no es necesario para la mayoría de los escenarios AD FS incluye Azure AD y Office 365. De forma predeterminada, AD FS configura el certificado SSL proporcionado tras la configuración inicial como el certificado de comunicación de servicio.

**Recomendación:**
- Use el mismo certificado que usa para SSL.  

### <a name="token-signing-certificate"></a>Certificado de firma de tokens
Este certificado se usa para firmar los tokens emitidos para los usuarios autenticados, por lo que las aplicaciones de confianza deben reconocer el certificado y sus asociados clave como conocida y de confianza. Cuando el token cambios de certificado firma, como cuando caduca y configuración un nuevo certificado, todos los usuarios de confianza deben actualizarse.

**Recomendación:** Utilice el valor predeterminado de AD FS, el token generado internamente, firmado automáticamente certificados de firma.  

**Requisitos:**
- Si su organización requiere que se usan certificados de la PKI de empresa para la firma de tokens, esto puede hacerse mediante el parámetro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Si va a utiliza los certificados predeterminados que se hayan generado internamente o inscritos externamente los certificados, cuando se cambia el certificado de firma de tokens deben asegurarse de todos los usuarios de confianza se actualizan con la nueva información del certificado.  En caso contrario, se producirá un error en los inicios de sesión a los usuarios de confianza no actualizados.

### <a name="token-encryptingdecrypting-certificate"></a>Certificado de cifrado/descifrado de tokens
Este certificado se usa por los proveedores de notificaciones que cifrar los tokens emitidos para AD FS.

**Recomendación:** Utilice el valor predeterminado de AD FS, el token autofirmado, generado internamente, los certificados de descifrado.  

**Requisitos:**
- Si su organización requiere que se usan certificados de la PKI de empresa para la firma de tokens, esto puede hacerse mediante el parámetro DecryptingCertificateThumbprint del cmdlet Install-AdfsFarm.
- Si va a utiliza los certificados predeterminados que se hayan generado internamente o inscritos externamente los certificados, cuando se cambia el certificado de descifrado de tokens deben asegurarse de todos los proveedores de notificaciones se actualizan con la nueva información del certificado.  En caso contrario, los inicios de sesión utilizando cualquiera no actualizados los proveedores de notificaciones se producirá un error.
  
> [!CAUTION]  
> Los certificados que se usan para el token\-token y firma\-descifrar\/cifrar son fundamentales para la estabilidad del servicio de federación. Los clientes que administran su propio token\-firma & token\-descifrar\/cifrar los certificados debe asegurarse de que estos certificados son una copia de seguridad y están disponibles por separado durante un evento de recuperación.  

### <a name="user-certificates"></a>Certificados de usuario
- Cuando se usa la autenticación de certificados de usuario con AD FS, todos los certificados de usuario debe encadenarse a una entidad de certificación raíz de confianza para los servidores de AD FS y Proxy de aplicación Web de x509.

## <a name="BKMK_2"></a>Requisitos de hardware  
Requisitos de hardware AD FS y Proxy de aplicación Web (físicos o virtuales) están controlados en CPU, por lo que debe cambiar el tamaño de la granja de servidores para la capacidad de procesamiento.  
- Use la [hoja de cálculo de planeamiento de capacidad de AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) para determinar el número de servidores de AD FS y Proxy de aplicación Web que necesitará.

Los requisitos de memoria y disco para AD FS son bastante estáticos, vea la tabla siguiente:


|**Requisito de hardware**|**Requisito mínimo**|**Requisito recomendado**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Espacio en disco|32 GB|100 GB |

**Requisitos de Hardware SQL Server**

Si usas SQL Server para la base de datos de configuración de AD FS, cambiar el tamaño de SQL Server según las recomendaciones más básicas de SQL Server.  Es muy pequeño el tamaño de la base de datos de AD FS y AD FS no colocar una carga de procesamiento significativos en la instancia de base de datos.  AD FS, sin embargo, conectarse a la base de datos varias veces durante la autenticación, por lo que la conexión de red debe ser robusta.  Por desgracia, SQL Azure no se admite para la base de datos de configuración de AD FS.
  
## <a name="BKMK_3"></a>Requisitos de proxy  
  
-   Para el acceso de extranet, debe implementar el servicio de rol Proxy de aplicación Web \- forma parte de la función de servidor de acceso remoto. 

-   Servidores proxy de terceros deben admitir la [protocolo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) formando un proxy de AD FS.  Para obtener una lista de parte 3ª ven proveedores el [preguntas más frecuentes sobre](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 necesita servidores Proxy de aplicación Web en Windows Server 2016.  No se puede configurar un proxy de nivel inferior para una granja de AD FS 2016 ejecutándose en el nivel de granja 2016 comportamiento.
  
-   No se puede instalar un servidor de federación y el servicio de rol Proxy de aplicación Web en el mismo equipo.  
  
## <a name="BKMK_4"></a>Requisitos de AD DS  
**Requisitos del controlador de dominio**  
  
- AD FS requiere controladores de dominio que ejecutan Windows Server 2008 o posterior.

- Falta al menos un controlador de dominio de Windows Server 2016 para Microsoft Passport for Work.
  
> [!NOTE]  
> Ha finalizado todo el soporte técnico para entornos con controladores de dominio de Windows Server 2003. Visite [esta página](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) para obtener más información sobre el ciclo de vida de soporte técnico de Microsoft.  
  
**Funcional del dominio\-requisitos de niveles**  
  
 - Todos los dominios de cuentas de usuario y el dominio al que se unen los servidores de AD FS deben estar funcionando en el nivel funcional del dominio de Windows Server 2003 o superior.  
  
 - Un dominio de Windows Server 2008 funcional nivel o superior es necesario para la autenticación de certificados de cliente si el certificado está asignado explícitamente a una cuenta de usuario en AD DS.  
  
**Requisitos de esquema**  
  
-   Las nuevas instalaciones de AD FS 2016 requieren el esquema de Active Directory de 2016 (versión mínima de 85).

-   Elevar el nivel de comportamiento de granja de servidores de AD FS (FBL) en el nivel de 2016, requiere el esquema de Active Directory de 2016 (versión mínima de 85).  

  
**Requisitos de la cuenta de servicio**  
  
-   Cualquier cuenta de dominio estándar puede usarse como una cuenta de servicio de AD FS. También se admiten cuentas de servicio administradas de grupo. Los permisos necesarios en tiempo de ejecución se agregará automáticamente al configurar AD FS.

-   La asignación de derechos de usuario necesarios para la cuenta de servicio de AD es 'Iniciar sesión como servicio'

-   Las asignaciones de derechos de usuario necesarias para la 'NT Service\adfssrv' y 'NT Service\drs' son 'Generar auditorías de seguridad' y 'Iniciar sesión como servicio'.

-   Cuentas de servicio administradas de grupo requieren al menos un controlador de dominio que ejecuta Windows Server 2012 o posterior.  La GMSA debe residir en el valor predeterminado "CN = contenedor de cuentas de servicio administradas.  

-   Para la autenticación Kerberos, el nombre principal de servicio '`HOST/<adfs\_service\_name>`' debe estar registrado en la cuenta de servicio de AD FS. De forma predeterminada, AD FS para configurar esto al crear una nueva granja de AD FS.  Si se produce un error, como en el caso de un conflicto o no tiene permisos suficientes, verá una advertencia y debe agregarla manualmente.  
   
**Requisitos de dominio**  
  
-   Todos los servidores de AD FS deben ser un unido a un dominio de AD DS.  
  
-   Todos los servidores de AD FS en una granja de servidores deben implementarse en el mismo dominio.  
   
**Requisitos de varios bosques**  
  
-   Cada dominio o bosque que contiene los usuarios autenticarse en el servicio AD FS debe confiar en el dominio al que se unen los servidores de AD FS.  

-   El bosque, que la cuenta de servicio de AD FS es miembro, debe confiar en todos los bosques de inicio de sesión de usuario. 
  
-   La cuenta de servicio de AD FS debe tener permisos para leer los atributos de usuario en cada dominio que contenga los usuarios autenticarse en el servicio AD FS.  
  
## <a name="BKMK_5"></a>Requisitos de la base de datos de configuración  
En esta sección se describe los requisitos y restricciones para las granjas de servidores de AD FS que usan respectivamente la Windows Internal Database (WID) o SQL Server como base de datos:  
  
**WID**  
  
-   No se admite el perfil de resolución de artefactos de SAML 2.0 en una granja WID.    

-   Detección de reproducción de tokens no es compatible en una granja WID. (Esta funcionalidad solo se usa solo en escenarios donde se actúa como proveedor de federación de AD FS y consumir tokens de seguridad de proveedores de notificaciones externo).  
  
En la tabla siguiente proporciona un resumen de cuántos servidores de AD FS son de un vs WID admite una granja de servidores de SQL Server.    
  
  
|| 1 - 100 autenticado (RP) configurado en AD FS | Más de 100 confianzas RP configuradas  |
| --- |--- | --- |
|Servidores de 1-30 AD FS|WID admitida|No se admite con WID - necesarios de SQL Server |
|Servidores de más de 30 AD FS|No se admite con WID - necesarios de SQL Server|No se admite con WID - necesarios de SQL Server  
  
**SQL Server**  
  
- AD FS en Windows Server 2016, SQL Server 2008 y versiones posteriores se admiten.

- Resolución de artefactos SAML y detección de reproducción de tokens se admiten en una granja de servidores de SQL Server.  
  
## <a name="BKMK_6"></a>Requisitos del explorador  
Cuando se realiza la autenticación de AD FS a través de un explorador o el control del explorador, el explorador debe ser compatible con los siguientes requisitos:  
  
-   JavaScript debe estar habilitado  
  
-   Para el inicio de sesión único, el explorador del cliente debe configurarse para permitir las cookies  
  
-   Indicación de nombre de servidor \(SNI\) deben ser compatibles  
  
-   Para la autenticación de certificados de certificado & dispositivo de usuario, el explorador debe admitir la autenticación de certificado de cliente SSL  

-   Para el inicio de sesión sin problemas con la autenticación integrada de Windows, el nombre del servicio de federación (por ejemplo, https:\/\/fs.contoso.com) debe configurarse en la zona de intranet local o sitios de confianza.
## <a name="BKMK_7"></a>Requisitos de red  
 
**Requisitos de Firewall**  
  
El firewall situado entre el Proxy de aplicación Web y la granja de servidores de federación y el firewall entre los clientes y el Proxy de aplicación Web debe tener el puerto TCP 443 habilitado entrante.  
  
Además, si la autenticación de certificado de usuario del cliente \(autenticación de clientTLS mediante X509 certificados de usuario\) es necesario y no está habilitado el punto de conexión certauth en el puerto 443, AD FS 2016 requiere que el puerto TCP 49443 esté habilitado entrada del firewall entre los clientes y el Proxy de aplicación Web. Esto no es necesario en el firewall entre el Proxy de aplicación Web y los servidores de federación\). 

Para obtener más información sobre el híbrido de puerto de requisitos, consulte [protocolos y puertos de identidad híbrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Para obtener más información, consulte [procedimientos recomendados para proteger los servicios de federación de Active Directory](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Requisitos de DNS**  
  
-   Obtener acceso a la intranet, todos los clientes tener acceso a AD FS del servicio dentro de la red corporativa interna \(intranet\) debe ser capaz de resolver el nombre del servicio AD FS para el equilibrador de carga para los servidores de AD FS o el servidor de AD FS.  
  
-   Para el acceso de extranet, todos los clientes tener acceso a AD FS del servicio desde fuera de la red corporativa \(extranet\/internet\) debe ser capaz de resolver el nombre del servicio AD FS para el equilibrador de carga para los servidores Proxy de aplicación Web o el servidor Proxy de aplicación Web.  
  
-   Cada servidor Proxy de aplicación Web en la red Perimetral debe ser capaz de resolver el nombre del servicio AD FS para el equilibrador de carga para los servidores de AD FS o el servidor de AD FS. Esto puede lograrse mediante un servidor DNS alternativo en la red Perimetral o cambiando la resolución de servidor local mediante el archivo de HOSTS.  
  
-   Para la autenticación integrada de Windows, debe usar un registro DNS A \(CNAME no\) para el nombre del servicio de federación.  

-   Para la autenticación de certificados de usuario en el puerto 443, "certauth. \<nombre_servicio_federación\>"debe configurarse en DNS para resolver en el proxy de aplicación web o servidor de federación.

-   Para el registro de dispositivos o para la autenticación moderna a los recursos locales con los clientes de versiones anteriores de Windows 10, "enterpriseregistration. \<sufijo upn\>", para cada sufijo UPN en uso en su organización, debe configurarse para resolver en el proxy de aplicación web o servidor de federación.

**Requisitos de equilibrador de carga**
- El equilibrador de carga no debe terminar SSL. AD FS admite varios casos de uso con la autenticación de certificado que se interrumpa cuando terminación SSL. No se admite la terminación SSL en el equilibrador de carga para los casos de uso. 
- Se recomienda usar un equilibrador de carga que admita SNI. En el evento no es así, mediante la reserva de enlace de AD FS 0.0.0.0 / servidor Proxy de aplicación Web debe proporcionar una solución alternativa.
- Se recomienda utilizar los puntos de conexión de sondeo de estado HTTP (no HTTPS) para realizar comprobaciones de estado del equilibrador de carga para enrutar el tráfico. Esto evita los problemas relativos a SNI. La respuesta a estos puntos de conexión de sondeo es HTTP 200 OK y sirve localmente sin ninguna dependencia en los servicios back-end. El sondeo de HTTP puede obtenerse a través de HTTP mediante la ruta de acceso '/ adfs/probe'
    - http://&lt;nombre de Proxy de aplicación Web &gt; /adfs/probe
    - http://&lt;el nombre del servidor ADFS &gt; /adfs/probe
    - http://&lt;dirección IP de Proxy de aplicación Web &gt; /adfs/probe
    - http://&lt;dirección IP de ADFS &gt; /adfs/probe
- NO se recomienda usar DNS round robin como una forma de equilibrar la carga. Usar este tipo de equilibrio de carga no proporcionan una forma automatizada para quitar un nodo del equilibrador de carga mediante sondeos de estado. 
- Se recomienda no usar afinidad de sesión basado en IP o sesiones permanentes para tráfico de autenticación para AD FS en el equilibrador de carga. Esto puede producir una sobrecarga de determinados nodos cuando se usa el protocolo de autenticación heredados para los clientes de correo electrónico para conectarse a servicios de correo electrónico de Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisitos de permisos  
El administrador que realiza la instalación y la configuración inicial de AD FS debe tener permisos de administrador local en el servidor de AD FS.  Si el administrador local no tiene permisos para crear objetos en Active Directory, primero deben tener un administrador de dominio crear los objetos de AD necesarios, a continuación, configurar la granja de AD FS mediante el parámetro AdminConfiguration.  
  
  

