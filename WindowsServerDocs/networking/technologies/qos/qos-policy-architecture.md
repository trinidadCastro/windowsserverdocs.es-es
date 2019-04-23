---
title: Arquitectura de la directiva de QoS
description: En este tema se proporciona información general de la directiva de calidad de servicio (QoS), que le permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843146"
---
# <a name="qos-policy-architecture"></a>Arquitectura de la directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información acerca de la arquitectura de directiva de QoS.

En la siguiente ilustración muestra la arquitectura de QoS basada en directivas.

![Arquitectura de directiva de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

La arquitectura de QoS basada en directivas consta de los siguientes componentes:

- **Servicio de cliente de directiva de grupo**. Un servicio de Windows que administra la configuración de directiva de grupo de configuración de usuario y equipo.

- **Motor de directiva de grupo**. Un componente del servicio de cliente de directiva de grupo que recupera la configuración de directiva de grupo de configuración de usuario y equipo de Active Directory durante el inicio comprueba si hay cambios \(de forma predeterminada, cada 90 minutos\). Si se detectan cambios, el motor de directiva de grupo recupera la nueva configuración de directiva de grupo. El motor de directiva de grupo procesa los objetos GPO entrantes y le informa de la extensión del lado cliente de calidad de servicio cuando se actualizan las directivas de QoS.

- **Extensión del lado cliente de QoS**. Un componente de servicio de cliente de directiva de grupo que espera las indicaciones del motor de directiva de grupo que han cambiado las directivas de QoS e informa al módulo de inspección de QoS.

- **Pila TCP/IP**. La pila TCP/IP que incluye compatibilidad integrada para IPv4 e IPv6 y admite la plataforma de filtrado de Windows. 

- **Inspección de QoS**. Componente de un módulo dentro de la pila de TCP/IP que espera indicaciones de cambios en la directiva QoS de la extensión del lado cliente de QoS, recupera la configuración de directiva de QoS e interactúa con la capa de transporte y Pacer.sys para marcar de forma interna el tráfico que coincide con la calidad de servicio directivas.

- **NDIS 6.x**. Una interfaz estándar entre controladores de red de modo kernel y el sistema operativo en sistemas operativos Windows Server y cliente. NDIS 6.x admite filtros ligeros, que es un modelo de controladores simplificados para controladores intermedios de NDIS y controladores de minipuerto que proporciona un mejor rendimiento.

- **Interfaz del proveedor de red de QoS \(NPI\)**. Una interfaz para controladores de modo de núcleo que interactúa con Pacer.sys.

- **Pacer.sys**. Un controlador de filtro ligero de NDIS 6.x que controla la programación de paquetes QoS basada en directivas y para el tráfico de las aplicaciones que usan el genéricas de QoS \(GQoS\) y Control de tráfico \(TC\) API. Pacer.sys sustituye a Psched.sys en Windows Server 2003 y Windows XP. Pacer.sys se instala con el componente Programador de paquetes QoS desde las propiedades de una conexión de red o un adaptador.

El siguiente tema en esta guía, consulte [escenarios de directiva de QoS](qos-policy-scenarios.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).

