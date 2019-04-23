---
title: Configurar la detección de correo electrónico para suscribirse a la fuente de RDS
description: Obtenga información sobre cómo integrar Azure AD Domain Services en su implementación de RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878326"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar la detección de correo electrónico para suscribirse a la fuente de RDS

¿Ha tenido problemas al acceder a los usuarios finales conectados a sus RDS publicado fuente, ya sea debido a un único carácter que faltan en la fuente dirección URL o porque perdió el correo electrónico con la dirección URL? Casi todas las aplicaciones de cliente de escritorio remoto admiten la búsqueda de la suscripción especificando su dirección de correo electrónico, más fácil que nunca para que los usuarios conectados a su RemoteApp y escritorios.

>[!IMPORTANT]
>La aplicación de escritorio remoto de Microsoft en la Microsoft Store no es compatible con la suscripción de la dirección de correo electrónico en este momento.

Antes de configurar la detección de correo electrónico, haga lo siguiente:

- Asegúrese de que tiene permiso para agregar un registro TXT en el dominio asociado con su correo electrónico (por ejemplo, si los usuarios tienen @contoso.com direcciones de correo electrónico necesitaría permisos para el dominio contoso.com)
- Crear una dirección URL de fuente de Web de escritorio remoto (https://\<nombre-dns-rdweb\>.domain/RDWeb/Feed/webfeed.aspx, como https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Ahora, siga estos pasos para configurar la detección de correo electrónico:

1. En el explorador, conéctese al sitio Web del registrador de nombres de dominio donde se registró su dominio.
2. Vaya a la página adecuada para el dominio registrado, donde puede ver, agregar y editar registros de DNS.
3. Escriba un nuevo registro DNS con las siguientes propiedades:
   - **Host:** _msradc
   - **Texto:** \<Dirección URL de fuente de Web de escritorio remoto\>
   - **TTL:** 300

   Los nombres de los campos de registros DNS varían según el registrador de nombres de dominio, pero este proceso dará como resultado un registro TXT denominado _msradc. \<nombre_dominio\> (por ejemplo, _msradc.contoso.com) que tiene un valor de la fuente de Web de escritorio remoto completa.

Ahora ya puede Ahora, inicie la aplicación de escritorio remoto en el dispositivo y suscribirse.