---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: "Revisa el rol del servidor Proxy Server federación del asociado de cuenta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Revisa el rol del servidor Proxy Server federación del asociado de cuenta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La función principal del servidor proxy de servidor de federación de la red del perímetro de la organización de partner de la cuenta en los servicios de federación de Active Directory \(AD FS\) es para recopilar las credenciales de autenticación de un equipo cliente que inicie sesión a través de Internet y para pasar las credenciales al servidor de federación, que se encuentra dentro de la red corporativa de la organización de partner de la cuenta. La cuenta para el equipo cliente se almacena en la tienda de atributo del partner de la cuenta.  
  
Un proxy de servidor de federación también puede funcionar en una o varias de las siguientes funciones, dependiendo de la configuración para satisfacer las necesidades de la organización de partner de la cuenta:  
  
-   Transmitir Tokens de seguridad, el servidor de federación emite un token de seguridad para el proxy de servidor de federación, que, a continuación, pasa el token en el equipo cliente. El token de seguridad se usa para proporcionar acceso a ese equipo cliente a un usuario de confianza específicas.  
  
-   Recopilar las credenciales, el proxy de servidor de federación usa un \(clientlogon.aspx\) predeterminado cliente inicio de sesión Web formulario para recopilar las credenciales basadas en password\ a través de la autenticación basada en forms\. Sin embargo, puedes personalizar este formulario para aceptar otros tipos de autenticación, como la autenticación de cliente de capa de Sockets seguros \(SSL\) admitidos. ¿Para obtener más información sobre cómo personalizar esta página, consulta personalizar inicio de sesión del cliente y páginas de detección de territorio Home \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un proxy de servidor de federación no acepta credenciales mediante la autenticación integrada de Windows.  
  
Para resumir, un proxy de servidor de federación de asociado de cuenta actúa como proxy para los inicios de sesión del cliente a un servidor de federación que se encuentra en la red corporativa. El proxy de servidor de federación también facilita la distribución de tokens de seguridad a los clientes de Internet que están destinados a usuarios de confianza.  
  
> [!CAUTION]  
> Exponer a un proxy de federación de servidor en la extranet de asociado de cuenta el inicio de sesión del cliente un formulario Web accesible cualquier persona con Internet accederá. Esto puede potencialmente vulnerable tu organización a algunos ataques basados en password\, como los ataques de diccionario o los ataques de fuerza bruta que puedan desencadenar bloqueos de cuenta para cuentas de usuario que se almacenan en la empresa \(AD DS\) los servicios de dominio de Active Directory.  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
