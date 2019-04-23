---
title: Cómo funciona la directiva de QoS
description: En este tema se proporciona información general de la directiva de calidad de servicio (QoS), que le permite usar la directiva de grupo para establecer prioridades de ancho de banda de tráfico de red de determinadas aplicaciones y servicios en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864286"
---
# <a name="how-qos-policy-works"></a>Cómo funciona la directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al iniciar o al obtener usuario actualizado o directiva de grupo Configuración del equipo de QoS, se produce el siguiente proceso.

1. El motor de directiva de grupo recupera la configuración de directiva de grupo de configuración de usuario o equipo de Active Directory.

2. El motor de directiva de grupo informa a la extensión del lado cliente de QoS que se produjeron cambios en las directivas de QoS.

3. La extensión del lado cliente de QoS envía una notificación de eventos de directiva de QoS al módulo de inspección de QoS.

4. El módulo de inspección de QoS recupera las directivas de QoS de usuario o equipo y los almacena.

Cuando un nuevo extremo Capa transporte \(TCP conexiones o tráfico UDP\) se crea, se produce el siguiente proceso.

1. El componente de nivel de transporte de la pila TCP/IP informa al módulo de inspección de QoS.

2. El módulo de inspección de QoS compara los parámetros del extremo Capa transporte para las directivas de QoS almacenadas.

3. Si se encuentra una coincidencia, el módulo de inspección de QoS se pone en contacto con Pacer.sys para crear un flujo, una estructura de datos que contiene el valor DSCP y la configuración de la directiva de QoS coincidente de limitación de tráfico. Si hay varias directivas de QoS que coinciden con los parámetros del extremo Capa transporte, se usa la directiva de QoS más específica.

4. Pacer.sys almacena el flujo y devuelve el número de flujo correspondiente al flujo de para el módulo de inspección de QoS.

5. El módulo de inspección de QoS devuelve el número de flujo para la capa de transporte.

6. La capa transporte almacena el número de flujo con el punto de conexión de capa de transporte.

Cuando un paquete correspondiente a un extremo Capa Transporte marcado con un número de flujo se envía, se produce el siguiente proceso.

1. La capa transporte marca de forma interna el paquete con el número de flujo.

2. La capa red solicita a Pacer.sys el valor DSCP correspondiente al número de flujo del paquete.

3. Pacer.sys devuelve el valor DSCP para el nivel de red.

4. La capa red cambia el campo TOS de IPv4 o el campo de clase de tráfico de IPv6 para el valor DSCP especificado por Pacer.sys y, para los paquetes de IPv4, calcula la suma de comprobación de encabezado de IPv4 final.

5. La capa red entrega el paquete a la capa tramas.

6. Dado que el paquete se ha marcado con un número de flujo, la capa tramas lo entrega a Pacer.sys a través de NDIS 6.x.

7. Pacer.sys utiliza el número de flujo del paquete para determinar si el paquete necesita o limitación y si es así, se programa el envío.

8. Pacer.sys entrega el paquete ya sea inmediatamente \(si no hay ningún tráfico limitación\) o según lo programado \(si no hay tráfico limitación\) a NDIS 6.x para su transmisión a través del adaptador de red adecuada.

Estos procesos de QoS basada en directivas proporcionan las siguientes ventajas.

- Se realiza la inspección del tráfico para determinar si se aplica una directiva de QoS extremo Capa Transporte por, en lugar de por paquete.

- No hay ningún impacto en el rendimiento para el tráfico que no coincide con una directiva de QoS.

- Aplicaciones no necesitan modificarse para aprovechar las ventajas de servicio diferenciado basado en DSCP o la limitación del tráfico.

- Pueden aplicar las directivas de QoS al tráfico protegido por IPsec.

El siguiente tema en esta guía, consulte [arquitectura de la directiva de QoS](qos-policy-architecture.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).
