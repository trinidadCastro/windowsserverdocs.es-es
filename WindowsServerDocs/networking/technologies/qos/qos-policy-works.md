---
title: Cómo funciona la Directiva de QoS
description: En este tema se proporciona información general sobre la Directiva de calidad de servicio (QoS), que permite usar directiva de grupo para priorizar el ancho de banda del tráfico de red de aplicaciones y servicios específicos en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4de9674e2d1700d342af380c79a611c3d5961cda
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405180"
---
# <a name="how-qos-policy-works"></a>Cómo funciona la Directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al iniciar o obtener la configuración actualizada de directiva de grupo de usuario o equipo para QoS, se produce el siguiente proceso.

1. El motor de directiva de grupo recupera la configuración del usuario o del equipo directiva de grupo de Active Directory.

2. El motor de directiva de grupo informa a la extensión del lado cliente de QoS de que hubo cambios en las directivas de QoS.

3. La extensión del lado cliente QoS envía una notificación de eventos de directiva QoS al módulo de inspección de QoS.

4. El módulo de inspección de QoS recupera las directivas QoS de usuario o equipo y las almacena.

Cuando se crea un nuevo extremo de capa de transporte \(conexión TCP o\) de tráfico UDP, se produce el siguiente proceso.

1. El componente de capa de transporte de la pila TCP/IP informa al módulo de inspección de QoS.

2. El módulo de inspección de QoS compara los parámetros del punto de conexión de la capa de transporte con las directivas de QoS almacenadas.

3. Si se encuentra una coincidencia, el módulo de inspección de QoS se pone en contacto con Pacer. sys para crear un flujo, una estructura de datos que contiene el valor de DSCP y la configuración de limitación de tráfico de la Directiva de QoS coincidente. Si hay varias directivas de QoS que coinciden con los parámetros del punto de conexión de la capa de transporte, se usa la Directiva de QoS más específica.

4. Pacer. sys almacena el flujo y devuelve un número de flujo correspondiente al flujo del módulo de inspección de QoS.

5. El módulo de inspección de QoS devuelve el número de flujo a la capa de transporte.

6. La capa de transporte almacena el número de flujo con el punto de conexión de la capa de transporte.

Cuando se envía un paquete correspondiente a un extremo de capa de transporte marcado con un número de flujo, se produce el siguiente proceso.

1. La capa de transporte marca internamente el paquete con el número de flujo.

2. El nivel de red consulta Pacer. sys para el valor de DSCP correspondiente al número de flujo del paquete.

3. Pacer. sys devuelve el valor de DSCP al nivel de red.

4. El nivel de red cambia el campo TOS de IPv4 o la clase de tráfico IPv6 al valor de DSCP especificado por Pacer. sys y, en el caso de los paquetes IPv4, calcula la suma de comprobación final del encabezado IPv4.

5. El nivel de red entrega el paquete a la capa de tramas.

6. Dado que el paquete se ha marcado con un número de flujo, la capa de tramas entrega el paquete a Pacer. sys a través de NDIS 6. x.

7. Pacer. sys usa el número de flujo del paquete para determinar si es necesario limitar el paquete y, en caso afirmativo, programa el paquete para enviarlo.

8. Pacer. sys entrega el paquete inmediatamente \(si no hay ningún límite de tráfico\) o según lo programado \(si hay una limitación de tráfico\) a NDIS 6. x para la transmisión a través del adaptador de red adecuado.

Estos procesos de QoS basada en directivas proporcionan las siguientes ventajas.

- La inspección del tráfico para determinar si se aplica una directiva de QoS se hace por punto de conexión de la capa de transporte, en lugar de por paquete.

- No se produce ningún impacto en el rendimiento del tráfico que no coincide con una directiva de QoS.

- No es necesario modificar las aplicaciones para aprovechar las ventajas del servicio diferenciado basado en DSCP o la limitación del tráfico.

- Las directivas de QoS se pueden aplicar al tráfico protegido con IPsec.

Para el siguiente tema de esta guía, consulte [arquitectura](qos-policy-architecture.md)de la Directiva de QoS.

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).
