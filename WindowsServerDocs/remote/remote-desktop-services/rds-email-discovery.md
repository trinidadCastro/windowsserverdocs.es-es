---
title: Configurar la detección de correo electrónico para suscribirse a la fuente de RDS
description: Obtenga información sobre cómo integrar los servicios de dominio de AD de Azure en la implementación de RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691674"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurar la detección de correo electrónico para suscribirse a la fuente de RDS

¿Nunca ha tenido algún problema a los usuarios finales conectados a sus RDS publicado fuente, ya sea debido a un solo carácter que faltan en la fuente de dirección URL o debido a la pérdida del correo electrónico con la dirección URL? Casi todas las aplicaciones de cliente de escritorio remoto admiten buscar su suscripción escribiendo su dirección de correo electrónico, haciendo que sea más fácil que nunca para obtener los usuarios conectados a sus RemoteApps y escritorios.

>[!IMPORTANT]
>La aplicación de escritorio remoto de Microsoft en el Microsoft Store no es compatible con la suscripción de la dirección de correo electrónico en este momento.

Antes de configurar la detección de correo electrónico, haga lo siguiente:

- Asegúrese de que tiene permiso para agregar un registro TXT a dominio asociado con el correo electrónico (por ejemplo, si los usuarios tienen @contoso.com , las direcciones de correo electrónico tendría permisos para el dominio contoso.com)
- Crear una dirección URL de fuente del sitio Web RD (https://\ < rdweb-dns-name\ >.domain/RDWeb/Feed/webfeed.aspx, tales comohttps://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Ahora, siga estos pasos para configurar la detección de correo electrónico:

1. En el explorador, conéctese al sitio Web del registrador de nombre de dominio donde se registró su dominio.
2. Vaya a la página adecuada para su dominio registrado, donde puede ver, agregar y editar los registros DNS.
3. Escriba un nuevo registro DNS con las siguientes propiedades:
   - **Host:** _msradc
   - **Texto:** \ < RD Web fuente URL\ >
   - **TTL:** 300

   Los nombres de los campos de registros DNS varían según el registrador de nombres de dominio, pero este proceso, se producirá un registro TXT denominado _msradc. \ < nombre_dominio > (por ejemplo, _msradc.contoso.com) que tiene un valor de la fuente de Web de escritorio remoto completo.

¡Eso es todo! Ahora, inicia la aplicación de escritorio remoto en su dispositivo y suscribirse usted mismo!