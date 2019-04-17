---
title: Configurar las directivas de solicitud de conexión
description: Este tema proporciona información sobre cómo configurar las directivas de solicitud de conexión en el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f62c6a67-4dda-47f8-8bdf-9b76c37953e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9677e147bdaea4de71a054cd6c52d81126e005d1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-connection-request-policies"></a>Configurar las directivas de solicitud de conexión

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para crear y configurar las directivas de la solicitud de conexión que indique si el servidor NPS local procesa las solicitudes de conexión o reenvía al servidor RADIUS remoto para su procesamiento.

Directivas de solicitud de conexión son conjuntos de condiciones y opciones de configuración que permiten a los administradores de red designar qué servidores de servicio de autenticación remota telefónica de usuario (RADIUS) realizan la autenticación y autorización de solicitudes de conexión que el servidor que ejecuta el servidor de directivas de red \(NPS\) recibe de los clientes RADIUS.

La directiva de solicitud de conexión predeterminada usa NPS como un servidor RADIUS y procesa todas las solicitudes de autenticación localmente.

Para configurar un servidor que ejecuta NPS para que actúe como un proxy RADIUS y solicitudes de conexión directa a otros NPS o servidores RADIUS, debes configurar un grupo de servidores RADIUS remoto además de agregar una nueva directiva de solicitud de conexión que especifica las condiciones y opciones de configuración que deben coincidir con las solicitudes de conexión.

Puedes crear un nuevo grupo de servidores RADIUS remoto mientras vas a crear una nueva directiva de solicitud de conexión con el Asistente para nueva directiva de solicitud de conexión.

Si no desea que el servidor NPS para que actúe como localmente las solicitudes de una conexión de servidor y el proceso de radio, puede eliminar la directiva de solicitud de conexión predeterminada.

Si quieres que el servidor NPS para actuar como servidor RADIUS, procesamiento de solicitudes de conexión localmente y como proxy RADIUS, reenviar algunas solicitudes de conexión a un grupo de servidores RADIUS remotos, agrega una nueva directiva mediante el procedimiento siguiente y, a continuación, comprueba que la directiva de solicitud de conexión predeterminada es la última directiva procesada por última colocándolo en la lista de directivas.

## <a name="add-a-connection-request-policy"></a>Agregar una directiva de solicitud de conexión

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-add-a-new-connection-request-policy"></a>Para agregar una nueva directiva de solicitud de conexión 

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red** para abrir la consola NPS. 
2. En el árbol de consola, haz doble clic en **directivas**.
3. Haz clic en **directivas de solicitud de conexión**y, a continuación, haz clic en **nueva directiva de solicitud de conexión**.
4. Uso de solicitud directiva Asistente para configurar la conexión de directiva de solicitud y, si no configurado anteriormente, un grupo de servidores RADIUS remotos.


Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).

