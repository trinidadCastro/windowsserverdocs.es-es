---
title: Redes definidas por software (SDN)
description: Redes definidas por software (SDN) proporciona un método para configurar y administrar dispositivos de red virtual y física de manera centralizada, como enrutadores, conmutadores y puertas de enlace en su centro de datos. Use este tema para obtener información acerca de las tecnologías de redes definidas por Software (SDN) que se proporcionan en Windows Server, System Center y Microsoft Azure.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.date: 08/09/2018
ms.openlocfilehash: a6c4db97b1c55d5114eba09251685ef0f2b840bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855236"
---
# <a name="sdn-in-windows-server-overview"></a>Introducción a SDN en Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


Redes definidas por software (SDN) proporciona un método para configurar y administrar dispositivos de red virtual y física de manera centralizada, como enrutadores, conmutadores y puertas de enlace en su centro de datos. Puede usar los dispositivos compatibles con SDN existentes para lograr una mayor integración entre la red virtual y la red física. Elementos de la red virtual, como el conmutador Virtual de Hyper-V, virtualización de red de Hyper-V y puerta de enlace RAS están diseñados para ser elementos integrales de la infraestructura de SDN. 

>[!Note]
>Hosts de Hyper-V y máquinas virtuales (VM) ejecutando los servidores de infraestructura SDN, como nodos de controladora de red y el equilibrio de carga de Software, debe tener instalado de Windows Server 2016 Datacenter edition. 
>
>Que contiene solo inquilino carga de trabajo de máquinas virtuales conectado a redes de bajo control de SDN de hosts de Hyper-V pueden usar Windows Server 2016 Standard edition.

SDN es posible porque los planos de red ya no están enlazados a los propios dispositivos de red. Sin embargo, otras entidades, como software de administración de centro de datos como System Center 2016 usan planos de red. SDN le permite administrar la red de centro de datos de forma dinámica, que proporciona una manera automatizada y centralizada para cumplir los requisitos de las aplicaciones y cargas de trabajo. 

Puede usar SDN para:

- Crear, proteger y conectar la red para satisfacer las necesidades en constante evolución de las aplicaciones dinámicamente
- Acelerar la implementación de las cargas de trabajo de una manera sin interrupciones
- Contener vulnerabilidades de seguridad de propagación a través de la red
- Definir y controlar las directivas que rigen las redes físicas y virtuales 
- Implementar directivas de red de forma coherente a escala

SDN permite llevar a cabo todo esto al tiempo que reduce los costos de infraestructura general.



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Póngase en contacto con el equipo de producto del centro de datos y de red en la nube

Si está interesado en analizar las tecnologías SDN con Microsoft o de otros clientes SDN, hay una variedad de métodos para hacer contacto.

Para obtener más información, consulte [póngase en contacto con el centro de datos y el equipo de redes en la nube](contact-sdn-team.md).
