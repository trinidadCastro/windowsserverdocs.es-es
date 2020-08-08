---
title: Configuración de las directivas de solicitud de conexión
description: En este tema se proporciona información sobre cómo configurar directivas de solicitud de conexión en el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 422ef3238aac950b7d2808d8576b186aeb430767
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969422"
---
# <a name="configure-connection-request-policies"></a>Configuración de las directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear y configurar directivas de solicitud de conexión que designen si el NPS local procesa las solicitudes de conexión o las reenvía al servidor RADIUS remoto para su procesamiento.

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permiten a los administradores de red designar qué servidores Servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de solicitudes de conexión que el servidor que ejecuta NPS del servidor \( \) de directivas de redes recibe desde clientes RADIUS.

La directiva de solicitud de conexión predeterminada usa NPS como servidor RADIUS y procesa localmente todas las solicitudes de autenticación.

Para configurar un servidor que ejecute NPS para que funcione como proxy RADIUS y reenvíe las solicitudes de conexión a otros NPS o servidores RADIUS, debe configurar un grupo remoto de servidores RADIUS además de agregar una nueva directiva de solicitud de conexión que especifique las condiciones y configuraciones con las que deben coincidir las solicitudes de conexión.

Puede crear un nuevo grupo de servidores remotos RADIUS mientras crea una nueva directiva de solicitud de conexión con el asistente para Nueva directiva de solicitud de conexión.

Si no desea que NPS actúe como servidor RADIUS y procese las solicitudes de conexión localmente, puede eliminar la Directiva de solicitud de conexión predeterminada.

Si desea que NPS actúe como un servidor RADIUS, procesando las solicitudes de conexión localmente y como proxy RADIUS, reenviando algunas solicitudes de conexión a un grupo de servidores RADIUS remotos, agregue una nueva Directiva mediante el siguiente procedimiento y, a continuación, compruebe que la Directiva de solicitud de conexión predeterminada es la última Directiva procesada, colocándolo en último lugar en la lista de directivas.

## <a name="add-a-connection-request-policy"></a>Adición de una directiva de solicitud de conexión

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-new-connection-request-policy"></a>Para agregar una nueva Directiva de solicitud de conexión

1. En Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes** para abrir la consola NPS.
2. En el árbol de consola, haga doble clic en **directivas**.
3. Haga clic con el botón secundario en **directivas de solicitud de conexión**y luego haga clic en **nueva Directiva de solicitud de conexión**.
4. Use el Asistente para nueva directiva de solicitud de conexión para configurar la directiva de solicitud de conexión y, si no lo hizo anteriormente, un grupo remoto de servidores RADIUS.


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

