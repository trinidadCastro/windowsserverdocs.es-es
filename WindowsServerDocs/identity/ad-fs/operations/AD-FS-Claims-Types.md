---
ms.assetid: 
title: "Tipos de AD FS de notificación de acceso de cliente"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Tipos de AD FS de notificación de la directiva de acceso de cliente

Para proporcionar información de contexto de la solicitud adicional, las directivas de acceso de cliente usa los siguientes tipos de notificación, que genera AD FS de la información de encabezado de solicitud para el procesamiento.  Para obtener más información, consulta [el rol del motor reclamaciones](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-reenvía-IP del cliente

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Esta notificación de AD FS representa un "mejor intento" determinar la dirección IP del usuario (por ejemplo, el cliente de Outlook) que realiza la solicitud. Esta notificación puede contener varias direcciones IP, incluida la dirección de cada proxy que reenvía la solicitud.  Esta notificación se rellena con un encabezado HTTP que se encuentra actualmente solo se establece mediante Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. El valor de la notificación puede ser una de las siguientes acciones:


- Una sola dirección IP: la dirección IP del cliente que está conectada directamente al Exchange Online

    >! [Nota] La dirección IP de un cliente en la red corporativa aparecerá como la dirección IP de interfaz externa del servidor proxy de salida de la organización o puerta de enlace.

- Una o varias direcciones IP
    - Si no puede determinar la dirección IP del cliente Exchange Online, establecerá el valor en función del valor de encabezado x reenvía para solicitudes de un encabezado estándar que puede incluirse en basados en HTTP y es compatible con muchos de los clientes, equilibradores de carga y servidores proxy en el mercado.
    - Varias direcciones IP que indica la dirección IP del cliente y la dirección de cada servidor proxy que pasa la solicitud se se separados por comas.

    >! [Nota] Direcciones IP relacionadas con la infraestructura de Exchange Online no estarán presentes en la lista.


>! [Advertencia] Exchange Online actualmente admite solo las direcciones IPV4; no admite las direcciones IPV6. 


## <a name="x-ms-client-application"></a>X-MS-aplicación de cliente

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Esta notificación de AD FS representa el protocolo usado por el cliente final, que corresponde ligeramente a la aplicación usada.  Esta notificación se rellena con un encabezado HTTP que se encuentra actualmente solo se establece mediante Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. Dependiendo de la aplicación, el valor de esta notificación será uno de los siguientes:



- En el caso de dispositivos que usan Exchange Active Sync, el valor es Microsoft.Exchange.ActiveSync. 
- Uso del cliente de Microsoft Outlook podría resultar en cualquiera de los siguientes valores:
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

## <a name="x-ms-client-user-agent"></a>X-MS-cliente de agente de usuario

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Esta notificación de AD FS proporciona una cadena que represente el tipo de dispositivo que el cliente está usando para acceder al servicio. Esto se puede usar cuando los clientes le gustaría impedir el acceso para ciertos dispositivos (por ejemplo, determinados tipos de teléfonos inteligentes).  Esta notificación se rellena con un encabezado HTTP que se encuentra actualmente solo se establece mediante Exchange Online, que rellena el encabezado al pasar la solicitud de autenticación de AD FS. Ejemplos de valores para esta notificación incluyen (pero no se limitan a) los siguientes valores.
>! [Nota] El a continuación se muestran algunos ejemplos de lo que puede contener el valor de x-ms-agente de usuario para un cliente cuya x-ms-aplicación de cliente es "Microsoft.Exchange.ActiveSync"

- Vórtice/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Nota] También es posible que este valor está vacío.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Esta notificación de AD FS indica que la solicitud se ha pasado a través del proxy del servidor de federación.  Esta notificación se rellena mediante el proxy de servidor de federación que rellena el encabezado al pasar el back-end de servicios de federación de la solicitud de autenticación. AD FS después convierte en una reclamación. 

El valor de la notificación es el nombre DNS del proxy del servidor de federación que pasa la solicitud.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-extremo-absoluta-ruta de acceso (Active vs pasivo)

Tipo de notificación: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Este tipo de notificación puede usarse para determinar las solicitudes procedentes de los clientes (enriquecidos) "activos" frente a los clientes "pasivos" (explorador basado en web). Esto permite que las solicitudes externas desde aplicaciones basadas en el explorador, como Outlook Web Access, SharePoint Online o el portal de Office 365 para poder mientras se bloquean las solicitudes procedentes de los clientes enriquecidos, como Microsoft Outlook.

El valor de la notificación es el nombre del servicio de AD FS que recibe la solicitud.
