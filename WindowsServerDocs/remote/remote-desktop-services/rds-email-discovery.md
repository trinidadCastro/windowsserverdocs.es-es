---
title: Configuración de la detección de correo electrónico para suscribirte a la fuente de RDS
description: Aprenda a integrar Azure AD Domain Services en tu implementación de RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712654"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configuración de la detección de correo electrónico para suscribirte a la fuente de RDS

¿Alguna vez has tenido problemas para que tus usuarios finales se conecten a tu fuente de RDS publicada, ya sea porque falta un solo carácter en la dirección URL de la fuente o porque perdieron el correo electrónico con la dirección URL? Casi todas las aplicaciones cliente de Escritorio remoto admiten la búsqueda de tu suscripción al especificar tu dirección de correo electrónico, lo que facilita más que nunca la conexión de tus usuarios a las RemoteApps y escritorios.

>[!IMPORTANT]
>La aplicación Escritorio remoto de Microsoft de Microsoft Store no admite la suscripción de direcciones de correo electrónico en este momento.

Antes de configurar la detección de correo electrónico, haz lo siguiente:

- Asegúrate de que tienes permiso para agregar un registro TXT al dominio asociado a tu correo electrónico (por ejemplo, si tus usuarios tienen @contoso.com direcciones de correo electrónico, necesitarás permisos para el dominio contoso.com).
- Crea una dirección URL de fuente web de Escritorio remoto (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx, por ejemplo https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx).

Ahora, sigue estos pasos para configurar la detección de correo electrónico:

1. En su explorador, conéctate al sitio web del registrador de nombres de dominio en el que está registrado tu dominio.
2. Ve a la página apropiada para tu dominio registrado donde puedes ver, agregar y editar registros DNS.
3. Especifica un nuevo registro DNS con las siguientes propiedades:
   - **Host:** _msradc
   - **Texto:** \<Dirección URL de fuente web de Escritorio remoto\>
   - **TTL:** 300

   Los nombres de los campos de los registros DNS varían según el registrador del nombre de dominio, pero este proceso dará lugar a un registro TXT llamado _msradc.\<nombre_dominio\>. (como _msradc.contoso.com) que tiene un valor de la fuente web de Escritorio remoto completa.

Ahora ya puede Ahora, inicia la aplicación Escritorio remoto en el dispositivo y suscríbete.