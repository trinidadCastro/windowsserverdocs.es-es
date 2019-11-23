---
title: Configuración de las directivas de solicitud de conexión
description: En este tema se proporciona información sobre cómo configurar directivas de solicitud de conexión en el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d62beb3106141d4683c957020bc96e4a7dfb306f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405480"
---
# <a name="configure-connection-request-policies"></a>Configuración de las directivas de solicitud de conexión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear y configurar directivas de solicitud de conexión que designen si el NPS local procesa las solicitudes de conexión o las reenvía al servidor RADIUS remoto para su procesamiento.

Las directivas de solicitud de conexión son conjuntos de condiciones y configuraciones que permiten a los administradores de red designar qué servidores de Servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de las solicitudes de conexión que el servidor que ejecuta el servidor de directivas de redes \(NPS\) recibe de los clientes RADIUS.

La Directiva de solicitud de conexión predeterminada usa NPS como servidor RADIUS y procesa todas las solicitudes de autenticación de forma local.

Para configurar un servidor que ejecuta NPS para que actúe como proxy RADIUS y reenvíe las solicitudes de conexión a otros servidores NPS o RADIUS, debe configurar un grupo de servidores RADIUS remotos además de agregar una nueva Directiva de solicitud de conexión que especifique las condiciones y la configuración que las solicitudes de conexión deben coincidir.

Puede crear un nuevo grupo de servidores RADIUS remotos mientras crea una nueva Directiva de solicitud de conexión con el Asistente para nueva Directiva de solicitud de conexión.

Si no desea que NPS actúe como servidor RADIUS y procese las solicitudes de conexión localmente, puede eliminar la Directiva de solicitud de conexión predeterminada.

Si desea que NPS actúe como servidor RADIUS, procesando las solicitudes de conexión localmente y como proxy RADIUS, reenviando algunas solicitudes de conexión a un grupo de servidores RADIUS remotos, agregue una nueva Directiva mediante el siguiente procedimiento y, a continuación, compruebe que el valor predeterminado es la Directiva de solicitud de conexión es la última Directiva procesada colocándolo en último lugar en la lista de directivas.

## <a name="add-a-connection-request-policy"></a>Agregar una directiva de solicitud de conexión

Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

### <a name="to-add-a-new-connection-request-policy"></a>Para agregar una nueva Directiva de solicitud de conexión 

1. En Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de redes** para abrir la consola NPS. 
2. En el árbol de consola, haga doble clic en **directivas**.
3. Haga clic con el botón secundario en **directivas de solicitud de conexión**y luego haga clic en **nueva Directiva de solicitud de conexión**.
4. Use el Asistente para nueva Directiva de solicitud de conexión para configurar la Directiva de solicitud de conexión y, si no se ha configurado previamente, un grupo de servidores RADIUS remotos.


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

