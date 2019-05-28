---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Revisar el rol del servidor proxy de federación en el asociado de cuenta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbceb19d31738bdc5b628a9a2b069e5d3022d145
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190952"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Revisar el rol del servidor proxy de federación en el asociado de cuenta

La función principal del servidor proxy de federación en la red perimetral de la organización del asociado de cuenta en Active Directory Federation Services \(AD FS\) consiste en recopilar las credenciales de autenticación de un equipo cliente que inicia sesión a través de Internet y pasar esas credenciales para el servidor de federación, que se encuentra dentro de la red corporativa de la organización del asociado de cuenta. La cuenta para el equipo cliente se almacena en el almacén de atributos del asociado de cuenta.  
  
Un servidor proxy de federación también puede funcionar en uno o varios de los roles siguientes, dependiendo de cómo se configura para satisfacer las necesidades de la organización del asociado de cuenta:  
  
-   Los Tokens de seguridad de retransmisión, el servidor de federación emite un token de seguridad para el proxy de servidor de federación, que luego retransmite el token en el equipo cliente. El token de seguridad se usa para proporcionar acceso a dicho equipo cliente a un usuario de confianza específico.  
  
-   Recopilar las credenciales, el servidor proxy de federación utiliza un inicio de sesión de cliente predeterminada formulario Web Forms \(clientlogon.aspx\) para recopilar la contraseña\-credenciales a través de formularios basadas en\-autenticación basada en. Sin embargo, puede personalizar este formulario para aceptar otros tipos admitidos de autenticación, como Secure Sockets Layer \(SSL\) autenticación del cliente. ¿Para obtener más información sobre cómo personalizar esta página, consulte Personalizar el inicio de sesión de cliente y páginas de detección del territorio inicio \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un servidor proxy de federación no acepta credenciales mediante la autenticación integrada de Windows.  
  
En resumen, un servidor proxy de federación del asociado de cuenta actúa como un proxy para los inicios de sesión de cliente a un servidor de federación que se encuentra en la red corporativa. El servidor proxy de federación también facilita la distribución de los tokens de seguridad a los clientes de Internet que están destinados a usuarios de confianza.  
  
> [!CAUTION]  
> Exponer a un servidor proxy de federación en la extranet del asociado de cuenta tendrá el formulario Web accesibles por cualquier persona con Internet de inicio de sesión de cliente acceso a. Posiblemente esto puede hacer su organización sea vulnerable a algunos contraseña\-en función de los ataques, como los ataques de diccionario o ataques de fuerza bruta que pueden desencadenar bloqueos de cuentas para las cuentas de usuario que se almacenan en el dominio de Active Directory corporativo Servicios \(AD DS\).  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
