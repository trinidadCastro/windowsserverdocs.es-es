---
title: Arquitectura de la directiva de QoS
description: Este tema proporciona una visión general de la directiva de calidad de servicio (QoS), que permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>Arquitectura de la directiva de QoS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre la arquitectura de directiva de QoS.

La siguiente figura muestra la arquitectura de QoS basada en directivas.

![Arquitectura de directiva de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

La arquitectura de QoS basada en directivas consta de los siguientes componentes:

- **Servicio de cliente de directiva de grupo**. Un servicio de Windows que administra la configuración de directiva de grupo de configuración de usuario y del equipo.

- **Motor de directiva de grupo**. Un componente del servicio de cliente de directiva de grupo que recupera la configuración de directiva de grupo de configuración de usuarios y equipos de Active Directory durante el inicio busca cambios \ (de manera predeterminada, cada 90 minutes\). Si se detectan cambios, el motor de directiva de grupo recupera la nueva configuración de directiva de grupo. El motor de directiva de grupo procesa los GPO entrantes e informa la extensión del lado cliente QoS cuando se actualizan las directivas de QoS.

- **Extensión del lado cliente de QoS**. Un componente de servicio de cliente de directiva de grupo que espera una indicación del motor de directiva de grupo que han cambiado las directivas de QoS e informa al módulo de inspección de QoS.

- **Pila TCP/IP**. La pila TCP/IP que incluye compatibilidad integrada para IPv4 e IPv6 y admite la plataforma de filtrado de Windows. 

- **Inspección de QoS**. Componente de un módulo de la pila TCP/IP que espera indicaciones QoS de cambios de directiva de la extensión del lado cliente QoS, recupera la configuración de directiva de QoS e interactúa con la capa de transporte y Pacer.sys para marcar el tráfico que coincida con las directivas de QoS de forma interna.

- **NDIS 6.x**. Una interfaz estándar entre los controladores de red de modo kernel y el sistema operativo en sistemas operativos cliente y servidor de Windows. NDIS 6.x admite ligeros filtros, que es un modelo de controlador simplificada para controladores de minipuerto y controladores intermedios NDIS que proporciona un mejor rendimiento.

- **Interfaz de proveedor de red de QoS \(NPI\)**. Una interfaz para controladores de modo kernel interactuar con Pacer.sys.

- **Pacer.sys**. Un controlador de filtro ligero 6.x NDIS que controla la programación de paquetes QoS basada en directivas y para el tráfico de las aplicaciones que usan el \(GQoS\) QoS genérico y el Control de tráfico \(TC\) API. Pacer.sys había reemplazado Psched.sys en Windows Server 2003 y Windows XP. Pacer.sys se instala con el componente de programador de paquetes QoS en las propiedades de una conexión de red o el adaptador.

El siguiente tema en esta guía, encontrarás [escenarios de la directiva de QoS](qos-policy-scenarios.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).

