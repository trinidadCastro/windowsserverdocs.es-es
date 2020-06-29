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
ms.openlocfilehash: 78d8d76c2d8fc92e7fc5ca070d04942facd97427
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475362"
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

## <a name="additional-references"></a>Referencias adicionales

- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
