---
description: 'Más información sobre: máquinas virtuales blindadas para inquilinos: implementación de una máquina virtual blindada mediante Windows Azure Pack'
title: 'Máquinas virtuales blindadas para inquilinos: implementación de una máquina virtual blindada mediante Windows Azure Pack'
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 7eb7078c9df81d4623c0f9655edc11ac991e1844
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043833"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Máquinas virtuales blindadas para inquilinos: implementación de una máquina virtual blindada mediante Windows Azure Pack

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Si su proveedor de servicios de hosting es compatible, puede usar Windows Azure Pack para implementar una máquina virtual blindada.

Siga los pasos siguientes:

1. Suscríbase a uno o varios planes ofrecidos en Windows Azure Pack.

2. Cree una máquina virtual blindada mediante Windows Azure Pack.

    [Use máquinas virtuales blindadas](/previous-versions/azure/windows-server-azure-pack/mt720674(v=technet.10)), que se describe en los temas siguientes:

   - [Cree datos de blindaje](/previous-versions/azure/windows-server-azure-pack/mt720672(v=technet.10)) (y cargue el archivo de datos de blindaje, tal como se describe en el segundo procedimiento del tema).

     > [!NOTE]
     > Como parte de la creación de datos de blindaje, descargará el archivo de clave de protección, que será un archivo XML en formato UTF-8. No cambie el archivo a UTF-16.

   - [Cree una máquina virtual blindada](/previous-versions/azure/windows-server-azure-pack/mt720673(v=technet.10)) : con **creación rápida**, a través de una plantilla blindada o a través de una plantilla normal.

       > [!WARNING]
       > Si [crea una máquina virtual blindada mediante una plantilla normal](/previous-versions/azure/windows-server-azure-pack/mt720673(v=technet.10)#Anchor_2), es importante tener en cuenta que la máquina virtual se aprovisiona sin *protección*. Esto significa que el disco de plantilla no se comprueba con la lista de discos de confianza del archivo de datos de blindaje ni con los secretos del archivo de datos de blindaje que se usa para aprovisionar la máquina virtual. Si hay disponible una plantilla blindada, es preferible implementar una máquina virtual blindada con una plantilla blindada para proporcionar una protección de un extremo a otro de los secretos.

   - [Conversión de una máquina virtual de generación 2 en una máquina virtual blindada](/previous-versions/azure/windows-server-azure-pack/mt720670(v=technet.10))

       > [!NOTE]
       > Si convierte una máquina virtual en una máquina virtual blindada, los puntos de control y las copias de seguridad existentes no se cifran. Debe eliminar los puntos de control anteriores cuando sea posible para impedir el acceso a los datos antiguos descifrados.

## <a name="additional-references"></a>Referencias adicionales

- [Pasos de configuración del proveedor de servicios de hosting para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
