---
title: 'Solución de problemas de AD FS: emisión de notificaciones'
description: En este documento se describe cómo solucionar problemas de emisión de tokens con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.openlocfilehash: 9eb5ce1ee92e828cc1fd6ceb40ddddec453afe87
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954191"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Solución de problemas de AD FS: emisión de notificaciones
Una demanda es una instrucción que hace un sujeto sobre sí misma u otro asunto.  Las notificaciones las emite un usuario de confianza, y se les asigna uno o varios valores y, a continuación, se empaquetan en los tokens de seguridad que emite el servidor de AD FS.  Dado que hay varias partes móviles en este proceso, la emisión de notificaciones se puede desglosar en estas partes clave.

>[!NOTE]
>Puede usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) en el sitio de [ayuda de ADFS](https://adfshelp.microsoft.com) para ayudar en la solución de problemas de notificaciones.

## <a name="token-request"></a>Solicitud de token
Cuando vaya a un usuario de confianza, se le redirigirá a AD FS con una solicitud de token.  Pueden surgir problemas con la solicitud.  Principalmente:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>El formato de la solicitud con terceros (especialmente SAML)

### <a name="pre-formated-urls-that-have-typos"></a>Direcciones URL con formato previo que tienen errores tipográficos
Al emitir un token de los usuarios de confianza de WS-Federaion que la solicitud de token entra en los parámetros de cadena de consulta de dirección URL.  Si el usuario de confianza no especifica los parámetros correctos en esa dirección URL cuando realiza la redirección a AD FS, podría producirse un problema con la solicitud.


Para comprobar el formato de los tokens, se puede usar una herramienta de depurador Web.


## <a name="token-response"></a>Respuesta de token

## <a name="authentication"></a>Autenticación

## <a name="claim-rule-processing"></a>Procesamiento de reglas de notificaciones