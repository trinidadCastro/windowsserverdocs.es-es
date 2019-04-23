---
title: Configuración de las directivas de solicitud de conexión
description: En este tema se proporciona información sobre cómo configurar directivas de solicitud de conexión en el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f80f9fb8be0c44cfb5685e5b9cc489282e4961d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884516"
---
# <a name="configure-connection-request-policies"></a>Configuración de las directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para crear y configurar directivas de solicitud de conexión que determine si el NPS local procesa las solicitudes de conexión o los reenvía al servidor RADIUS remoto para su procesamiento.

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permitan a los administradores de red designar qué servidores de servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de conexión que solicita el servidor que ejecuta el servidor de directivas de red \(NPS\) recibe de los clientes RADIUS.

La directiva de solicitud de conexión predeterminada usa NPS como servidor RADIUS y procesa localmente todas las solicitudes de autenticación.

Para configurar un servidor que ejecuta NPS para que actúe como proxy RADIUS y reenviar solicitudes de conexión a otros servidores NPS o RADIUS, debe configurar un grupo de servidores RADIUS remoto además de agregar una nueva directiva de solicitud de conexión que especifica las condiciones y configuraciones que deben coincidir con las solicitudes de conexión.

Puede crear un nuevo grupo de servidores remotos RADIUS mientras crea una nueva directiva de solicitud de conexión con el Asistente para nueva directiva de solicitud de conexión.

Si no desea que el NPS para que actúe como localmente las solicitudes de una conexión de servidor y el proceso RADIUS, puede eliminar la directiva de solicitud de conexión predeterminada.

Si desea que NPS actúe como servidor RADIUS, procesamiento de solicitudes de conexión localmente y como proxy RADIUS, reenviar algunas solicitudes de conexión a un grupo de servidores RADIUS remotos, agregue una nueva directiva mediante el procedimiento siguiente y, a continuación, compruebe que el valor predeterminado Directiva de solicitud de conexión es la última directiva procesada colocando por última vez en la lista de directivas.

## <a name="add-a-connection-request-policy"></a>Agregar una directiva de solicitud de conexión

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-add-a-new-connection-request-policy"></a>Para agregar una nueva directiva de solicitud de conexión 

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red** para abrir la consola de NPS. 
2. En el árbol de consola, haga doble clic en **directivas**.
3. Haga clic en **directivas de solicitud de conexión**y, a continuación, haga clic en **nueva directiva de solicitud de conexión**.
4. Use el Asistente para nueva de conexión solicitud de directiva para configurar la conexión solicitan una directiva y, si no configurado previamente, un grupo de servidores RADIUS remotos.


Para obtener más información sobre la administración de NPS, consulte [administrar un servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

