---
title: 'Emisión de notificaciones de AD FS solucionar:'
description: Este documento describe cómo solucionar problemas de emisión de tokens con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839666"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Emisión de notificaciones de AD FS solucionar:
Una notificación es una instrucción que hace un individuo sobre sí mismo o a otro asunto.  Notificaciones emitidas por un usuario de confianza, y se proporciona uno o más valores y, a continuación, empaqueta en tokens de seguridad emitidos por el servidor de AD FS.  Dado que hay varias piezas móviles en este proceso, emisión de notificaciones puede dividirse en estas partes de la claves.

>[!NOTE]  
>Puede usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) en el [ADFS ayuda](https://adfshelp.microsoft.com) sitio para ayudar a solucionar problemas de notificaciones.   

## <a name="token-request"></a>Solicitud de token
Cuando vaya a un usuario de confianza le redirigirá a AD FS con una solicitud de token.  Pueden surgir problemas con la solicitud.  En concreto:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>La solicitud con el formato con 3 partes (especialmente SAML)

### <a name="pre-formated-urls-that-have-typos"></a>Direcciones URL con formato previo que tienen errores tipográficos
Cuando se emite un token de confianza de WS-Federaion incluye esa solicitud de token a través de parámetros de cadena de consulta de dirección URL.  Si el usuario de confianza no especifica los parámetros correctos en esa dirección URL cuando realiza la redirección a AD FS, a continuación, podría producirse un problema con la solicitud.


En el orden para comprobar el formato del token, se puede usar una herramienta de depuración web


## <a name="token-response"></a>Respuesta de token

## <a name="authentication"></a>Autenticación

## <a name="claim-rule-processing"></a>Procesamiento de reglas de notificación