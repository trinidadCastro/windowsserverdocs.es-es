---
title: Implementar máquinas virtuales blindadas
ms.prod: windows-server
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f4550f8a92330c8f483e332ab9e4b36fda853b0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856868"
---
# <a name="deploy-shielded-vms"></a>Implementar máquinas virtuales blindadas


>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En los temas siguientes se describe cómo un inquilino puede trabajar con máquinas virtuales blindadas.

1. Opta [Cree un disco de plantilla de Windows](guarded-fabric-create-a-shielded-vm-template.md) o [cree un disco de plantilla de Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). El disco de plantilla se puede crear tanto en el inquilino como en el proveedor de servicios de hosting. 

2. Opta [Convertir una máquina virtual de Windows existente en una máquina virtual blindada](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Cree datos de blindaje para definir una máquina virtual blindada](guarded-fabric-tenant-creates-shielding-data.md).

    Para obtener una descripción y un diagrama de un archivo de datos de blindaje, consulte [¿Qué son los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Para obtener información sobre cómo crear un archivo de respuesta para incluirlo en un archivo de datos blindado, consulte [máquinas virtuales blindadas: generación de un archivo de respuesta mediante la función New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Cree una máquina virtual blindada:
 
    - Uso de **Windows Azure Pack**: [implementación de una máquina virtual blindada mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Uso de **Virtual Machine Manager**: [implementación de una máquina virtual blindada mediante Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Creación de una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Vea también

- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
