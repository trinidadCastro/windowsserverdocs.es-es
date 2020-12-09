---
title: Configuración de redes de área local virtual para Hyper-V
description: Proporciona instrucciones para configurar una red de área local virtual (VLAN) para que la usen las máquinas virtuales en un host de Hyper-V.
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/11/2016
ms.openlocfilehash: 22f9890fd1a3a90fcbab59a9ed1de8481e117e75
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866164"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configuración de redes de área local virtual para Hyper-V
Redes de área local virtuales las \( VLAN \) ofrecen una manera de aislar el tráfico de red. Las VLAN se configuran en conmutadores y enrutadores que admiten 802.1 q. Si configura varias VLAN y desea que se produzca la comunicación entre ellas, deberá configurar los dispositivos de red para que lo permitan.

Necesitará lo siguiente para configurar las VLAN:

- Un adaptador de red físico y un controlador que admita el etiquetado de VLAN 802.1 q.
- Un conmutador de red físico que admite el etiquetado de VLAN de 802.1 q.

En el host, configurará el conmutador virtual para permitir el tráfico de red en el puerto del conmutador físico. Esto es para los identificadores de VLAN que desea usar internamente con máquinas virtuales. A continuación, configure la máquina virtual para especificar la VLAN que usará la máquina virtual para todas las comunicaciones de red.

#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Para permitir que un conmutador virtual use una VLAN

1. Abra el \- Administrador de Hyper V.

2. En el menú Acciones , haz clic en **Administrador de conmutadores virtuales**.

3. En **conmutadores virtuales**, seleccione un conmutador virtual conectado a un adaptador de red físico que admita VLAN.

4. En el panel derecho, en ID. de VLAN, seleccione **Habilitar identificación de LAN virtual** y, a continuación, escriba un número para el identificador de VLAN.

    Todo el tráfico que atraviesa el adaptador de red físico conectado al conmutador virtual se etiquetará con el identificador de VLAN que establezca.

#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Para permitir que una máquina virtual use una VLAN

1. Abra el \- Administrador de Hyper V.

2. En el panel de resultados, en **virtual machines**, seleccione la máquina virtual adecuada y, a continuación, haga clic con el botón derecho en **configuración**.

3. En **hardware**, seleccione un conmutador virtual que esté configurado con una VLAN.

4. En el panel derecho, seleccione **Habilitar identificación de LAN virtual** y, a continuación, escriba el mismo identificador de VLAN que el que especificó para el conmutador virtual.

Si la máquina virtual necesita usar más VLAN, realice una de las siguientes acciones:

- Conecte más adaptadores de red virtuales a los conmutadores virtuales adecuados y asigne los identificadores de VLAN. Asegúrese de configurar las direcciones IP correctamente y de que el tráfico que desea enrutar a través de la VLAN use también la dirección IP correcta.

- Configure el adaptador de red virtual en el modo de tronco mediante el cmdlet [set \- VMNetworkAdapterVlan](/powershell/module/hyper-v/set-vmnetworkadaptervlan) .

## <a name="see-also"></a>Vea también

[\-Conmutador virtual de Hyper V](../../hyper-v-virtual-switch/hyper-v-virtual-switch.md)
