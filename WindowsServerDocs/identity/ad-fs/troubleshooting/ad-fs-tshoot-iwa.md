---
title: Solución de AD FS - de autenticación de Windows integrada
description: Este documento describe cómo solucionar problemas de autenticación integrada de windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 91f252f5b0eca0f4c44e0b1a4564037298bf023c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814066"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Solución de AD FS - de autenticación de Windows integrada
La autenticación integrada de Windows permite a los usuarios inicien sesión con sus credenciales de Windows y la experiencia de inicio de sesión único (SSO), utilizando Kerberos o NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Se produce un error en la autenticación integrada de windows de motivo
Hay tres motivo principal por qué se producirá un error de autenticación integrada de windows. Estas sobrecargas son:
    - Configuración incorrecta de la entidad de servicio (SPN)
    - Token de enlace de canal
    - Configuración de Internet Explorer

## <a name="spn-misonfiguration"></a>SPN misonfiguration
Un nombre principal de servicio (SPN) es un identificador único de una instancia de servicio. Los SPN se utilizan la autenticación Kerberos para asociar una instancia de servicio con una cuenta de inicio de sesión del servicio. Esto permite que una aplicación cliente solicitar que el servicio autentique una cuenta incluso si el cliente no tiene el nombre de cuenta.

Un ejemplo de un procedimiento que se usa un SPN con AD FS es como sigue:
1. Un explorador web consulta a Active Directory para determinar qué cuenta de servicio se está ejecutando sts.contoso.com
2. Active Directory indica al explorador que es la cuenta de servicio de AD FS.
3. El explorador obtendrán un vale de Kerberos para la cuenta de servicio de AD FS.

Si la cuenta de servicio de AD FS tiene un mal configurado o el SPN incorrecto, a continuación, esto puede causar problemas.  Examinar los seguimientos de red, puede ver errores como KRB Error: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Mediante los rastros de red (por ejemplo, Wireshark) puede determinar qué SPN está intentando resolver el explorador y, a continuación, mediante la línea de comandos de herramientas, setspn - Q <spn>, puede hacer una búsqueda en ese SPN.  No se puede encontrar o se pueden asignar a otra cuenta distinta de la cuenta de servicio de AD FS.

![Integrado](media/ad-fs-tshoot-iwa/iwa3.png)

El SPN, puede comprobar examinando las propiedades de la cuenta de servicio de AD FS.

![Integrado](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token de enlace de canal
Actualmente, cuando una aplicación cliente se autentica en el servidor mediante Kerberos, Digest o NTLM usando HTTPS, primero se establece un canal de seguridad de nivel de transporte (TLS) y autenticación lleva a cabo usando este canal. 

El Token de enlace de canal es una propiedad del canal externo seguro para TLS y se usa para enlazar el canal externo a una conversación a través del canal interno autenticado con el cliente.

Si hay que se produzca un ataque de "tipo man in the middle" y son de cifrado y volver a cifrar el tráfico SSL, entonces no coincidirá con la clave.  AD FS determinará que hay algo en la mitad entre el objeto de r de exploración web y ella misma.  Esto hará que la autenticación Kerberos de un error y se pedirá al usuario un cuadro de diálogo 401 en lugar de una experiencia de inicio de sesión único.

Esto puede ser originado por:
 - todo lo que se encuentre entre el explorador y AD FS
 - Fiddler
 - Servidores proxy inversos realizando el protocolo de puente SSL

De forma predeterminada, AD FS tiene este valor en "permitir".  Puede cambiar esta configuración mediante el script de PowerShell. `Set-ADFSProperties -ExtendProtectionTokenCheck`

Para obtener más información sobre este, consulte [prácticas recomendadas para proteger planeación e implementación de AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuración de Internet Explorer
De forma predeterminada, Internet explorer se tendrá la siguiente manera:

1. El Explorador de Internet recibirá una respuesta 401 de AD FS con la palabra NEGOTIATE en el encabezado.
2. Esto indica que el explorador web para obtener un vale de Kerberos o NTLM para enviar a AD FS.
3. De forma predeterminada, IE intentará hacer este (SPNEGO) sin interacción del usuario si la palabra NEGOTIATE está en el encabezado.  Solo funcionará para los sitios de intranet.

Hay 2 aspectos principales que pueden evitar que esto happeing.
   - Habilitar que la autenticación integrada de Windows no está activada en las propiedades de Internet Explorer.  Esto se encuentra en Opciones de Internet -> Opciones avanzadas -> seguridad.
![integrated](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Las zonas de seguridad no están configuradas correctamente
       - FQDN no están en la zona de intranet
       - URL de AD FS no está en la zona de intranet.

![Integrado](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)