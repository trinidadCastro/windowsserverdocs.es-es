---
title: Ajuste de rendimiento del subsistema de red
description: Obtenga información sobre el subsistema de red y vínculos a otros temas de esta guía.
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: 8684630634dcb1d1a5998f69f98ce16dd32b7f38
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040445"
---
# <a name="network-subsystem-performance-tuning"></a>Ajuste de rendimiento del subsistema de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general sobre el subsistema de red y vínculos a otros temas de esta guía.

>[!NOTE]
>Además de este tema, las siguientes secciones de esta guía proporcionan recomendaciones para la optimización del rendimiento de los dispositivos de red y la pila de red.
> - [Elección de un adaptador de red](net-sub-choose-nic.md)
> - [Configurar el orden de las interfaces de red](net-sub-interface-metric.md)
> - [Optimizar el rendimiento de los adaptadores de red](net-sub-performance-tuning-nics.md)
> - [Contadores de rendimiento relacionados con la red](net-sub-performance-counters.md)
> - [Herramientas de rendimiento de las cargas de trabajo de la red](net-sub-performance-tools.md)

Ajustar el rendimiento del subsistema de red, especialmente en el caso de cargas de trabajo intensivas de red, puede implicar cada nivel de la arquitectura de red, que también se denomina pila de red. Estas capas se dividen ampliamente en las secciones siguientes.

1. **Interfaz de red**. Esta es la capa más baja de la pila de red y contiene el controlador de red que se comunica directamente con el adaptador de red.

2. **Especificación de interfaz de controlador de red (NDIS)**. NDIS expone interfaces para el controlador que está por debajo y para las capas que hay por encima, como la pila de protocolos.

3. **Pila de protocolos**. La pila de protocolos implementa protocolos como TCP/IP y UDP/IP. Estas capas exponen la interfaz de capa de transporte para las capas por encima de ellas.

4. **Controladores del sistema**. Suelen ser clientes que usan una interfaz de extensión de datos de transporte (TDX) o de Winsock kernel (WSK) para exponer interfaces a aplicaciones de modo usuario. La interfaz WSK se presentó en Windows Server 2008 y Windows &reg; vista, y se expone mediante AFD.sys. La interfaz mejora el rendimiento eliminando el cambio entre el modo de usuario y el modo kernel.

5. **Aplicaciones de modo usuario**. Normalmente se trata de soluciones de Microsoft o aplicaciones personalizadas.

En la tabla siguiente se proporciona una ilustración vertical de las capas de la pila de red, incluidos ejemplos de elementos que se ejecutan en cada capa.

![Capas de la pila de red](../../media/Network-Subsystem/network-layers.jpg)

