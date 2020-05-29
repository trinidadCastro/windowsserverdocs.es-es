---
title: Quitar nodo de la pertenencia al clúster de conmutación por error activa
description: En este artículo se aborda el problema de buscar nodos quitados de la pertenencia al clúster de conmutación por error activa.
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 6613bf09c3588637cfe03cb7647e4fed358b760c
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150333"
---
# <a name="nodes-being-removed-from-failover-cluster-membership-on-vmware-esx"></a>Nodos que se quitan de la pertenencia al clúster de conmutación por error en VMWare ESX

En este artículo se aborda el problema de buscar nodos quitados de la pertenencia al clúster de conmutación por error activa cuando los nodos se hospedan en VMWare ESX.

## <a name="symptom"></a>Síntoma

Cuando se produce el problema, verá en el registro de eventos del sistema del Visor de eventos:

![Evento 1135](media/nodes-failover-cluster-vmware/1135.png)

## <a name="resolution"></a>Solución

Un problema específico se debe a que los adaptadores de VMXNET3 quitan los paquetes de red entrantes porque el búfer de entrada es demasiado bajo para administrar grandes cantidades de tráfico. Podemos averiguar fácilmente si se trata de un problema mediante el monitor de rendimiento para ver el contador "red Interface\Packets recibida descartada".

![Agregar contadores](media/nodes-failover-cluster-vmware/0527.png)

Una vez que haya agregado este contador, examine los números de medio, mínimo y máximo y, si son valores mayores que cero, el búfer de recepción debe ajustarse para el adaptador. Este problema está documentado en la base de conocimiento de VMWare: [pérdida de paquetes grande en el nivel de SO invitado en el VMXNET3 VNIC en ESXi 5. x/4. x](https://kb.vmware.com/s/article/2039495).
