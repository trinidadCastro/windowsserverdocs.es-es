---
title: Configurar los grupos de servidores RADIUS remotos
description: Este tema proporciona información sobre cómo configurar los grupos de servidores RADIUS remotos en el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f293fd18176115365e5e243a90a034676b3262f9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-remote-radius-server-groups"></a>Configurar los grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para configurar los grupos de servidores RADIUS remotos al que deseas configurar NPS para que actúe como un servidor proxy y solicitudes de conexión directa a otros servidores NPS para su procesamiento.

## <a name="add-a-remote-radius-server-group"></a>Agregar un grupo de servidores RADIUS remotos

Puedes usar este procedimiento para agregar un nuevo grupo de servidores RADIUS remoto en el servidor de directivas de redes (NPS).

Al configurar NPS como un proxy de radio, se crea una nueva directiva de solicitud de conexión que usa NPS para determinar qué solicitudes de conexión para reenviar a otros servidores RADIUS. Además, se configura la directiva de solicitud de conexión especificando un radio grupo de servidores remotos que contiene uno o varios de los servidores RADIUS, que indica dónde se deben enviar las solicitudes de conexión que coincidan con la directiva de solicitud de conexión a NPS.

>[!NOTE]
>También puedes configurar un nuevo grupo de servidores RADIUS remoto durante el proceso de creación de una nueva directiva de solicitud de conexión.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-remote-radius-server-group"></a>Para agregar un grupo de servidores RADIUS remotos 

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red** para abrir la consola NPS.
2. En el árbol de consola, haz doble clic en **clientes y servidores RADIUS**, haz clic en **grupos de servidores RADIUS remotos**y, a continuación, haz clic en **nueva**.
3. La **nuevo grupo de servidores RADIUS remotos** abre el cuadro de diálogo. En **nombre del grupo**, escribe un nombre para el grupo de servidores RADIUS remotos.
4. **En los servidores RADIUS**, haz clic en **agregar**. La **agregar los servidores RADIUS** abre el cuadro de diálogo. Escribe la dirección IP del servidor RADIUS que quieras agregar al grupo, o escribe el \(FQDN\) nombre de dominio completo del servidor RADIUS y, a continuación, haz clic en **comprobar**.
5. En **agregar los servidores RADIUS**, haz clic en el **autenticación/cuentas** pestaña. En **secreto compartido** y **Confirmar secreto compartido**, escriba el secreto compartido. Debes usar el mismo secreto compartido al configurar el equipo local como un cliente RADIUS en el servidor RADIUS remoto.
6. Si no estás usando el protocolo de autenticación Extensible (EAP) para la autenticación, haz clic en **solicitud debe contener el atributo de autenticación de mensaje**. EAP usa el atributo autenticador de mensaje de forma predeterminada.
7. Comprueba que los números de puerto de autenticación y cuentas sean correctos para la implementación.
8. Si usas un secreto compartido diferente para las cuentas, en **contabilidad**, desactiva la **usar el mismo secreto compartido para la autenticación y contabilidad** y, a continuación, escribe la clave secreta compartida de contabilidad en **secreto compartido** y **Confirmar secreto compartido**.
9. Si no quieres reenviar el inicio de servidor de acceso de red y detener mensajes al servidor de RADIUS remoto, desactive la **reenviar el inicio de servidor de acceso de red y dejar de recibir notificaciones a este servidor** casilla de verificación.

Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).

