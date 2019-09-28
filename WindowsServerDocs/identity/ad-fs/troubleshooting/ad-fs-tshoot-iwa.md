---
title: Solución de problemas de AD FS la autenticación integrada de Windows
description: En este documento se describe cómo solucionar problemas de la autenticación integrada de Windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 98f6c2e39b2a5eeab76103c1ae477dde785a0e04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385392"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Solución de problemas de AD FS la autenticación integrada de Windows
La autenticación integrada de Windows permite a los usuarios iniciar sesión con sus credenciales de Windows y experimentar el inicio de sesión único (SSO) con Kerberos o NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Motivo de error en la autenticación integrada de Windows
Hay tres razones principales por las que se producirá un error en la autenticación integrada de Windows. Estas sobrecargas son:
    - Configuración de un nombre de entidad de seguridad de servicio (SPN)
    - Token de enlace de canal
    - Configuración de Internet Explorer

## <a name="spn-misconfiguration"></a>Configuración de SPN inestable
Un nombre de entidad de seguridad de servicio (SPN) es un identificador único de una instancia de servicio. La autenticación Kerberos utiliza los SPN para asociar una instancia de servicio con una cuenta de inicio de sesión de servicio. Esto permite a una aplicación cliente solicitar que el servicio autentique una cuenta aunque el cliente no tenga el nombre de la cuenta.

A continuación se muestra un ejemplo de cómo se usa un SPN con AD FS:
1. Un explorador Web consulta Active Directory para determinar qué cuenta de servicio ejecuta sts.contoso.com
2. Active Directory indica al explorador que es la cuenta de servicio AD FS.
3. El explorador obtendrá un vale de Kerberos para la cuenta de servicio de AD FS.

Si la cuenta de servicio de AD FS tiene un SPN mal configurado o incorrecto, esto puede causar problemas.  Al examinar los seguimientos de red, puede ver errores como KRB error: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Con los seguimientos de red (como Wireshark) puede determinar qué SPN está intentando resolver el explorador y, a continuación, usar la herramienta de línea de comandos, setspn-Q <spn>, puede realizar una búsqueda en ese SPN.  Es posible que no se encuentre o que esté asignada a otra cuenta distinta de la cuenta de servicio de AD FS.

![integración](media/ad-fs-tshoot-iwa/iwa3.png)

Puede comprobar el SPN examinando las propiedades de la cuenta de servicio de AD FS.

![integración](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token de enlace de canal
Actualmente, cuando una aplicación cliente se autentica en el servidor mediante Kerberos, Digest o NTLM usando HTTPS, se establece primero un canal de seguridad de nivel de transporte (TLS) y la autenticación se realiza mediante este canal. 

El token de enlace de canal es una propiedad del canal externo protegido por TLS y se usa para enlazar el canal externo a una conversación a través del canal interno autenticado por el cliente.

En caso de que se produzca un ataque de tipo "Man in the Middle" y que descifren y vuelvan a cifrar el tráfico SSL, la clave no coincidirá.  AD FS determinará que hay algo que se encuentra en el centro entre el explorador Web y el propio.  Esto hará que se produzca un error en la autenticación Kerberos y se le pedirá al usuario un cuadro de diálogo de 401 en lugar de una experiencia de SSO.

Esto puede deberse a:
 - todo lo que se encuentra entre el explorador y el AD FS
 - Fiddler
 - Servidores proxy inversos que realizan el protocolo de puente SSL

De forma predeterminada, AD FS tiene establecido en "permitir".  Puede cambiar esta configuración mediante el cmdlt de PowerShell `Set-ADFSProperties -ExtendProtectionTokenCheck`

Para obtener más información, vea [prácticas recomendadas para el planeamiento y la implementación seguros de AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuración de Internet Explorer
De forma predeterminada, Internet Explorer tendrá la siguiente manera:

1. Internet Explorer recibirá una respuesta 401 de AD FS con la palabra NEGOTIATE en el encabezado.
2. Esto indica al explorador Web que obtenga un vale de Kerberos o NTLM para devolverlo a AD FS.
3. De forma predeterminada, IE intentará hacerlo (SPNEGO) sin la interacción del usuario si la palabra NEGOTIATE está en el encabezado.  Solo funcionará para sitios de intranet.

Hay 2 cosas principales que pueden impedir que esto se happeing.
   - Habilitar la autenticación integrada de Windows no está activada en las propiedades de IE.  Esto se encuentra en opciones de Internet: > Advanced-> Security.
   
   ![integración](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Las zonas de seguridad no están configuradas correctamente
       - Los FQDN no están en la zona de intranet
       - AD FS dirección URL no está en la zona de intranet.

      ![integración](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
