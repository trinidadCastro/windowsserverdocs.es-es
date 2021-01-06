---
title: Configuración y visualización de las opciones de VLAN en puertos de conmutador virtual de Hyper-V
description: Puede usar este tema para obtener información sobre las prácticas recomendadas para configurar y ver la configuración de la red de área local virtual (VLAN) en un puerto de conmutador virtual de Hyper-V en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: lizross
author: eross-msft
ms.date: 12/08/2020
ms.openlocfilehash: 5dea78fba26f34fbfd8d9e9de40563abafbf4fce
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949601"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configuración y visualización de las opciones de VLAN en puertos de conmutador virtual de Hyper-V

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las prácticas recomendadas para configurar y ver la configuración de la red de área local virtual (VLAN) en un puerto de conmutador virtual de Hyper-V.

Si desea configurar las opciones de VLAN en los puertos del conmutador virtual de Hyper-V, puede usar el &reg; Administrador de Hyper-v de Windows Server 2016 o System Center Virtual Machine Manager (VMM).

Si usa VMM, VMM usa el siguiente comando de Windows PowerShell para configurar el puerto del conmutador.

```powershell
Set-VMNetworkAdapterIsolation <VM-name|-ManagementOS -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Si no usa VMM y está configurando el puerto del conmutador en Windows Server, puede usar la consola del administrador de Hyper-V o el siguiente comando de Windows PowerShell.
```powershell
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

Debido a estos dos métodos para configurar las opciones de VLAN en los puertos del conmutador virtual de Hyper-V, es posible que, cuando intente ver la configuración del puerto del conmutador, parezca que la configuración de VLAN no está configurada, incluso cuando están configuradas.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Usar el mismo método para configurar y ver la configuración de VLAN del puerto del conmutador

Para asegurarse de que no encuentra estos problemas, debe usar el mismo método para ver la configuración de VLAN del puerto del conmutador que usó para configurar la configuración de VLAN del puerto del conmutador.

Para configurar y ver la configuración del puerto de conmutador de VLAN, debe hacer lo siguiente:

- Si usa VMM o una controladora de red para configurar y administrar la red, y ha implementado redes definidas por software (SDN), debe usar los cmdlets de **VMNetworkAdapterIsolation** .
- Si usa el administrador de Hyper-V de Windows Server 2016 o cmdlets de Windows PowerShell y no ha implementado redes definidas por software (SDN), debe usar los cmdlets de **VMNetworkAdapterVlan** .

### <a name="possible-issues"></a>Posibles problemas

Si no sigue estas instrucciones, puede encontrar los siguientes problemas.

- En los casos en los que haya implementado SDN y que use VMM, el controlador de red o los cmdlets de **VMNetworkAdapterIsolation** para configurar las opciones de VLAN en un puerto de conmutador virtual de Hyper-v: Si usa el administrador de Hyper-v o **obtiene VMNetworkAdapterVlan** para ver los valores de configuración, el resultado del comando no muestra la configuración de VLAN. En su lugar, debe usar el cmdlet **Get-VMNetworkIsolation** para ver la configuración de la VLAN.
- En las circunstancias en las que no ha implementado SDN y, en su lugar, use el administrador de Hyper-V o los cmdlets de **VMNetworkAdapterVlan** para configurar las opciones de VLAN en un puerto de conmutador virtual de Hyper-v: Si usa el cmdlet **Get-VMNetworkIsolation** para ver los valores de configuración, el resultado del comando no muestra la configuración de VLAN. En su lugar, debe usar el cmdlet **Get VMNetworkAdapterVlan** para ver la configuración de la VLAN.

También es importante no intentar configurar la misma configuración de VLAN del puerto del conmutador mediante los dos métodos de configuración. Si lo hace, el puerto del conmutador se configura incorrectamente y el resultado puede ser un error en la comunicación de red.

### <a name="resources"></a>Recursos

Para obtener más información sobre los comandos de Windows PowerShell que se mencionan en este tema, vea lo siguiente:

- [Set-VmNetworkAdapterIsolation](/powershell/module/hyper-v/set-vmnetworkadapterisolation)
- [Get-VmNetworkAdapterIsolation](/powershell/module/hyper-v/get-vmnetworkadapterisolation)
- [Set-VMNetworkAdapterVlan](/powershell/module/hyper-v/set-vmnetworkadaptervlan)
- [Get-VMNetworkAdapterVlan](/powershell/module/hyper-v/get-vmnetworkadaptervlan)
