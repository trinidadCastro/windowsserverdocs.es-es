---
title: Comprender el uso de redes virtuales y VLAN
description: En este tema, obtendrá información sobre las redes virtuales de virtualización de red de Hyper-V y cómo se diferencian de las redes de área local virtual (VLAN). Con virtualización de red de Hyper-V, cree redes virtuales de superposición, también denominadas redes virtuales.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 56e90966d38b8a138dd8781863a4eaad1db639fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854488"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Comprender el uso de redes virtuales y VLAN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre las redes virtuales de virtualización de red de Hyper-V y cómo se diferencian de las redes de área local virtual (VLAN). Con virtualización de red de Hyper-V, cree redes virtuales de superposición, también denominadas redes virtuales.



  
Las redes definidas por software (SDN) en Windows Server 2016 se basan en la Directiva de programación para redes virtuales de superposición en un conmutador virtual de Hyper-V. Puede crear redes virtuales de superposición, también denominadas redes virtuales, con virtualización de red de Hyper-V. 
  
Al implementar la virtualización de red de Hyper-V, las redes de superposición se crean encapsulando el fotograma de Ethernet de capa 2 de la máquina virtual de inquilino original con un encabezado de túnel o de superposición (por ejemplo, VXLAN o NVGRE) y una dirección IP de nivel 3 y Ethernet de capa 2. encabezados de la red proporcionaban (o física). Las redes virtuales de superposición se identifican mediante un identificador de Virtual Network de 24 bits (VNI) para mantener el aislamiento del tráfico de inquilinos y permitir direcciones IP superpuestas. El VNI se compone de un identificador de subred virtual (un identificador de red), un identificador de conmutador lógico y un identificador de túnel.  
  
Además, a cada inquilino se le asigna un dominio de enrutamiento (similar a enrutamiento virtual y reenvío-VRF) para que se puedan enrutar directamente entre sí varios prefijos de subred virtual (cada uno representado por un VNI). No se admite el enrutamiento entre inquilinos (o el dominio de enrutamiento cruzado) sin pasar por una puerta de enlace.   
  
La red física en la que se tuneliza el tráfico encapsulado de cada inquilino se representa mediante una red lógica denominada red lógica del proveedor. Esta red lógica de proveedor se compone de una o varias subredes, cada una de ellas representada por un prefijo IP y, opcionalmente, una etiqueta 802.1 q de VLAN.  
  
Puede crear redes lógicas y subredes adicionales a efectos de infraestructura para llevar el tráfico de administración, el tráfico de almacenamiento, el tráfico de migración en vivo, etc.  
  
Microsoft SDN no admite el aislamiento de redes de inquilinos mediante el uso de VLAN. El aislamiento de inquilinos se logra únicamente mediante el uso de la encapsulación y las redes virtuales de superposición de virtualización de red de Hyper-V. 


