---
title: Deshabilitar el reenvío de notificaciones de NAS en NPS
description: Este tema proporciona instrucciones sobre la configuración de autenticaciones simultáneas del servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882266"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Deshabilitar el reenvío de notificaciones de NAS en NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para deshabilitar el reenvío de inicio y detener los mensajes desde los servidores de acceso de red (NAS) a los miembros de un grupo de servidores RADIUS remoto configurado en NPS.

Cuando haya grupos de servidores RADIUS remotos configurados y, en NPS **directivas de solicitud de conexión**, desactiva la **reenviar las solicitudes de cuentas a este grupo de servidores RADIUS remotos** casilla de verificación, estos grupos son NAS enviados todavía iniciar y detener los mensajes de notificación. 

Esto crea el tráfico de red innecesario. Para eliminar este tráfico, deshabilitar la notificación de NAS de reenvío para servidores individuales en cada grupo de servidores RADIUS remotos.

Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

### <a name="to-disable-nas-notification-forwarding"></a>Para deshabilitar el reenvío de notificaciones de NAS

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2. En la consola NPS, haga doble clic en **clientes y servidores RADIUS**, haga clic en **grupos de servidores RADIUS remotos**y, a continuación, haga doble clic en el grupo de servidores remotos RADIUS que desea configurar. El grupo de servidores RADIUS remotos **propiedades** abre el cuadro de diálogo.

3. Haga doble clic en el miembro del grupo que desea configurar y, a continuación, haga clic en el **autenticación/cuentas** ficha.

4. En **contabilidad**, desactive la **reenviar el inicio del servidor de acceso de red y dejar de recibir notificaciones a este servidor** casilla de verificación y, a continuación, haga clic en **Aceptar**.

5. Repita los pasos 3 y 4 para todos los miembros del grupo que desea configurar.

Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
