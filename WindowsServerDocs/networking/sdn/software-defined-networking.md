---
title: Redes definidas por software (SDN)
description: Redes definidas por software (SDN) proporciona un método para configurar y administrar dispositivos de red virtual y física de manera centralizada, como enrutadores, conmutadores y puertas de enlace en su centro de datos. Use este tema para obtener información sobre las tecnologías de redes definidas por software (SDN) que se proporcionan en Windows Server, System Center y Microsoft Azure.
manager: grcusanz
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/09/2018
ms.openlocfilehash: aa5ff02e7bbb790b8d7af2b2dd7d7f478c2e3d0a
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716961"
---
# <a name="sdn-in-windows-server-overview"></a>Introducción a SDN en Windows Server

>Se aplica a: Windows Server 2019, Windows Server 2016


Redes definidas por software (SDN) proporciona un método para configurar y administrar dispositivos de red virtual y física de manera centralizada, como enrutadores, conmutadores y puertas de enlace en su centro de datos. Puede usar los dispositivos compatibles con SDN existentes para lograr una mayor integración entre la red virtual y la red física. Los elementos de red virtual como el conmutador virtual de Hyper-V, la virtualización de red de Hyper-V y la puerta de enlace de RAS están diseñados para ser elementos integrales de la infraestructura de SDN.

>[!Note]
>Los hosts de Hyper-V y las máquinas virtuales (VM) que ejecutan servidores de infraestructura de SDN, como los nodos de controladora de red y de equilibrio de carga de software, deben tener instalado Windows Server 2019 o 2016 Datacenter Edition.
>
>Los hosts de Hyper-V que contienen solo máquinas virtuales de carga de trabajo de inquilinos conectadas a redes controladas por SDN pueden usar Windows Server 2019 o 2016 Standard Edition.

SDN es posible porque los planos de red ya no están enlazados a los propios dispositivos de red. Sin embargo, otras entidades, como software de administración de centros de aplicaciones como System Center 2016, usan los planos de red. SDN le permite administrar la red del centro de trabajo de forma dinámica, lo que proporciona una manera automatizada y centralizada de cumplir los requisitos de las aplicaciones y las cargas de trabajo.

Puede usar SDN para:

- Cree, proteja y conecte dinámicamente su red para satisfacer las necesidades en constante evolución de sus aplicaciones.
- Acelere la implementación de las cargas de trabajo de forma no disruptiva
- Contener vulnerabilidades de seguridad de la propagación a través de la red
- Definir y controlar las directivas que rigen las redes físicas y virtuales
- Implementar directivas de red de forma coherente a escala

SDN le permite llevar a cabo todo esto, a la vez que reduce los costos generales de la infraestructura.



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Póngase en contacto con el equipo de productos de redes de nube y centros de trabajo

Si está interesado en hablar sobre las tecnologías de SDN con Microsoft u otros clientes de SDN, hay varios métodos para hacer contacto.

Para obtener más información, consulte [ponerse en contacto con el equipo de redes de centro de datos y nube](contact-sdn-team.md).
