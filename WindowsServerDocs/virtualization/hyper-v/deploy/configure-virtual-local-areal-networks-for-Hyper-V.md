---
title: Configuración de redes de área local virtual para Hyper-V
description: Proporciona instrucciones para configurar una red de área local virtual (VLAN) para que la usen las máquinas virtuales en un host de Hyper-V.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: f2c240a3ad9f9783e509efb288cc6c6410339685
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364282"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configuración de redes de área local virtual para Hyper-V
Las redes de área local virtuales \(VLAN\) ofrecen una manera de aislar el tráfico de red. Las VLAN se configuran en conmutadores y enrutadores que admiten 802.1 q. Si configura varias VLAN y desea que se produzca la comunicación entre ellas, deberá configurar los dispositivos de red para que lo permitan. 

Necesitará lo siguiente para configurar las VLAN:  
  
-   Un adaptador de red físico y un controlador que admita el etiquetado de VLAN 802.1 q.  
-   Un conmutador de red físico que admite el etiquetado de VLAN de 802.1 q.  
  
En el host, configurará el conmutador virtual para permitir el tráfico de red en el puerto del conmutador físico. Esto es para los identificadores de VLAN que desea usar internamente con máquinas virtuales. A continuación, configure la máquina virtual para especificar la VLAN que usará la máquina virtual para todas las comunicaciones de red.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Para permitir que un conmutador virtual use una VLAN  
  
1.  Abra el administrador de Hyper\-V.  
  
2.  En el menú acciones, haga clic en **Administrador de conmutadores virtuales**.  
  
3.  En **conmutadores virtuales**, seleccione un conmutador virtual conectado a un adaptador de red físico que admita VLAN. 

4. En el panel derecho, en ID. de VLAN, seleccione **Habilitar identificación de LAN virtual** y, a continuación, escriba un número para el identificador de VLAN.  
  
    Todo el tráfico que atraviesa el adaptador de red físico conectado al conmutador virtual se etiquetará con el identificador de VLAN que establezca.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Para permitir que una máquina virtual use una VLAN  
  
1.  Abra el administrador de Hyper\-V.  
  
2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual adecuada y, a continuación, haga clic con el botón derecho en **configuración**.  

3.  En **hardware**, seleccione un conmutador virtual que esté configurado con una VLAN.
  
4.  En el panel derecho, seleccione **Habilitar identificación de LAN virtual**y, a continuación, escriba el mismo identificador de VLAN que el que especificó para el conmutador virtual. 

Si la máquina virtual necesita usar más VLAN, realice una de las siguientes acciones:  
  
-   Conecte más adaptadores de red virtuales a los conmutadores virtuales adecuados y asigne los identificadores de VLAN. Asegúrese de configurar las direcciones IP correctamente y de que el tráfico que desea enrutar a través de la VLAN use también la dirección IP correcta.  
  
-   Configure el adaptador de Word para red virtual en el modo de tronco mediante el [conjunto\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt.
  
## <a name="see-also"></a>Consulta también  
 
[Conmutador virtual de Hyper\-V](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)