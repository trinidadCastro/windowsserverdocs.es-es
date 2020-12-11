---
description: 'Más información sobre: máquinas virtuales blindadas: preparación de una aplicación auxiliar de blindaje de VM VHD'
title: 'Máquinas virtuales blindadas: preparación de una aplicación auxiliar de blindaje de máquinas virtuales VHD'
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 79c28bb51abb0bd05a4302f392d8be288b9899d5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049113"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>Máquinas virtuales blindadas: preparación de una aplicación auxiliar de blindaje de máquinas virtuales VHD

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

> [!IMPORTANT]
> Antes de comenzar estos procedimientos, asegúrese de que ha instalado la actualización acumulativa más reciente para Windows Server 2016 o que usa la versión más reciente de Windows 10 [herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=45520). De lo contrario, los procedimientos no funcionarán.

En esta sección se describen los pasos que realiza un proveedor de servicios de hosting para habilitar la compatibilidad con la conversión de máquinas virtuales existentes en blindadas.

Para entender cómo se ajusta este tema en el proceso general de implementación de máquinas virtuales blindadas, consulte [los pasos de configuración de proveedor de servicio de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>¿Qué máquinas virtuales se pueden blindar?

El proceso de blindaje para las máquinas virtuales existentes solo está disponible para las máquinas virtuales que cumplen los requisitos previos siguientes:

- El SO invitado es Windows Server 2012, 2012 R2, 2016 o una versión de canal semianual. Las máquinas virtuales de Linux existentes no se pueden convertir en máquinas virtuales blindadas.
- La máquina virtual es una máquina virtual de generación 2 (firmware UEFI)
- La máquina virtual no usa discos de diferenciación para su volumen de sistema operativo.

## <a name="prepare-helper-vhd"></a>Preparar la aplicación auxiliar VHD

1.  En un equipo con Hyper-V y las herramientas de **máquinas virtuales blindadas** de características de herramientas de administración remota del servidor instaladas, cree una nueva máquina virtual de generación 2 con un VHDX en blanco e instale windows Server 2016 en ella con los medios de instalación ISO de Windows Server. Esta máquina virtual no debe estar blindada y debe ejecutar Server Core o Server con experiencia de escritorio.

    > [!IMPORTANT]
    > La aplicación auxiliar de blindaje de máquina virtual VHD **no debe** estar relacionada con los discos de plantilla que creó en el [proveedor de servicios de hosting. crea una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md). Si vuelve a usar un disco de plantilla, habrá una colisión de la firma de disco durante el proceso de blindaje porque ambos discos tendrán el mismo identificador de disco GPT. Para evitarlo, puede crear un VHD nuevo (en blanco) e instalar Windows Server 2016 en él mediante los medios de instalación ISO.

2.  Inicie la máquina virtual, complete los pasos de configuración e inicie sesión en el escritorio. Una vez que haya comprobado que la máquina virtual está en un estado de funcionamiento, apague la máquina virtual.

3.  En una ventana de Windows PowerShell con privilegios elevados, ejecute el siguiente comando para preparar el VHDX creado anteriormente para convertirse en un disco auxiliar de blindaje de máquinas virtuales. Actualice la ruta de acceso con la ruta de acceso correcta para su entorno.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Una vez que el comando se haya completado correctamente, copie el VHDX en el recurso compartido de biblioteca de VMM. **No** vuelva a iniciar la máquina virtual desde el paso 1. Si lo hace, se dañará el disco auxiliar.

5.  Ahora puede eliminar la máquina virtual del paso 1 en Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configuración del servidor de protección de host de VMM

En la consola VMM, abra el panel Configuración y, luego, **hospede la configuración del servicio de protección de host** en **General**. En la parte inferior de esta ventana, hay un campo para configurar la ubicación de la aplicación auxiliar VHD. Use el botón Examinar para seleccionar el disco duro virtual en el recurso compartido de biblioteca. Si no ve el disco en el recurso compartido, es posible que tenga que actualizar manualmente la biblioteca en VMM para que se muestre.

![VMM: configuración del servicio de protección de host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="additional-references"></a>Referencias adicionales

- [Pasos de configuración del proveedor de servicios de hosting para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
