---
title: Tipos de notificaciones de acceso de cliente en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3c68d615bcb7fffd8bb0c91116f5e3b73e252a65
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940349"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>Tipos de notificaciones de directiva de acceso de cliente en AD FS

Para proporcionar información adicional sobre el contexto de la solicitud, las directivas de acceso de cliente usan los siguientes tipos de notificaciones, que AD FS genera a partir de la información del encabezado de la solicitud para su procesamiento.  Para obtener más información, vea [el rol del motor de notificaciones](../technical-reference/the-role-of-the-claims-engine.md).

## <a name="x-ms-forwarded-client-ip"></a>X-MS-forwarded-Client-IP

Tipo de Claim:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Esta demanda AD FS representa un "mejor intento" en la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta solicitud puede contener varias direcciones IP, incluida la dirección de cada proxy que reenvió la solicitud.Esta solicitud se rellena a partir de un encabezado HTTP que actualmente solo está establecido en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. El valor de la demanda puede ser uno de los siguientes:


- Una dirección IP única: la dirección IP del cliente que está conectada directamente a Exchange Online.

    >! Tenga en cuenta La dirección IP de un cliente de la red corporativa aparecerá como la dirección IP de la interfaz externa del proxy o la puerta de enlace de salida de la organización.

- Una o varias direcciones IP
  - Si Exchange online no puede determinar la dirección IP del cliente que se conecta, establecerá el valor según el valor del encabezado x-forwarded-for, un encabezado no estándar que se puede incluir en las solicitudes basadas en HTTP y es compatible con muchos clientes, equilibradores de carga y servidores proxy en el mercado.
  - Varias direcciones IP que indican la dirección IP del cliente y la dirección de cada proxy que pasó la solicitud se separan con una coma.

    >! Tenga en cuenta Las direcciones IP relacionadas con la infraestructura de Exchange online no estarán presentes en la lista.


>! Atención Actualmente, Exchange Online solo admite direcciones IPV4; no es compatible con las direcciones IPV6.


## <a name="x-ms-client-application"></a>X-MS-Client-Application

Tipo de Claim:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Esta demanda AD FS representa el protocolo usado por el cliente final, que se corresponde de forma flexible a la aplicación que se está usando.Esta solicitud se rellena a partir de un encabezado HTTP que actualmente solo está establecido en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. En función de la aplicación, el valor de esta demanda será uno de los siguientes:



- En el caso de los dispositivos que usan Exchange Active Sync, el valor es Microsoft. Exchange. ActiveSync.
- El uso del cliente de Microsoft Outlook puede dar lugar a cualquiera de los siguientes valores:
    - Microsoft. Exchange. Autodiscover
    - Microsoft. Exchange. OfflineAddressBook
    - Microsoft. Exchange. RPC
    - Microsoft. Exchange. webservices
    - Microsoft. Exchange. MAPI
- Otros valores posibles para este encabezado son los siguientes:
    - Microsoft. Exchange. PowerShell
    - Microsoft. Exchange. SMTP
    - Microsoft. Exchange. PopImap
    - Microsoft. Exchange. pop
    - Microsoft. Exchange. IMAP

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo de Claim:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Esta AD FS Claim proporciona una cadena que representa el tipo de dispositivo que el cliente está usando para obtener acceso al servicio. Se puede usar cuando los clientes deseen evitar el acceso a determinados dispositivos (por ejemplo, tipos particulares de teléfonos inteligentes).Esta solicitud se rellena a partir de un encabezado HTTP que actualmente solo está establecido en Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación a AD FS. Los valores de ejemplo de esta demanda incluyen (pero no se limitan a) los valores siguientes.
>! Tenga en cuenta A continuación se muestran ejemplos de lo que puede contener el valor x-ms-User-Agent para un cliente cuya x-MS-Client-Application es "Microsoft. Exchange. ActiveSync"

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0,3

>! Tenga en cuenta También es posible que este valor esté vacío.


## <a name="x-ms-proxy"></a>X-MS-proxy

Tipo de Claim:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Esta AD FS notificaciones indica que la solicitud ha pasado a través del servidor proxy de Federación.Esta solicitud se rellena mediante el servidor proxy de Federación, que rellena el encabezado al pasar la solicitud de autenticación al back-end Servicio de federación. A continuación, AD FS lo convierte en una demanda.

El valor de la demanda es el nombre DNS del servidor proxy de Federación que pasó la solicitud.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (activo frente a pasivo)

Tipo de Claim:`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Este tipo de notificaciones se puede usar para determinar las solicitudes que se originan en clientes "activos" (enriquecidos) frente a clientes "pasivos" (basados en explorador Web). Esto permite que se permitan las solicitudes externas desde aplicaciones basadas en explorador como Outlook Web Access, SharePoint Online o el portal de Office 365 mientras se bloquean las solicitudes procedentes de clientes enriquecidos como Microsoft Outlook.

El valor de la demanda es el nombre del servicio AD FS que recibió la solicitud.
