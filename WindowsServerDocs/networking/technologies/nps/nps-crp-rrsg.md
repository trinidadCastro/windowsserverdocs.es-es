---
title: Grupos de servidores RADIUS remotos
description: En este tema se proporciona información general de red directiva de servidor RADIUS grupos de servidores remotos en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9912927a7b75e4c9f04aa3d24eb7ed46c73a7dd2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855266"
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando se configura el servidor de directivas de redes (NPS) como un proxy de servicio de autenticación remota telefónica de usuario (RADIUS), se usa NPS para reenviar las solicitudes de conexión a servidores RADIUS que son capaces de procesar las solicitudes de conexión porque pueden realizar autenticación y autorización en el dominio donde se encuentra la cuenta de usuario o equipo. Por ejemplo, si desea reenviar solicitudes de conexión a uno o más servidores RADIUS en dominios de confianza, puede configurar NPS como un proxy RADIUS para reenviar las solicitudes a los servidores RADIUS remotos en el dominio de confianza.

>[!NOTE]
>Grupos de servidores RADIUS remotos son relacionada e independiente de los grupos de Windows.

Para configurar NPS como proxy RADIUS, debe crear una directiva de solicitud de conexión que contiene toda la información necesaria para que NPS evalúe qué mensajes reenviar y dónde enviar los mensajes.

Al configurar un grupo de servidores RADIUS remotos en NPS y configurar una directiva de solicitud de conexión con el grupo, que designa la ubicación donde NPS reenviar las solicitudes de conexión.

## <a name="configuring-radius-servers-for-a-group"></a>Configuración de servidores RADIUS para un grupo

Un grupo de servidores RADIUS remotos es un grupo con nombre que contiene uno o más servidores RADIUS. Si configura varios servidores, puede especificar valores para determinar el orden en que se usan los servidores por el proxy o para distribuir el flujo de mensajes RADIUS entre todos los servidores del grupo para evitar la sobrecarga de uno o más servidores de equilibrio de carga con demasiadas solicitudes de conexión.

Cada servidor en el grupo tiene los siguientes valores.

- **Nombre o dirección**. Cada miembro del grupo debe tener un nombre único dentro del grupo. El nombre puede ser una dirección IP o un nombre que se puede resolver en su dirección IP.

- **Autenticación y cuentas**. Puede reenviar las solicitudes de autenticación, las solicitudes de cuentas o ambos a cada miembro del grupo de servidores RADIUS remotos.

- **Equilibrio de carga**. Un valor de prioridad se utiliza para indicar el miembro del grupo es el servidor principal (la prioridad se establece en 1). Los miembros del grupo que tienen la misma prioridad, se usa una opción de peso para calcular con qué frecuencia se envían mensajes RADIUS a cada servidor. Puede usar una configuración adicional para configurar la forma en que el NPS detecta cuando un miembro del grupo deja de estar disponible y cuando esté disponible una vez que se ha determinado que no esté disponible.

Después de haber configurado un grupo de servidores RADIUS remotos, puede especificar el grupo en la autenticación y la configuración de cuentas de una directiva de solicitud de conexión. Por este motivo, puede configurar un grupo de servidores RADIUS remotos en primer lugar. A continuación, puede configurar la directiva de solicitud de conexión para usar el grupo de servidores remotos RADIUS recién configurado. Como alternativa, puede usar al Asistente para nueva directiva de solicitud de conexión para crear un nuevo grupo de servidores remotos RADIUS mientras crea la directiva de solicitud de conexión.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
