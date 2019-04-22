---
title: Configurar grupos de servidores RADIUS remotos
description: En este tema se proporciona información sobre cómo configurar grupos de servidores RADIUS remotos en el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818766"
---
# <a name="configure-remote-radius-server-groups"></a>Configurar grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para configurar grupos de servidores RADIUS remotos cuando desea configurar NPS para que actúe como un servidor proxy y reenviar solicitudes de conexión a otros NPSs para su procesamiento.

## <a name="add-a-remote-radius-server-group"></a>Agregar un grupo de servidores remotos RADIUS

Puede usar este procedimiento para agregar un nuevo grupo de servidores RADIUS remoto en el servidor de directivas de redes (NPS).

Cuando se configura NPS como proxy RADIUS, cree una nueva directiva de solicitud de conexión que NPS usa para determinar qué solicitudes de conexión para reenviar a otros servidores RADIUS. Además, la directiva de solicitud de conexión se configura especificando un remoto grupo de servidores RADIUS que contiene uno o más servidores RADIUS, que indica a NPS dónde debe enviar las solicitudes de conexión que coinciden con la directiva de solicitud de conexión.

>[!NOTE]
>También puede configurar un nuevo grupo de servidores remotos RADIUS durante el proceso de creación de una nueva directiva de solicitud de conexión.

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-add-a-remote-radius-server-group"></a>Para agregar un grupo de servidores RADIUS remotos 

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red** para abrir la consola de NPS.
2. En el árbol de consola, haga doble clic en **clientes y servidores RADIUS**, haga clic en **grupos de servidores RADIUS remotos**y, a continuación, haga clic en **New**.
3. El **nuevo grupo de servidores RADIUS remotos** abre el cuadro de diálogo. En **conmutaciónporerror**, escriba un nombre para el grupo de servidores RADIUS remotos.
4. **En servidores RADIUS**, haga clic en **agregar**. El **Agregar servidor RADIUS** abre el cuadro de diálogo. Escriba la dirección IP del servidor RADIUS que desea agregar al grupo, o escriba el nombre de dominio completo \(FQDN\) del servidor RADIUS y, a continuación, haga clic en **compruebe**.
5. En **Agregar servidor RADIUS**, haga clic en el **autenticación/cuentas** ficha. En **secreto compartido** y **Confirmar secreto compartido**, escriba el secreto compartido. Debe usar el mismo secreto compartido al configurar el equipo local como un cliente RADIUS en el servidor RADIUS remoto.
6. Si no usa el protocolo de autenticación Extensible (EAP) para la autenticación, haga clic en **solicitud debe contener el atributo de autenticador de mensaje**. EAP usa el atributo de autenticador de mensaje de forma predeterminada.
7. Compruebe que los números de puerto de autenticación y cuentas son correctos para su implementación.
8. Si usa un secreto compartido diferente para las cuentas, en **contabilidad**, desactive la **usar el mismo secreto compartido para autenticación y cuentas** casilla de verificación y, a continuación, escriba el secreto compartido de cuentas en  **Secreto compartido** y **Confirmar secreto compartido**.
9. Si no desea reenviar el inicio del servidor de acceso de red y detener los mensajes al servidor remoto RADIUS, desactive la **reenviar el inicio del servidor de acceso de red y dejar de recibir notificaciones a este servidor** casilla de verificación.

Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

