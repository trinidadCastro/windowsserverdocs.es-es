---
title: Grupos de servidores RADIUS remotos
description: En este tema se proporciona información general de los grupos de servidores RADIUS remotos del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b41697afe6d4f4e0c80dce8429bd7a4494c89284
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949011"
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al configurar el servidor de directivas de redes (NPS) como un proxy de Servicio de autenticación remota telefónica de usuario (RADIUS), se usa NPS para reenviar las solicitudes de conexión a los servidores RADIUS que son capaces de procesar las solicitudes de conexión porque pueden realizar la autenticación y autorización en el dominio donde se encuentra la cuenta de usuario o equipo. Por ejemplo, si desea reenviar solicitudes de conexión a uno o varios servidores RADIUS en dominios que no son de confianza, puede configurar NPS como un proxy RADIUS para reenviar las solicitudes a los servidores RADIUS remotos en el dominio que no es de confianza.

>[!NOTE]
>Los grupos de servidores RADIUS remotos no están relacionados con grupos de Windows ni se separan de ellos.

Para configurar NPS como un proxy RADIUS, debe crear una directiva de solicitud de conexión que contenga toda la información necesaria para que NPS evalúe qué mensajes reenviar y dónde enviar los mensajes.

Al configurar un grupo de servidores RADIUS remotos en NPS y configurar una directiva de solicitud de conexión con el grupo, se designa la ubicación donde NPS debe reenviar las solicitudes de conexión.

## <a name="configuring-radius-servers-for-a-group"></a>Configuración de servidores RADIUS para un grupo

Un grupo de servidores RADIUS remotos es un grupo con nombre que contiene uno o varios servidores RADIUS. Si se configura más de un servidor, se puede especificar la configuración de equilibrio de carga para determinar el orden en que el proxy usa los servidores o para distribuir el flujo de mensajes RADIUS entre todos los servidores del grupo para evitar sobrecargar a uno o varios servidores con demasiadas solicitudes de conexión.

Cada servidor del grupo tiene la siguiente configuración.

- **Nombre o dirección**. Cada miembro del grupo debe tener un nombre único dentro del grupo. El nombre puede ser una dirección IP o un nombre que se pueda resolver en su dirección IP.

- **Autenticación y cuentas**. Puede Reenviar solicitudes de autenticación, solicitudes de cuentas o ambas a cada miembro del grupo de servidores RADIUS remotos.

- **Equilibrio de carga**. Se usa una opción de prioridad para indicar el miembro del grupo que es el servidor principal (la prioridad se establece en 1). Para los miembros del grupo que tienen la misma prioridad, se usa una opción de peso para calcular la frecuencia con que se envían mensajes RADIUS a cada servidor. Puede usar valores de configuración adicionales para configurar el modo en que NPS detecta cuándo un miembro del grupo deja de estar disponible y cuándo está disponible una vez que se ha determinado que no está disponible.

Después de configurar un grupo de servidores RADIUS remotos, puede especificar el grupo en la configuración de autenticación y cuentas de una directiva de solicitud de conexión. Por ello, primero se debe configurar un grupo de servidores RADIUS remotos. A continuación, se puede configurar la directiva de solicitud de conexión que se usará con el recién creado grupo de servidores RADIUS remotos. Asimismo, mientras crea la directiva de solicitud de conexión, se puede usar el asistente para nueva directiva de solicitud de conexión para crear un nuevo grupo de servidores RADIUS remotos.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
