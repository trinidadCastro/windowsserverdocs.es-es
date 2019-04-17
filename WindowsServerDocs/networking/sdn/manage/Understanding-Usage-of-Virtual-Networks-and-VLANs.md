---
title: Descripción del uso de redes virtuales y VLAN
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>Descripción del uso de redes virtuales y VLAN

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre redes virtuales de virtualización de red de Hyper-V y cómo se diferencian de las redes de área local virtuales (VLAN).  
  
Software definido de redes (SDN) en Windows Server 2016 se basa en la programación de directivas de redes virtuales de superposición de un conmutador Virtual de Hyper-V. Puedes crear redes virtuales de superposición, también denominadas redes virtuales, con la virtualización de Hyper-V red.   
  
Cuando implementas virtualización de la red de Hyper-V, se crean redes de superposición, encapsulando marco de Ethernet de nivel 2 de la original inquilino de la máquina virtual con una superposición - o bien túnel - encabezado (por ejemplo, VXLAN o NVGRE) y la red de encabezados de la subposición (o físico) IP de capa 3 y Ethernet de nivel 2. Las redes virtuales de superposición se identifican por un 24 bits Virtual red identificador (VNI) para aislar el tráfico de inquilino y para permitir que las direcciones IP superpuestas. La VNI se compone de una subred virtual ID (VSID), el identificador de switch lógico y el Id. de túnel  
  
Además, cada inquilino se asigna a un dominio de enrutamiento (similar de enrutamiento virtual y reenvío - VRF) para que varias prefijos de subred virtual (cada una representada por un VNI) se pueden enrutar directamente a entre sí. Entre inquilino (o entre dominios enrutamiento) no se admite el enrutamiento sin pasar por una puerta de enlace.   
  
La red física en la que se aplica un túnel tráfico encapsulado del cada inquilino está representada por una red lógica llamada a la red lógica de proveedor. Esta red lógica de proveedor consta de una o varias subredes, cada una representada por un prefijo de IP y, opcionalmente, una VLAN 802.1q etiqueta.  
  
Puedes crear redes lógicas adicionales y subredes por motivos de infraestructura transportar el tráfico de administración, el tráfico de almacenamiento, live tráfico de migración, etcetera.  
  
Microsoft SDN no admite el aislamiento de las redes de inquilino usando VLAN. El aislamiento de inquilino se logra únicamente mediante la virtualización de Hyper-V red superposición redes virtuales y encapsulación. 


