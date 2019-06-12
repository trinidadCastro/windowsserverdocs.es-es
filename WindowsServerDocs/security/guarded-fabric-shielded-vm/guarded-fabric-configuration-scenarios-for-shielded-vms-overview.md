---
title: Implementar máquinas virtuales blindadas
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9badbfcb709c29451425aaecc56b46ac98837e18
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443801"
---
# <a name="deploy-shielded-vms"></a>Implementar máquinas virtuales blindadas


>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Los temas siguientes describen cómo un inquilino puede trabajar con máquinas virtuales blindadas.

1. (Opcional) [Crear un disco de plantilla de Windows](guarded-fabric-create-a-shielded-vm-template.md) o [crear un disco de plantilla Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). El disco de plantilla se puede crear por el inquilino o el proveedor de servicios de hospedaje. 

2. (Opcional) [Convertir una máquina virtual Windows existente en una máquina virtual blindada](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Crear datos de blindaje para definir una máquina virtual blindada](guarded-fabric-tenant-creates-shielding-data.md).

    ¿Para una descripción y un diagrama de un archivo de datos de blindaje, consulte [lo es con los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Para obtener información acerca de cómo crear un archivo de respuesta debe incluir en un archivo de datos blindados, consulte [máquinas virtuales blindadas: generar un archivo de respuesta mediante la función New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Cree una máquina virtual blindada:
 
    - Uso de **Windows Azure Pack**: [Implementar una máquina virtual blindada mediante Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Uso de **de Virtual Machine Manager**: [Implementar una máquina virtual blindada mediante el uso de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Crear una plantilla de máquina virtual blindada](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Vea también

- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
