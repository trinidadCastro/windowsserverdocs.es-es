---
title: Grupos de servidores RADIUS remotos
description: Este tema proporciona una visión general de la red directiva servidor remoto grupos de servidores RADIUS en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d81678a7-be21-48f2-9b3f-5a75d6aef013
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f27b5e501f110a038264cd54d75c8b8f9566a64
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="remote-radius-server-groups"></a>Grupos de servidores RADIUS remotos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Al configurar el servidor de directivas de redes (NPS) como un proxy de servicio de autenticación remota telefónica de usuario (RADIUS), puedes usar NPS para reenviar las solicitudes de conexión a servidores RADIUS que son capaces de procesar las solicitudes de conexión porque pueden realizar la autenticación y autorización en el dominio donde se encuentra la cuenta de usuario o equipo. Por ejemplo, si quieres reenviar solicitudes de conexión a uno o varios servidores RADIUS en dominios de confianza, puede configurar NPS como proxy RADIUS para reenviar las solicitudes a los servidores RADIUS remotos en el dominio de confianza.

>[!NOTE]
>Grupos de servidores RADIUS remotos son relacionada con la y la separa de grupos de Windows.

Para configurar NPS como un proxy RADIUS, debes crear una directiva de solicitud de conexión que contiene toda la información necesaria para que NPS evalúe qué mensajes reenviar y dónde enviar los mensajes.

Al configurar un grupo de servidores RADIUS remotos en NPS y configurar una directiva de solicitud de conexión con el grupo, se designa la ubicación donde se NPS reenviar las solicitudes de conexión.

## <a name="configuring-radius-servers-for-a-group"></a>Configuración de servidores RADIUS para un grupo

Un grupo de servidores RADIUS remotos es un grupo denominado que contiene uno o varios servidores RADIUS. Si configuras más de un servidor, puedes especificar opciones de configuración para determinar el orden en que se utilizan los servidores mediante el proxy o para distribuir el flujo de mensajes de radio en todos los servidores en el grupo para evitar la sobrecarga de uno o varios servidores con demasiadas solicitudes de conexión de equilibrio de carga.

Cada servidor en el grupo tiene la siguiente configuración.

- **Nombre o dirección**. Cada miembro del grupo debe tener un nombre único en el grupo. El nombre puede ser una dirección IP o un nombre que se puede resolver a su dirección IP.

- **Autenticación y cuentas**. Puede reenviar solicitudes de autenticación, las solicitudes de cuentas o ambos para cada miembro del grupo de servidores RADIUS remoto.

- **Equilibrio de carga**. Una configuración de prioridad se usa para indicar el miembro del grupo es el servidor principal (la prioridad se establece en 1). Los miembros del grupo que tienen la misma prioridad, se usa una opción de peso para calcular ¿con qué frecuencia se envían mensajes RADIUS a cada servidor. Puedes usar opciones de configuración adicionales para configurar el modo en que el servidor NPS detecta cuando un miembro del grupo, primero no esté disponible y cuando esté disponible después de se ha determinado que no estén disponibles.

Después de haber configurado un grupo de servidores RADIUS remotos, puedes especificar el grupo en la autenticación y la configuración de las cuentas de una directiva de solicitud de conexión. Por este motivo, puedes configurar un grupo de servidores RADIUS remotos en primer lugar. A continuación, puedes configurar la directiva de solicitud de conexión para usar el grupo de servidores RADIUS remotos recientemente configurado. Como alternativa, puedes usar al Asistente para nueva directiva de solicitud de conexión para crear un nuevo grupo de servidores RADIUS remoto mientras vas a crear la directiva de solicitud de conexión.

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
