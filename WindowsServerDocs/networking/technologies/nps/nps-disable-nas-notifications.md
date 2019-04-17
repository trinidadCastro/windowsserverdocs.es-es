---
title: Deshabilite el reenvío de Notification NAS en NPS
description: Este tema proporciona instrucciones sobre cómo configurar autenticaciones simultáneas de servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Deshabilite el reenvío de Notification NAS en NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para deshabilitar el reenvío de inicio y detener mensajes desde servidores de acceso de red (NAS) a los miembros de un grupo de servidores RADIUS remoto configurado en NPS.

Cuando tengas grupos de servidores RADIUS remotos configurados y, en NPS **directivas de solicitud de conexión**, desactiva la **reenviar solicitudes de contabilidad a este grupo de servidores RADIUS remotos** casilla de verificación, estos grupos aún se envían NAS iniciar y detener los mensajes de notificación. 

Esto crea tráfico de red innecesario. Para eliminar este tráfico, deshabilitar notification NAS de reenvío para los servidores individuales en cada grupo de servidores RADIUS remotos.

Para completar este procedimiento, debe ser miembro de la **administradores** grupo.

### <a name="to-disable-nas-notification-forwarding"></a>Para deshabilitar el reenvío de notification NAS

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.

2. En la consola NPS, haz doble clic en **clientes y servidores RADIUS**, haz clic en **grupos de servidores RADIUS remotos**y, a continuación, haz doble clic en el grupo de servidores RADIUS remoto que quieras configurar. El grupo de servidores RADIUS remotos **propiedades** abre el cuadro de diálogo.

3. Haz doble clic en el miembro del grupo que quieras configurar y, a continuación, haz clic en el **autenticación/cuentas** pestaña.

4. En **contabilidad**, desactiva la **reenviar el inicio de servidor de acceso de red y dejar de recibir notificaciones a este servidor** y, a continuación, haz clic en **Aceptar**.

5. Repite los pasos 3 y 4 para todos los miembros del grupo que quieras configurar.

Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
