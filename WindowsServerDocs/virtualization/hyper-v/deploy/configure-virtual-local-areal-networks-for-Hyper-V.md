---
title: Configurar redes de área local virtuales para Hyper-V
description: Proporciona instrucciones para configurar una red de área local virtual (VLAN) para su uso por máquinas virtuales en un host de Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848466"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configurar redes de área local virtuales para Hyper-V
Redes de área local virtuales \(VLAN\) ofrecen una manera de aislar el tráfico de red. Las VLAN se configuran en los conmutadores y enrutadores que admiten 802.1q. Si configura varias VLAN y desea la comunicación entre ellos, deberá configurar para permitir los dispositivos de red. 

Necesita lo siguiente para configurar las VLAN:  
  
-   Un adaptador de red físico y un controlador que admita 802.1q etiquetado de VLAN.  
-   Un conmutador de red físico que admita 802.1q etiquetado de VLAN.  
  
En el host, a configurar el conmutador virtual para permitir el tráfico de red en el puerto del conmutador físico. Esto es para los identificadores de VLAN que desee usar internamente con máquinas virtuales. A continuación, configure la máquina virtual para especificar la VLAN que se usará la máquina virtual para todas las comunicaciones de red.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Para permitir que un conmutador virtual que se va a usar una VLAN  
  
1.  Abra Hyper\-V Manager.  
  
2.  En el menú Acciones, haga clic en **Administrador de conmutadores virtuales**.  
  
3.  En **conmutadores virtuales**, seleccione un conmutador virtual conectado a un adaptador de red físico que admita VLAN. 

4. En el panel derecho, Id. de VLAN, seleccione **habilite la identificación de LAN virtual** y, a continuación, escriba un número del VLAN ID. de  
  
    Todo el tráfico que pasa a través del adaptador de red físico conectado al conmutador virtual se etiquetarán con el identificador de VLAN que establezca.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Para permitir que una máquina virtual para usar una VLAN  
  
1.  Abra Hyper\-V Manager.  
  
2.  En el panel de resultados, bajo **máquinas virtuales**, seleccione la máquina virtual correspondiente y, a continuación, haga clic en **configuración**.  

3.  En **Hardware**, seleccione un conmutador virtual que se ha configurado con una VLAN.
  
4.  En el panel derecho, seleccione **habilite la identificación de LAN virtual**, y, a continuación, escriba el mismo identificador de VLAN que el especificado para el conmutador virtual. 

Si la máquina virtual debe usar más VLAN, realice una de las siguientes acciones:  
  
-   Conecte los adaptadores de red virtual más adecuada de conmutadores virtuales y asignar los identificadores de VLAN. Asegúrese de configurar las direcciones IP correctamente y que el tráfico que desea enrutar a través de la VLAN también usa la dirección IP correcta.  
  
-   Configurar el adaptador de red virtual word en modo de tronco mediante el [establecer\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt.
  
## <a name="see-also"></a>Vea también  
 
[Hyper\-V Virtual Switch](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)