---
title: Grupos de servidores RADIUS remotos
description: En este tema se proporciona información general de los grupos de servidores RADIUS remotos del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63a6eb5f0f78ed8dbcc0144602f16274fd6ec213
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396310"
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando se configura el servidor de directivas de redes (NPS) como un proxy de Servicio de autenticación remota telefónica de usuario (RADIUS), se usa NPS para reenviar las solicitudes de conexión a los servidores RADIUS que son capaces de procesar las solicitudes de conexión porque pueden realizar autenticación y autorización en el dominio donde se encuentra la cuenta de usuario o de equipo. Por ejemplo, si desea reenviar las solicitudes de conexión a uno o varios servidores RADIUS de dominios que no son de confianza, puede configurar NPS como un proxy RADIUS para reenviar las solicitudes a los servidores RADIUS remotos en el dominio que no es de confianza.

>[!NOTE]
>Los grupos de servidores RADIUS remotos no están relacionados con grupos de Windows ni se separan de ellos.

Para configurar NPS como un proxy RADIUS, debe crear una directiva de solicitud de conexión que contenga toda la información necesaria para que NPS evalúe los mensajes que se van a reenviar y la ubicación de envío de los mensajes.

Al configurar un grupo de servidores RADIUS remotos en NPS y configurar una directiva de solicitud de conexión con el grupo, está designando la ubicación donde NPS va a reenviar las solicitudes de conexión.

## <a name="configuring-radius-servers-for-a-group"></a>Configuración de servidores RADIUS para un grupo

Un grupo de servidores RADIUS remotos es un grupo con nombre que contiene uno o varios servidores RADIUS. Si configura más de un servidor, puede especificar la configuración de equilibrio de carga para determinar el orden en el que el proxy usa los servidores o distribuir el flujo de mensajes RADIUS en todos los servidores del grupo para evitar la sobrecarga de uno o más servidores. con demasiadas solicitudes de conexión.

Cada servidor del grupo tiene la siguiente configuración.

- **Nombre o dirección**. Cada miembro del grupo debe tener un nombre único dentro del grupo. El nombre puede ser una dirección IP o un nombre que se pueda resolver en su dirección IP.

- **Autenticación y cuentas**. Puede Reenviar solicitudes de autenticación, solicitudes de cuentas o ambas a cada miembro del grupo de servidores RADIUS remotos.

- **Equilibrio de carga**. Se usa un valor de prioridad para indicar qué miembro del grupo es el servidor principal (la prioridad se establece en 1). En el caso de los miembros del grupo que tienen la misma prioridad, se usa un valor de peso para calcular la frecuencia con que se envían mensajes RADIUS a cada servidor. Puede usar valores de configuración adicionales para configurar el modo en que NPS detecta cuándo un miembro del grupo deja de estar disponible y cuándo está disponible una vez que se ha determinado que no está disponible.

Después de configurar un grupo de servidores RADIUS remotos, puede especificar el grupo en la configuración de autenticación y cuentas de una directiva de solicitud de conexión. Por este motivo, puede configurar un grupo de servidores RADIUS remotos primero. A continuación, puede configurar la Directiva de solicitud de conexión para usar el grupo de servidores RADIUS remotos recién configurados. Como alternativa, puede usar el Asistente para nueva Directiva de solicitud de conexión para crear un nuevo grupo de servidores RADIUS remotos mientras crea la Directiva de solicitud de conexión.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
