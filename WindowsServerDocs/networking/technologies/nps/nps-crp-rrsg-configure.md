---
title: Configurar grupos de servidores RADIUS remotos
description: En este tema se proporciona información sobre cómo configurar grupos de servidores RADIUS remotos en el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d9e1afd9505d3bbf1383d174cac6a2f543fcaae2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316181"
---
# <a name="configure-remote-radius-server-groups"></a>Configurar grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para configurar grupos de servidores RADIUS remotos si desea configurar NPS para que actúe como servidor proxy y reenvíe las solicitudes de conexión a otros NPSs para su procesamiento.

## <a name="add-a-remote-radius-server-group"></a>Adición de un grupo de servidores remotos RADIUS

Puede usar este procedimiento para agregar un nuevo grupo de servidores remotos RADIUS en el complemento Servidor de directivas de redes (NPS).

Al configurar NPS como proxy RADIUS, crea una nueva directiva de solicitud de conexión que NPS usa para determinar cuáles son las solicitudes de conexión que se deben reenviar a otros servidores RADIUS. Además, la Directiva de solicitud de conexión se configura especificando un grupo de servidores RADIUS remotos que contiene uno o más servidores RADIUS, que indica a NPS dónde debe enviar las solicitudes de conexión que coincidan con la Directiva de solicitud de conexión.

>[!NOTE]
>Además, puede configurar un nuevo grupo de servidores remotos RADIUS durante el proceso de creación de una nueva directiva de solicitud de conexión.

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-remote-radius-server-group"></a>Para agregar un grupo de servidores RADIUS remotos 

1. En Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes** para abrir la consola NPS.
2. En el árbol de consola, haga doble clic en **clientes y servidores RADIUS**, haga clic con el botón secundario en **grupos de servidores RADIUS remotos**y, a continuación, haga clic en **nuevo**.
3. Se abrirá el cuadro de diálogo **nuevo grupo de servidores RADIUS remotos** . En **nombre de grupo**, escriba un nombre para el grupo de servidores RADIUS remotos.
4. **En servidores RADIUS**, haga clic en **Agregar**. Se abre el cuadro de diálogo **agregar servidores RADIUS** . Escriba la dirección IP del servidor RADIUS que desea agregar al grupo, o escriba el nombre de dominio completo \(FQDN\) del servidor RADIUS y, a continuación, haga clic en **comprobar**.
5. En **agregar servidores RADIUS**, haga clic en la pestaña **Autenticación/cuentas** . En **secreto compartido** y **confirmar secreto compartido**, escriba el secreto compartido. Debe usar el mismo secreto compartido al configurar el equipo local como cliente RADIUS en el servidor remoto RADIUS.
6. Si no usa el protocolo de autenticación extensible (EAP) para la autenticación, haga clic en **la solicitud debe contener el atributo de autenticador de mensaje**. EAP usa el atributo Message-Authenticator de forma predeterminada.
7. Compruebe que los números de puertos de autenticación y cuentas sean los correctos para su implementación.
8. Si usa un secreto compartido diferente para cuentas, en **Contabilidad**, desactive la casilla **usar el mismo secreto compartido para autenticación y cuentas** y, a continuación, escriba el secreto compartido de contabilidad en **secreto compartido** y **confirme el secreto compartido**.
9. Si no desea reenviar los mensajes de inicio y detención del servidor de acceso a la red al servidor RADIUS remoto, desactive la casilla **reenviar las notificaciones de inicio y detención del servidor de acceso a la red a este servidor** .

Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

