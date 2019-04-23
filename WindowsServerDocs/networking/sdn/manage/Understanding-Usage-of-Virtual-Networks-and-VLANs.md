---
title: Entender el uso de las redes virtuales y VLAN
description: En este tema, obtendrá información sobre redes virtuales de virtualización de red de Hyper-V y cómo se diferencian de redes de área local virtuales (VLAN). Con la virtualización de red de Hyper-V, cree redes virtuales de superposición, también llamadas redes virtuales.
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875536"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Entender el uso de las redes virtuales y VLAN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre redes virtuales de virtualización de red de Hyper-V y cómo se diferencian de redes de área local virtuales (VLAN). Con la virtualización de red de Hyper-V, cree redes virtuales de superposición, también llamadas redes virtuales.



  
Definidas por software Networking (SDN) en Windows Server 2016 se basa en la directiva de redes virtuales de superposición en un conmutador Virtual de Hyper-V de programación. Puede crear redes virtuales de superposición, también llamadas redes virtuales, con virtualización de red de Hyper-V. 
  
Al implementar la virtualización de red de Hyper-V, se crean redes superpuestas encapsulando el marco de Ethernet de capa 2 de los inquilinos la máquina virtual original con una superposición - o túnel - encabezado (por ejemplo, VXLAN o NVGRE) y 3 de la capa IP y Ethernet de capa 2 red de los encabezados del subposición (o físicos). Las redes virtuales de superposición se identifican por un 24 bits Virtual Network identificador (VNI) para mantener el aislamiento del tráfico de inquilinos y para permitir direcciones IP superpuestas. El VNI se compone de una subred virtual ID (VSID), el identificador de conmutador lógico y el identificador de túnel.  
  
Además, cada inquilino se asigna a un dominio de enrutamiento (similar a enrutamiento virtual y reenvío - VRF) para que varios prefijos de subred virtual (cada uno representado por un VNI) se pueden enrutar directamente entre sí. Varios inquilinos (o entre dominios de enrutamiento) no se admite el enrutamiento sin pasar por una puerta de enlace.   
  
La red física en el que se abren el tráfico encapsulado cada inquilino se representa mediante una red lógica denominada la red lógica del proveedor. Esta red lógica del proveedor se compone de una o varias subredes, cada uno representado por un prefijo IP y, opcionalmente, una VLAN 802.1q etiqueta.  
  
Puede crear redes lógicas adicionales y las subredes por motivos de infraestructura transportar el tráfico de administración, el tráfico de almacenamiento en vivo tráfico de migración, etcetera.  
  
Microsoft SDN no admite el aislamiento de redes de inquilinos mediante el uso de VLAN. Aislamiento de inquilinos se consigue únicamente mediante el uso de las redes virtuales de superposición de virtualización de red de Hyper-V y la encapsulación. 


