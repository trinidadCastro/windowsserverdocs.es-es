---
title: Ajuste del rendimiento del subsistema de red
description: En este tema forma parte de la Guía de ajuste de rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857416"
---
# <a name="network-subsystem-performance-tuning"></a>Ajuste del rendimiento del subsistema de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general del subsistema de red y los vínculos a otros temas de esta guía.

>[!NOTE]
>Además de este tema, las secciones siguientes de esta guía proporcionan recomendaciones de optimización de rendimiento para los dispositivos de red y la pila de red.
> - [Elección de un adaptador de red](net-sub-choose-nic.md)
> - [Configurar el orden de las Interfaces de red](net-sub-interface-metric.md)
> - [Adaptadores de red de optimización de rendimiento](net-sub-performance-tuning-nics.md)
> - [Contadores de rendimiento relacionados con la red](net-sub-performance-counters.md)
> - [Herramientas de rendimiento para cargas de trabajo de red](net-sub-performance-tools.md)

El subsistema de red, especialmente para cargas de trabajo intensivas de red, de optimización del rendimiento puede implicar cada capa de la arquitectura de red, que también se denomina la pila de red. Estas capas se dividen en las secciones siguientes.

1. **Interfaz de red**. Esta es la capa inferior de la pila de red y contiene el controlador de red que se comunica directamente con el adaptador de red.

2. **Especificación de interfaz de controlador (NDIS) de red**. NDIS expone interfaces para el controlador debajo de él y para las capas por encima de él, como la pila del protocolo.
  
3. **Pila de protocolo**. La pila del protocolo implementa los protocolos como TCP/IP y UDP/IP. Estas capas exponen la interfaz de capa de transporte para las capas por encima de ellos.
  
4. **Los controladores del sistema**. Normalmente, estos son los clientes que usan una extensión de datos de transporte (TDX) o la interfaz de Winsock Kernel (WSK) para exponer interfaces para las aplicaciones en modo de usuario. La interfaz de WSK se introdujo en Windows Server 2008 y Windows&reg; Vista y se expone mediante AFD.sys. La interfaz mejora el rendimiento mediante la eliminación de la conmutación entre el modo de usuario y el modo de kernel.
  
5. **Aplicaciones en modo usuario**. Estos son normalmente las soluciones de Microsoft o aplicaciones personalizadas.

En la tabla siguiente proporciona una ilustración vertical de las capas de la pila de red, incluidos ejemplos de elementos que se ejecutan en cada capa.  

![Capas de la pila de red](../../media/Network-Subsystem/network-layers.jpg)

