---
title: Cómo funciona la directiva de QoS
description: Este tema proporciona una visión general de la directiva de calidad de servicio (QoS), que permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>Cómo funciona la directiva de QoS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Al iniciar o al obtener la configuración de directiva de grupo de configuración del equipo para QoS o de usuario actualizada, se produce el siguiente proceso.

1. El motor de directiva de grupo, recupera la configuración de directiva de grupo de configuración de usuario o equipo de Active Directory.

2. El motor de directiva de grupo informa la extensión del lado cliente QoS que se produjeron cambios en las directivas de QoS.

3. La extensión del lado cliente QoS envía una notificación de eventos de directiva de QoS para el módulo de inspección de QoS.

4. El módulo de inspección de QoS recupera las directivas de equipo o usuario QoS y los almacena.

Cuando un nuevo extremo de la capa de transporte \ (conexión TCP o UDP traffic\) se crea, se produce el siguiente proceso.

1. El componente de la capa de transporte de la pila TCP/IP informa al módulo de inspección de QoS.

2. El módulo de inspección de QoS compara los parámetros del extremo de capa de transporte en las directivas de QoS almacenadas.

3. Si se encuentra una coincidencia, el módulo de inspección de QoS contactos Pacer.sys para crear un flujo, una estructura de datos que contiene el valor DSCP y el tráfico de límite de la directiva de QoS coincidente. Si hay varias directivas de QoS que coinciden con los parámetros del extremo de la capa de transporte, se usa la directiva de QoS más específica.

4. Pacer.sys almacena el flujo y devuelve el número de flujo correspondiente para el flujo para el módulo de inspección de QoS.

5. El módulo de inspección de QoS devuelve el número de flujo a la capa de transporte.

6. La capa de transporte se almacena el número de flujo con el extremo de la capa de transporte.

Cuando un paquete correspondiente a un extremo de la capa de transporte marcados con un número de flujo se envían el siguiente proceso se produce.

1. La capa de transporte marca internamente el paquete con el número de flujo.

2. La capa de red consulta Pacer.sys el valor DSCP correspondiente al número de flujo del paquete.

3. Pacer.sys devuelve el valor DSCP a la capa de red.

4. La capa de red cambia el campo de procedimientos de IPv4 o IPv6 tráfico clase al valor DSCP especificado por Pacer.sys y, para los paquetes de IPv4, calcula la suma de comprobación de encabezado IPv4 final.

5. La capa de red entrega el paquete a la capa tramas.

6. Dado que el paquete se ha marcado con un número de flujo, la capa tramas entrega el paquete a Pacer.sys a través de NDIS 6.x.

7. Pacer.sys usa el número de flujo del paquete para determinar si el paquete debe estar limitada y si es así, programa el paquete de envío.

8. Pacer.sys entrega el paquete ya sea inmediatamente \ (si no hay ningún throttling\ tráfico) o programada como \ (si hay tráfico throttling\) NDIS 6.x para la transmisión a través del adaptador de red apropiada.

Estos procesos de QoS basada en directivas proporcionan las siguientes ventajas.

- Se realiza la inspección de tráfico para determinar si se aplica una directiva de QoS extremo de la capa de transporte, en lugar de por paquete.

- No hay ningún impacto en el rendimiento para el tráfico que no coincide con una directiva de QoS.

- Las aplicaciones no deben modificarse para aprovechar las ventajas de servicio diferenciado basado en DSCP o la limitación del tráfico.

- Aplicarán las directivas de QoS al tráfico protegido con IPsec.

El siguiente tema en esta guía, encontrarás [arquitectura de la directiva de QoS](qos-policy-architecture.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).
