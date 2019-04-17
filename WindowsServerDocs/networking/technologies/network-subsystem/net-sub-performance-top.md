---
title: Rendimiento del subsistema de red optimización
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>Rendimiento del subsistema de red optimización

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información general del subsistema de red y vínculos a otros temas de esta guía.

>[!NOTE]
>Además de este tema, las siguientes secciones de esta guía proporcionan recomendaciones para optimizar el rendimiento de los dispositivos de red y la pila de red.
> - [Elección de un adaptador de red](net-sub-choose-nic.md)
> - [Configurar el orden de las Interfaces de red](net-sub-interface-metric.md)
> - [Adaptadores de red de optimización de rendimiento](net-sub-performance-tuning-nics.md)
> - [Contadores de rendimiento relacionados con la red](net-sub-performance-counters.md)
> - [Herramientas de rendimiento de las cargas de trabajo de la red](net-sub-performance-tools.md)

El subsistema de red, especialmente para cargas de trabajo intensivo de red, el ajuste del rendimiento puede implicar cada capa de la arquitectura de red, que también se denomina la pila de red. Estas capas ampliamente se dividen en las siguientes secciones.

1. **Interfaz de red**. Este es el nivel más bajo en la pila de red y contiene el controlador de red que se comunica directamente con el adaptador de red.

2. **Especificación de interfaz de controlador (NDIS) de la red**. NDIS expone interfaces el controlador, debajo de ella así como para las capas encima de él, por ejemplo, la pila de protocolo.
  
3. **Pila de protocolo**. La pila de protocolo implementa protocolos, como TCP/IP y UDP/IP. Estas capas exponen la interfaz de la capa de transporte para las capas sobre ellos.
  
4. **Controladores del sistema**. Por lo general, estos son los clientes que usan una extensión de datos de transporte (TDX) o la interfaz de Winsock Kernel (WSK) para exponer interfaces a aplicaciones de modo de usuario. La interfaz WSK se habilitó en Windows Server 2008 y Windows&reg; Vista y se exponen AFD.sys. La interfaz de mejora el rendimiento al eliminar el cambio entre el modo de usuario y modo kernel.
  
5. **Aplicaciones en modo usuario**. Normalmente son soluciones de Microsoft o aplicaciones personalizadas.

La siguiente tabla proporciona una ilustración vertical de las capas de la pila de red, incluidos ejemplos de elementos que se ejecutan en cada capa.  

![Capas de la pila de red](../../media/Network-Subsystem/network-layers.jpg)

