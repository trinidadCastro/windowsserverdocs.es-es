---
title: Configuración y visualización de la configuración de VLAN en puertos de conmutador virtual de Hyper-V
description: Puede utilizar este tema para obtener información sobre procedimientos recomendados para configurar y ver la configuración de red de área Local (VLAN) virtual en un puerto de conmutador Virtual de Hyper-V en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820556"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configuración y visualización de la configuración de VLAN en puertos de conmutador virtual de Hyper-V

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre procedimientos recomendados para configurar y ver la configuración de red de área Local (VLAN) virtual en un puerto de conmutador Virtual de Hyper-V.

Cuando desea establecer la configuración de VLAN en puertos de conmutador Virtual de Hyper-V, puede usar cualquier Windows&reg; Server 2016 Hyper-V Manager o System Center Virtual Machine Manager (VMM).

Si usa VMM, VMM usa el siguiente comando de Windows PowerShell para configurar el puerto del conmutador.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Si no usa VMM y está configurando el puerto del conmutador en Windows Server, puede usar la consola de administrador de Hyper-V o el siguiente comando de Windows PowerShell.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

Debido a estos dos métodos para definir la configuración de VLAN en puertos de conmutador Virtual de Hyper-V, es posible que al intentar ver la configuración de puerto del conmutador, parece que no está configurada la configuración de VLAN - incluso cuando están configurados.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Usar el mismo método para configurar y ver la configuración de VLAN del puerto de conmutador

Para asegurarse de no encontrar estos problemas, debe usar el mismo método para ver la configuración de VLAN del puerto de conmutador que usó para configurar la configuración de VLAN del puerto de conmutador.

Para configurar y ver la configuración de puerto del conmutador VLAN, haga lo siguiente:

- Si usa VMM o controladora de red para configurar y administrar la red y ha implementado redes definidas por Software (SDN), debe usar el **VMNetworkAdapterIsolation** cmdlets. 
- Si usas los cmdlets de PowerShell de Windows o Windows Server 2016 Hyper-V Manager y no ha implementado redes definidas por Software (SDN), debe usar el **VMNetworkAdapterVlan** cmdlets.

### <a name="possible-issues"></a>Posibles problemas

Si no se siguen estas directrices pueden surgir los siguientes problemas.

- En circunstancias donde ha implementado SDN y usar VMM, la controladora de red, o la **VMNetworkAdapterIsolation** cmdlets para configurar las VLAN en un puerto de conmutador Virtual de Hyper-V: Si usa el Administrador de Hyper-V o **VMNetworkAdapterVlan obtener** para ver las opciones de configuración, la salida del comando no muestra la configuración de VLAN. En su lugar, debe usar el **Get VMNetworkIsolation** cmdlet para ver la configuración de VLAN.
- En circunstancias donde no se ha implementado SDN y, en su lugar, use el Administrador de Hyper-V o el **VMNetworkAdapterVlan** cmdlets para configurar las VLAN en un puerto de conmutador Virtual de Hyper-V: Si usas el **Get VMNetworkIsolation** cmdlet para ver las opciones de configuración, la salida del comando no muestra la configuración de VLAN. En su lugar, debe usar el **VMNetworkAdapterVlan obtener** cmdlet para ver la configuración de VLAN.

También es importante no intentar configurar la misma configuración de VLAN del puerto de conmutador mediante el uso de estos métodos de configuración. Si lo hace, el puerto del conmutador no está configurado correctamente y el resultado puede ser un error de comunicación de red.

### <a name="resources"></a>Recursos

Para obtener más información sobre los comandos de Windows PowerShell que se mencionan en este tema, vea lo siguiente:

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





