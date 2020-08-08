---
title: Deshabilitación del reenvío de notificaciones NAS en NPS
description: En este tema se proporcionan instrucciones sobre cómo configurar las autenticaciones simultáneas del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 72ac7d13005fef609df437ce3fcb4e12f617a400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946979"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Deshabilitación del reenvío de notificaciones NAS en NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para deshabilitar el reenvío de mensajes de inicio y detención de los servidores de acceso a la red (NAS) a los miembros de un grupo de servidores remotos RADIUS configurado en NPS.

Cuando tiene grupos de servidores RADIUS remotos configurados y, en **las directivas de solicitud de conexión**NPS, desactiva la casilla **Reenviar solicitudes de cuentas a este grupo de servidores RADIUS remotos** , estos grupos siguen enviando mensajes de notificación de inicio y detención de NAS.

Esto genera un tráfico de red innecesario. Para eliminar este tráfico, deshabilite el reenvío de notificaciones de NAS para servidores individuales en cada grupo de servidores RADIUS remotos.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-disable-nas-notification-forwarding"></a>Para deshabilitar el reenvío de notificaciones NAS

1. En el Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Servidor de directivas de redes**. Se abre la consola NPS.

2. En la consola de NPS, haga doble clic en **clientes y servidores RADIUS**, haga clic en **grupos de servidores remotos RADIUS**y, a continuación, haga doble clic en el grupo de servidores remotos RADIUS que desea configurar. Se abre el cuadro de diálogo **propiedades** del grupo de servidores RADIUS remoto.

3. Haga doble clic en el miembro del grupo que desee configurar y, a continuación, haga clic en la pestaña **Autenticación/cuentas** .

4. En **Contabilidad**, desactive la casilla **reenviar las notificaciones de inicio y detención del servidor de acceso de red a este servidor** y, a continuación, haga clic en **Aceptar**.

5. Repita los pasos 3 y 4 para todos los miembros del grupo que desee configurar.

Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
