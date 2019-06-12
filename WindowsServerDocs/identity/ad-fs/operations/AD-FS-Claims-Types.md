---
ms.assetid: ''
title: Acceso de cliente de tipos de notificación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0ffa4273a2c776a16f3ea0ce77d1b3a528481468
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445163"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>Directiva de acceso de cliente tipos de notificación de AD FS

Para proporcionar información de contexto de solicitud adicionales, las directivas de acceso de cliente use los siguientes tipos de notificación, que AD FS genera a partir de la información de encabezado de solicitud de procesamiento.  Para obtener más información, consulte [el rol del motor de notificaciones de](../technical-reference/the-role-of-the-claims-engine.md).

## <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Esta notificación de AD FS representa un "mejor intento" en determinar la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta notificación puede contener varias direcciones IP, incluida la dirección de cada servidor proxy que reenvía la solicitud.  Esta notificación se rellena a partir de un encabezado HTTP que está actualmente establecido sólo por Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. El valor de la notificación puede ser uno de los siguientes:


- Una única dirección IP: dirección IP del cliente que está conectado directamente a Exchange Online

    >! [Nota] La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa de la puerta de enlace o proxy de salida de la organización.

- Una o varias direcciones IP
  - Si no se puede determinar la dirección IP del cliente de conexión de Exchange Online, establecerá el valor según el valor del encabezado x-forwarded-for, las solicitudes de un encabezado no estándar que puede incluirse en basado en HTTP y es compatible con muchos clientes, equilibradores de carga, y servidores proxy en el mercado.
  - Varias direcciones IP que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud estarán separadas por una coma.

    >! [Nota] Las direcciones IP relacionadas con la infraestructura de Exchange Online no estará presentes en la lista.


>! [Advertencia] Exchange Online actualmente admite solo las direcciones IPV4; no admite direcciones IPV6. 


## <a name="x-ms-client-application"></a>X-MS-Client-Application

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Esta notificación de AD FS representa el protocolo utilizado por el cliente final, que imprecisa corresponde a la aplicación que se va a usar.  Esta notificación se rellena a partir de un encabezado HTTP que está actualmente establecido sólo por Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. Dependiendo de la aplicación, el valor de esta notificación será uno de los siguientes:



- En el caso de los dispositivos que usan Exchange Active Sync, el valor es Microsoft.Exchange.ActiveSync. 
- Uso del cliente Microsoft Outlook puede producir en cualquiera de los siguientes valores:
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- Otros valores posibles para este encabezado incluyen lo siguiente:
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Esta notificación de AD FS proporciona una cadena que represente el tipo de dispositivo que usa el cliente para tener acceso al servicio. Esto se puede usar cuando los clientes deseen evitar el acceso a determinados dispositivos (por ejemplo, determinados tipos de teléfonos inteligentes).  Esta notificación se rellena a partir de un encabezado HTTP que está actualmente establecido sólo por Exchange Online, que llena el encabezado al pasar la solicitud de autenticación para AD FS. Los valores de esta notificación de ejemplo incluyen (pero no están limitados a) los siguientes valores.
>! [Nota] El a continuación se muestran algunos ejemplos de lo que podría contener el valor de x-ms-user-agent para un cliente cuyo x-ms-client-application es "Microsoft.Exchange.ActiveSync"

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Nota] También es posible que este valor está vacío.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Esta notificación de AD FS indica que la solicitud se pasa a través del proxy de servidor de federación.  Esta notificación se rellena con el proxy de servidor de federación, que llena el encabezado al pasar la solicitud de autenticación para el back-end del servicio de federación. AD FS, a continuación, lo convierte en una notificación. 

El valor de la notificación es el nombre DNS del servidor proxy de federación que pasa la solicitud.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-absoluto-Path (activa o pasiva)

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Este tipo de notificación puede utilizarse para determinar las solicitudes procedentes de clientes (avanzados) "activos" frente a los clientes "pasivos" (explorador basadas en web). Esto permite que las solicitudes externas desde aplicaciones basadas en explorador como Outlook Web Access, SharePoint Online o el portal de Office 365 se permite mientras se bloquean las solicitudes procedentes de clientes enriquecidos, como Microsoft Outlook.

El valor de la notificación es el nombre del servicio AD FS que recibió la solicitud.
