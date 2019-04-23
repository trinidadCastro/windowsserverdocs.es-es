---
title: 'Máquinas virtuales blindadas: preparar una VHD de aplicación auxiliar de blindaje de VM'
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e14cdeed435f23f28ca514e232fbcfa6220fc74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887726"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>Máquinas virtuales blindadas: preparar una VHD de aplicación auxiliar de blindaje de VM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

<!-- This comment creates a break between the Applies To above and the Important note below. -->

> [!IMPORTANT]
> Antes de comenzar estos procedimientos, asegúrese de que ha instalado la actualización acumulativa más reciente para Windows Server 2016 o está usando la versión más reciente de Windows 10 [herramientas de administración remota del servidor](https://www.microsoft.com/en-us/download/details.aspx?id=45520). En caso contrario, los procedimientos no funcionará. 

En esta sección se describe los pasos realizados por un proveedor de servicios de hospedaje para habilitar la compatibilidad para la conversión de máquinas virtuales existentes en máquinas virtuales blindadas.

Para entender cómo encaja este tema en el proceso general de la implementación de máquinas virtuales blindadas, consulte [hospeda los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>¿Pueden ser blindadas que las máquinas virtuales?

El proceso de blindaje para máquinas virtuales existentes solo está disponible para las máquinas virtuales que cumplen los requisitos previos siguientes:

- El sistema operativo invitado es Windows Server 2012, 2012 R2, 2016 o una semianual versión del canal. No se puede convertir máquinas virtuales de Linux existente para las máquinas virtuales blindadas.
- La máquina virtual es una generación 2 VM (firmware UEFI)
- La máquina virtual no utiliza discos de diferenciación para su volumen del sistema operativo.

## <a name="prepare-helper-vhd"></a>Preparar la aplicación auxiliar de VHD

1.  En un equipo con Hyper-V y la característica herramientas de administración remota del servidor **herramientas VM blindadas** instalado, crear una nueva generación 2 VM con un VHDX en blanco e instale Windows Server 2016 en ella con la instalación de la ISO de Windows Server Medio. Esta máquina virtual no protegida y debe ejecutar Server Core o servidor con experiencia de escritorio.

    > [!IMPORTANT]
    > La aplicación auxiliar de blindaje VHD de máquina virtual **no debe** deberse a que los discos de plantilla que creó en [proveedor de servicios de Hosting crea una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md). Si vuelve a usar un disco de plantilla, habrá un conflicto de firma de disco durante el proceso de blindaje porque ambos discos tendrán el mismo identificador de disco GPT. Para evitarlo, cree un nuevo disco duro virtual (en blanco) e instale Windows Server 2016 en él con los medios de instalación de ISO.

2.  Inicie la máquina virtual, complete los pasos de instalación e inicie sesión en el escritorio. Cuando haya comprobado la máquina virtual está en un estado de funcionamiento, apague la máquina virtual.

3.  En una ventana de Windows PowerShell con privilegios elevados, ejecute el comando siguiente para preparar el VHDX que creó anteriormente para convertirse en un disco de aplicación auxiliar de blindaje de máquina virtual. Actualice la ruta de acceso con la ruta de acceso correcta para su entorno.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Una vez que el comando se ha completado correctamente, copie el VHDX en el recurso compartido de biblioteca VMM. **No** vuelvan a iniciar la máquina virtual del paso 1. Al hacerlo se dañará el disco de la aplicación auxiliar.

5.  Ahora puede eliminar la máquina virtual del paso 1 de Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurar la configuración de servidor de protección de Host VMM

En la consola VMM, abra el panel de configuración y, a continuación, **configuración del servicio de protección de Host** en **General**. En la parte inferior de esta ventana, hay un campo para configurar la ubicación de la aplicación auxiliar de VHD. Use el botón Examinar para seleccionar el disco duro virtual desde el recurso compartido de biblioteca. Si no ve el disco en el recurso compartido, deberá actualizar manualmente la biblioteca de VMM para aparezcan.

![VMM - configuración de servicio de protección de Host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Vea también

- [Hospedaje de los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
