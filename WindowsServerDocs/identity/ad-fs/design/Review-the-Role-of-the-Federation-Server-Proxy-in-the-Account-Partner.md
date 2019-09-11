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
ms.openlocfilehash: eb0547ac1d84fffed31360f0ee510504510c24ce
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867668"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Revisar el rol del servidor proxy de federación en el asociado de cuenta

El rol principal del servidor proxy de Federación en la red perimetral de la organización del asociado de cuenta \(en\) servicios de Federación de Active Directory (AD FS) AD FS consiste en recopilar las credenciales de autenticación de un equipo cliente que inicia sesión a través de Internet y para pasar esas credenciales al servidor de Federación, que se encuentra dentro de la red corporativa de la organización del asociado de cuenta. La cuenta para el equipo cliente se almacena en el almacén de atributos del asociado de cuenta.  
  
Un servidor proxy de Federación también puede funcionar en uno o varios de los roles siguientes, dependiendo de cómo lo configure para satisfacer las necesidades de la organización del asociado de cuenta:  
  
-   Tokens de seguridad de retransmisión: el servidor de Federación emite un token de seguridad para el servidor proxy de Federación, que luego retransmite el token al equipo cliente. El token de seguridad se usa para proporcionar acceso a dicho equipo cliente a un usuario de confianza específico.  
  
-   Recopilar credenciales: el servidor proxy de Federación usa un formulario \(Web de inicio de sesión de cliente predeterminado clientlogon. aspx\-\) para recopilar credenciales basadas en contraseña\-a través de la autenticación basada en formularios. Sin embargo, puede personalizar este formulario para aceptar otros tipos de autenticación admitidos, como capa de sockets seguros \(la\) autenticación del cliente SSL. Para obtener más información acerca de cómo personalizar esta página, consulte Personalización del inicio de sesión de cliente \(y páginas de detección del dominio de inicio [http:\/\/\/go.Microsoft.com\/fwlink? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275).\) Un servidor proxy de Federación no acepta credenciales a través de la autenticación integrada de Windows.  
  
En Resumen, un servidor proxy de Federación del asociado de cuenta actúa como proxy para los inicios de sesión de cliente en un servidor de Federación ubicado en la red corporativa. El proxy de servidor de Federación también facilita la distribución de tokens de seguridad a los clientes de Internet que están destinados a usuarios de confianza.  
  
> [!CAUTION]  
> Exponer un servidor proxy de Federación en la extranet del asociado de cuenta hará que el formulario web de inicio de sesión de cliente sea accesible para cualquier usuario con acceso a Internet. Esto puede hacer que su organización sea vulnerable a algunos\-ataques basados en contraseñas, como ataques de diccionario o ataques por fuerza bruta que pueden desencadenar bloqueos de cuentas para las cuentas de usuario almacenadas en el dominio de Active Directory corporativo Servicios \(AD DS\).  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
