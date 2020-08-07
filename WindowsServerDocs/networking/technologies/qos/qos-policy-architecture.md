---
title: Arquitectura de la directiva QoS
description: En este tema se proporciona información general sobre la Directiva de calidad de servicio (QoS), que permite usar directiva de grupo para priorizar el ancho de banda del tráfico de red de aplicaciones y servicios específicos en Windows Server 2016.
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: eca49b5b20e34aca9c5e65b1544f2308295d7445
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953871"
---
# <a name="qos-policy-architecture"></a>Arquitectura de la directiva QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre la arquitectura de la directiva QoS.

En la ilustración siguiente se muestra la arquitectura de QoS basada en directivas.

![Arquitectura de la directiva QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

La arquitectura de QoS basada en Directiva consta de los siguientes componentes:

- **Directiva de grupo servicio de cliente**. Un servicio de Windows que administra la configuración de directiva de grupo de usuario y equipo.

- **Motor de directiva de grupo**. Componente del servicio de cliente de directiva de grupo que recupera la configuración del usuario y del equipo directiva de grupo de Active Directory al inicio y comprueba periódicamente si hay cambios de \( forma predeterminada, cada 90 minutos \) . Si se detectan cambios, el motor de directiva de grupo recupera la nueva configuración de directiva de grupo. El motor de directiva de grupo procesa los GPO entrantes e informa a la extensión del lado cliente QoS cuando se actualizan las directivas QoS.

- **Extensión del lado cliente QoS**. Componente del servicio de cliente de directiva de grupo que espera una indicación del motor de directiva de grupo que las directivas de QoS han cambiado e informa al módulo de inspección de QoS.

- **Pila TCP/IP**. La pila TCP/IP que incluye compatibilidad integrada para IPv4 e IPv6 y admite la plataforma de filtrado de Windows.

- **Inspección de QoS**. Módulo un componente dentro de la pila TCP/IP que espera las indicaciones de los cambios de la Directiva de QoS de la extensión del lado cliente QoS, recupera la configuración de la directiva QoS e interactúa con la capa de transporte y Pacer.sys para marcar internamente el tráfico que coincide con las directivas QoS.

- **NDIS 6. x**. Una interfaz estándar entre los controladores de red en modo kernel y el sistema operativo en Windows Server y los sistemas operativos cliente. NDIS 6. x es compatible con los filtros ligeros, que es un modelo de controlador simplificado para controladores intermedios NDIS y controladores de minipuerto que proporcionan un mejor rendimiento.

- Interfaz del proveedor de ** \( red QoS NPI \) **. Interfaz para que los controladores de modo kernel interactúen con Pacer.sys.

- **Pacer.sys**. Un controlador de filtro ligero de NDIS 6. x que controla la programación de paquetes para QoS basada en directivas y para el tráfico de aplicaciones que usan las \( API de control de tráfico y GQoS de QoS genérico \) \( \) . Pacer.sys reemplaza Psched.sys en Windows Server 2003 y Windows XP. Pacer.sys se instala con el componente programador de paquetes QoS de las propiedades de una conexión de red o adaptador.

Para el siguiente tema de esta guía, vea [escenarios de directivas de QoS](qos-policy-scenarios.md).

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).

