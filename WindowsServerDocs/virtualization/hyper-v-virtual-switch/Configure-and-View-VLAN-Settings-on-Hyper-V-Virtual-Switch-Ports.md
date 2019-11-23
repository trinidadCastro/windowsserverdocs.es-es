---
title: Configuración y visualización de la configuración de VLAN en puertos de conmutador virtual de Hyper-V
description: Puede usar este tema para obtener información sobre las prácticas recomendadas para configurar y ver la configuración de la red de área local virtual (VLAN) en un puerto de conmutador virtual de Hyper-V en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 28abdfe8295ad3f9fac29b8cc80aeebe2992392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366870"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configuración y visualización de la configuración de VLAN en puertos de conmutador virtual de Hyper-V

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las prácticas recomendadas para configurar y ver la configuración de la red de área local virtual (VLAN) en un puerto de conmutador virtual de Hyper-V.

Si desea configurar las opciones de VLAN en los puertos del conmutador virtual de Hyper-V, puede usar el administrador de Hyper-V de Windows&reg; Server 2016 o System Center Virtual Machine Manager (VMM).

Si usa VMM, VMM usa el siguiente comando de Windows PowerShell para configurar el puerto del conmutador.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Si no usa VMM y está configurando el puerto del conmutador en Windows Server, puede usar la consola del administrador de Hyper-V o el siguiente comando de Windows PowerShell.
```
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

- En los casos en los que haya implementado SDN y que use VMM, el controlador de red o los cmdlets de **VMNetworkAdapterIsolation** para configurar las opciones de VLAN en un puerto de conmutador virtual de Hyper-v: Si usa el administrador de Hyper-v o **obtiene VMNetworkAdapterVlan** para ver la configuración, la salida del comando no mostrará la configuración de VLAN. En su lugar, debe usar el cmdlet **Get-VMNetworkIsolation** para ver la configuración de la VLAN.
- En las circunstancias en las que no ha implementado SDN y, en su lugar, use el administrador de Hyper-V o los cmdlets de **VMNetworkAdapterVlan** para configurar las opciones de VLAN en un puerto de conmutador virtual de Hyper-v: Si usa el cmdlet **Get-VMNetworkIsolation** para ver la configuración, la salida del comando no mostrará la configuración de VLAN. En su lugar, debe usar el cmdlet **Get VMNetworkAdapterVlan** para ver la configuración de la VLAN.

También es importante no intentar configurar la misma configuración de VLAN del puerto del conmutador mediante los dos métodos de configuración. Si lo hace, el puerto del conmutador se configura incorrectamente y el resultado puede ser un error en la comunicación de red.

### <a name="resources"></a>Recursos

Para obtener más información sobre los comandos de Windows PowerShell que se mencionan en este tema, vea lo siguiente:

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





