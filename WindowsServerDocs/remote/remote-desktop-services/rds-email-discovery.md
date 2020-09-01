---
title: Configuración de la detección de correo electrónico para suscribirte a la fuente de RDS
description: Obtenga información sobre cómo configurar la detección de correo electrónico en su implementación de RDS.
ms.author: chrimo
ms.date: 8/28/2020
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 71b892d95b15f02445ec7898a6c57f931bc4b501
ms.sourcegitcommit: 2b1a12c85acff137e5ac84cd0e62d8353fcdde31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087468"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configuración de la detección de correo electrónico para suscribirte a la fuente de RDS

¿Alguna vez has tenido problemas para que tus usuarios finales se conecten a tu fuente de RDS publicada, ya sea porque falta un solo carácter en la dirección URL de la fuente o porque perdieron el correo electrónico con la dirección URL? Casi todas las aplicaciones cliente de Escritorio remoto admiten la búsqueda de tu suscripción al especificar tu dirección de correo electrónico, lo que facilita más que nunca la conexión de tus usuarios a las RemoteApps y escritorios.

Antes de configurar la detección de correo electrónico, haz lo siguiente:

- Asegúrate de que tienes permiso para agregar un registro TXT al dominio asociado a tu correo electrónico (por ejemplo, si tus usuarios tienen @contoso.com direcciones de correo electrónico, necesitarás permisos para el dominio contoso.com).
- Cree una dirección URL de fuente web de Escritorio remoto (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx, como https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx) ).

>[!NOTE]
>Si usa Windows Virtual Desktop en lugar de Escritorio remoto, querrá usar estas direcciones URL en su lugar:
>
>- Si usa Windows Virtual Desktop (clásico): <https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfedddiscovery.aspx>
>- Si usa Windows Virtual Desktop: <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>

Ahora, siga estos pasos para configurar la detección de correo electrónico:

1. En su explorador, conéctate al sitio web del registrador de nombres de dominio en el que está registrado tu dominio.
2. Ve a la página apropiada para tu dominio registrado donde puedes ver, agregar y editar registros DNS.
3. Especifica un nuevo registro DNS con las siguientes propiedades:
   - **Host:** _msradc
   - **Texto:** \<RD Web Feed URL\>
   - **TTL:** 300

   Los nombres de los campos de los registros DNS varían según el registrador del nombre de dominio, pero este proceso dará lugar a un registro TXT llamado _msradc.\<domain_name\> (como _msradc.contoso.com) que tiene un valor de la fuente web de Escritorio remoto completa.

Eso es todo. Ahora, inicia la aplicación Escritorio remoto en el dispositivo y suscríbete.