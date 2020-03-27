---
title: Implementación de Azure Virtual Machines mediante el centro de administración de Windows
description: 'Implementación de máquinas virtuales de Azure con el centro de administración de Windows. Configuración de máquinas virtuales de Azure como parte del centro de administración de Windows: escenarios administrados.'
ms.technology: manage
ms.topic: article
author: nedpyle
ms.author: nedpyle
manager: jgerend
ms.date: 01/28/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2249a69f60fe87758c74a58aa13b47124da41361
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319369"
---
# <a name="deploy-azure-virtual-machines-from-within-windows-admin-center"></a>Implementar máquinas virtuales de Azure desde el centro de administración de Windows

>Se aplica a: Centro de administración de Windows, versión preliminar del centro de administración de Windows

La versión 1910 del centro de administración de Windows permite implementar máquinas virtuales de Azure. Esto integra la implementación de la máquina virtual en cargas de trabajo administradas del centro de administración de Windows como el [servicio de migración de almacenamiento](../../../storage/storage-migration-service/overview.md) y la réplica de [almacenamiento](../../../storage/storage-replica/storage-replica-overview.md). En lugar de crear nuevos servidores y máquinas virtuales en Azure portal manualmente antes de implementar la carga de trabajo, y posiblemente faltan los pasos necesarios y la configuración: el centro de administración de Windows puede implementar la máquina virtual de Azure, configurar su almacenamiento, unirse al dominio, instalar roles y a continuación, configure el sistema distribuido. También puede implementar nuevas máquinas virtuales de Azure sin una carga de trabajo desde la página conexiones del centro de administración de Windows.

El centro de administración de Windows también administra una gran variedad de servicios de Azure. [Obtenga más información sobre las opciones de integración de Azure disponibles con el centro de administración de Windows](../plan/azure-integration-options.md).

## <a name="scenarios"></a>Escenarios

La implementación de la máquina virtual de Azure de la versión 1910 del centro de administración de Windows admite los siguientes escenarios:

- [Servicio de migración de almacenamiento](../../../storage/storage-migration-service/overview.md)
- [Réplica de almacenamiento](../../../storage/storage-replica/storage-replica-overview.md)
- [Nuevo servidor independiente (sin roles)](index.md#extend-on-premises-capacity-with-azure)

## <a name="requirements"></a>Requisitos

La creación de una nueva máquina virtual de Azure desde el centro de administración de Windows requiere lo siguiente:

- Una [suscripción de Azure](https://azure.microsoft.com).
- Una [puerta de enlace del centro de administración de Windows registrada con Azure](azure-integration.md)
- Un [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) existente donde tenga permisos de creación.
- Un [Virtual Network de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) y una subred existentes.
- Una solución de Azure [expressroute](https://azure.microsoft.com/services/expressroute/) o [VPN de Azure](https://azure.microsoft.com/services/vpn-gateway/) vinculada a la red virtual y la subred que permite la conectividad desde máquinas virtuales de Azure a los clientes locales, los controladores de dominio, el equipo del centro de administración de Windows y los servidores que requieren comunicación con esta máquina virtual como parte de la implementación de una carga de trabajo. Por ejemplo, para usar el servicio de migración de almacenamiento para migrar el almacenamiento a una máquina virtual de Azure, el equipo de Orchestrator y el equipo de origen deben poder ponerse en contacto con la máquina virtual de Azure de destino a la que va a migrar.

## <a name="usage"></a>Uso

Los pasos y asistentes de implementación de máquinas virtuales de Azure varían según el escenario. Revise la documentación de la carga de trabajo para obtener información detallada sobre el escenario global.

### <a name="deploying-azure-vms-as-part-of-storage-migration-service"></a>Implementación de máquinas virtuales de Azure como parte del servicio de migración de almacenamiento

1. Desde la herramienta *Storage Migration Service* del centro de administración de Windows, realice un inventario de uno o varios servidores de origen.
2. Una vez que esté en la fase de *transferencia de datos* , seleccione **crear una nueva máquina virtual de Azure** en la página *especificar un destino* y, luego, haga clic en **crear máquina virtual**.<br><br>
Esto inicia una herramienta de creación paso a paso que selecciona una máquina virtual de Azure de Windows Server 2012 R2, Windows Server 2016 o Windows Server 2019 como destino de la migración. El servicio de migración de almacenamiento proporciona tamaños de máquina virtual recomendados para que coincidan con el origen, pero puede invalidarlos haciendo clic en **Ver todos los tamaños**.
<br><br>Los datos del servidor de origen también se usan para configurar automáticamente los discos administrados y sus sistemas de archivos, así como para unir la nueva máquina virtual de Azure a su dominio de Active Directory. Si la máquina virtual es Windows Server 2019 (lo que se recomienda), el centro de administración de Windows instala la característica de proxy del servicio de migración de almacenamiento. Una vez que haya creado la máquina virtual de Azure, el centro de administración de Windows vuelve al flujo de trabajo de transferencia de servicio de migración de almacenamiento normal.  

Este es un vídeo que muestra cómo usar el servicio de migración de almacenamiento para migrar a máquinas virtuales de Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ] 

### <a name="deploying-azure-vms-as-part-of-storage-replica"></a>Implementación de máquinas virtuales de Azure como parte de la réplica de almacenamiento

1. En la herramienta de *réplica de almacenamiento* del centro de administración de Windows, en la pestaña *asociaciones* , seleccione **nuevo** y, a continuación, en *replicar con otro servidor* , seleccione **usar una nueva máquina virtual de Azure** y, después, haga clic en **siguiente**.
2. Especifique la información del servidor de origen y el nombre del grupo de replicación y, a continuación, seleccione **siguiente**.<br><br>
Esto inicia un proceso que selecciona automáticamente una máquina virtual de Azure de Windows Server 2016 o Windows Server 2019 como destino del origen de la migración. El servicio de migración de almacenamiento recomienda tamaños de máquina virtual para que coincidan con el origen, pero puede invalidar esto seleccionando **Ver todos los tamaños**. Los datos de inventario se usan para configurar automáticamente los discos administrados y sus sistemas de archivos, así como para unir la nueva máquina virtual de Azure a su dominio de Active Directory. 
3. Después de que el centro de administración de Windows cree la máquina virtual de Azure, proporcione un nombre de grupo de replicación y, después, seleccione **crear**. Después, el centro de administración de Windows inicia el proceso de sincronización inicial de la réplica de almacenamiento normal para empezar a proteger los datos.

Este es un vídeo que muestra cómo usar la réplica de almacenamiento para replicar en máquinas virtuales de Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/_VqD7HjTewQ] 

### <a name="deploying-a-new-standalone-azure-vm"></a>Implementación de una nueva máquina virtual de Azure independiente

1. En la página *todas las conexiones* del centro de administración de Windows, seleccione **Agregar**.
2. En la sección *máquina virtual de Azure* , seleccione **crear nuevo**.<br><br> Esto inicia una herramienta de creación paso a paso que le permitirá seleccionar una máquina virtual de Azure con Windows Server 2012 R2, Windows Server 2016 o Windows Server 2019, elegir un tamaño, agregar discos administrados y, opcionalmente, unirse a su dominio de Active Directory.

Este es un vídeo que muestra cómo usar el centro de administración de Windows para crear máquinas virtuales de Azure.

> [!VIDEO https://www.youtube-nocookie.com/embed/__A8J9aC_Jk] 
