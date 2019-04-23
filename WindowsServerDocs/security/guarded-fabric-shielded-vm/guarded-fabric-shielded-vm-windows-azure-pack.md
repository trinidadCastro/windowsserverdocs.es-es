---
title: Máquinas virtuales blindadas para inquilinos - implementación de una máquina virtual blindada con Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e0372cb5b1f891bb724f246a3f8a7931619ce7ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847196"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Máquinas virtuales blindadas para inquilinos - implementación de una máquina virtual blindada con Windows Azure Pack

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Si su proveedor de servicios de hospedaje lo admite, puede usar Windows Azure Pack para implementar una máquina virtual blindada.

Complete los pasos siguientes:

<!-- When we have a link to the topic about how tenants subscribe, add that link as an indented item just under step 1 below. -->

1. Suscribirse a uno o varios planes que se ofrecen en Windows Azure Pack.

2. Crear una máquina virtual blindada con Windows Azure Pack.

    [Uso de máquinas virtuales blindadas](https://technet.microsoft.com/library/mt720674.aspx), que se describe en los temas siguientes:

    - [Crear datos de blindaje](https://technet.microsoft.com/library/mt720672.aspx) (y cargue el archivo de datos de blindaje, como se describe en el segundo procedimiento en el tema).
    
    > [!NOTE]
    > Como parte de la creación de los datos de blindaje, descargará el archivo de clave de protección, que será un archivo XML en formato UTF-8. No cambie el archivo a UTF-16.
    
    - [Crear una máquina virtual blindada](https://technet.microsoft.com/library/mt720673.aspx) : con **creación rápida**, mediante una plantilla blindada o a través de una plantilla Normal.
    
        > [!WARNING]
        > Si se [crear una máquina virtual blindada mediante una plantilla Normal](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), es importante tener en cuenta que la máquina virtual se aprovisione *no blindada*. Esto significa que el disco de plantilla no se comprueba con la lista de discos de confianza en el archivo de datos de blindaje, ni se usan los secretos en el archivo de datos de blindaje para aprovisionar la máquina virtual. Si está disponible una plantilla blindada, es preferible implementar una máquina virtual blindada con una plantilla blindada para proporcionar protección de extremo a otro de los secretos.
    
    - [Convertir una máquina virtual de generación 2 en una máquina virtual blindada](https://technet.microsoft.com/library/mt720670.aspx)
    
        > [!NOTE]
        > Si convierte una máquina virtual a una máquina virtual blindada, no se cifran las copias de seguridad y los puntos de control existentes. Debe eliminar los puntos de control anteriores cuando sea posible para evitar el acceso a los datos antiguos, descifrados.

## <a name="see-also"></a>Vea también

- [Hospedaje de los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
