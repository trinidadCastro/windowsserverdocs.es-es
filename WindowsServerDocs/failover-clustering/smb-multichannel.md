---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB multicanal simplificada y redes de clústeres de varias NIC
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7fad43cb5f3de5c10ed815fa802b6168c15850d1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990746"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB multicanal simplificada y redes de clústeres de varias NIC

> Se aplica a: Windows Server 2019, Windows Server 2016

SMB multicanal simplificado y múltiples<abbr title="Tarjeta de interfaz de red">NIC</abbr> Redes de clústeres es una característica que permite el uso de varias NIC en la misma subred de red del clúster y habilita automáticamente SMB multicanal.

Las redes de clústeres de varias NIC y de SMB multicanal simplificado ofrecen las siguientes ventajas:
- Los clústeres de conmutación por error reconocen automáticamente todas las NIC de los nodos que usan el mismo modificador o la misma subred; no se necesita ninguna configuración adicional.
- SMB multicanal se habilita automáticamente.
- Las redes que solo tienen recursos de direcciones IP locales de vínculo IPv6 (fe80) se reconocen en redes de solo clúster (privadas).
- De forma predeterminada, se configura un único recurso de dirección IP en cada nombre de red de punto de acceso de clúster (CAP).
- La validación del clúster ya no emite mensajes de advertencia cuando se encuentran varias NIC en la misma subred.

## <a name="requirements"></a>Requisitos
-   Varias NIC por servidor, con el mismo conmutador o subred.

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Cómo aprovechar las ventajas de las redes de clústeres de varias NIC y SMB multicanal simplificado
En esta sección se describe cómo aprovechar las ventajas de las nuevas redes de clústeres de varias NIC y las características simplificadas de SMB multicanal.

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Usar al menos dos redes para la agrupación en clústeres de conmutación por error
Aunque es poco frecuente, los conmutadores de red pueden producir un error, pero se recomienda usar al menos dos redes para la agrupación en clústeres de conmutación por error. Todas las redes que se encuentran se usan para los latidos de clúster. Evite el uso de una única red para el clúster de conmutación por error con el fin de evitar un único punto de error. Idealmente, debería haber varias rutas de comunicación físicas entre los nodos del clúster y ningún punto único de error.

![Ilustración de dos redes para el clúster de conmutación por error ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)
 **Ilustración 1: usar al menos dos redes para el clúster de conmutación por error**

### <a name="use-multiple-nics-across-clusters"></a>Uso de varias NIC entre clústeres

Se consigue el máximo beneficio de la simplificación de SMB multicanal cuando se usan varias NIC en los clústeres, tanto en los clústeres de carga de almacenamiento como en los de almacenamiento. Esto permite que los clústeres de carga de trabajo (Hyper-V, SQL Server instancia de clúster de conmutación por error, réplica de almacenamiento, etc.) usen SMB multicanal y resulten un uso más eficaz de la red. En una configuración de clúster convergente (también conocida como desagregada) en la que se usa un clúster de servidores de archivos de escalabilidad horizontal para almacenar los datos de la carga de trabajo de un clúster de instancias de clúster de conmutación por error de Hyper-V o SQL Server, esta red a menudo se denomina "subred norte-sur"/red. Muchos clientes maximizan el rendimiento de esta red al invertir en tarjetas y conmutadores NIC compatibles con RDMA.

![Ilustración de una subred de SMB de sur de norte ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)
 **: para lograr el máximo rendimiento de la red, use varias NIC en el clúster de servidores de archivos de escalabilidad horizontal y en el clúster de la instancia de clúster de conmutación por error de Hyper-V o SQL Server, que comparten la subred North meridional**

![Captura de dos clústeres que usan varias NIC en la misma subred para aprovechar SMB multicanal ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)
 **figura 3: dos clústeres (servidor de archivos de escalabilidad horizontal para almacenamiento, SQL Server <abbr title=" FCI de instancia de clúster de conmutación por error "> para la </abbr> carga de trabajo) usan varias NIC en la misma subred para aprovechar SMB multicanal y lograr un mejor rendimiento de la red.**

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconocimiento automático de redes privadas locales de vínculo IPv6
Cuando se detectan redes privadas (solo en clúster) con varias NIC, el clúster reconocerá automáticamente las direcciones IP de los vínculos IPv6 locales (fe80) de cada NIC de cada subred. Esto ahorra tiempo a los administradores, ya que ya no es necesario configurar manualmente los recursos de dirección IP de vínculo IPv6 local (fe80).

Cuando se usa más de una red privada (solo en clúster), Compruebe la configuración de enrutamiento IPv6 para asegurarse de que el enrutamiento no está configurado para cruzar subredes, ya que esto reducirá el rendimiento de la red.

![Captura de la configuración de red automática en la interfaz de usuario Administrador de clústeres de conmutación por error ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)
 **figura 4: configuración de recursos de dirección locales de vínculo IPv6 (fe80)**

## <a name="throughput-and-fault-tolerance"></a>Rendimiento y tolerancia a errores
Windows Server 2019 y Windows Server 2016 detectan automáticamente las funciones de NIC e intentarán usar cada NIC en la configuración más rápida posible. Se pueden usar todas las NIC que se han agrupado, las NIC con RSS y las NIC con capacidad RDMA. En la tabla siguiente se resumen las ventajas e inconvenientes al usar estas tecnologías. El rendimiento máximo se consigue al usar varias NIC compatibles con RDMA. Para obtener más información, consulte [los conceptos básicos de SMB Mutlichannel](/archive/blogs/josebda/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0).

![Una ilustración del rendimiento y la tolerancia a errores para varias configuraciones de NIC ](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)
 **figura 5: rendimiento y tolerancia a errores para varias CONIFIGURATIONS de NIC**

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
**¿Son todas las NIC de una red de varias NIC que se usan para el latido del corazón del clúster?**
Sí.

**¿Se puede usar una red de varias NIC solo para la comunicación de clústeres? ¿O solo se puede usar para la comunicación entre el cliente y el clúster?**
Cualquier configuración funcionará: todos los roles de red de clúster funcionarán en una red con varias NIC.

**¿Está SMB multicanal también se usa para el tráfico de clústeres y CSV?**
Sí, de forma predeterminada todo el tráfico de clústeres y CSV usará redes de varias NIC disponibles. Los administradores pueden usar los cmdlets de PowerShell de clústeres de conmutación por error o Administrador de clústeres de conmutación por error interfaz de usuario para cambiar el rol de red.

**¿Cómo se puede ver la configuración de multicanal de SMB?**
Use el cmdlet **Get-SMBServerConfiguration** para buscar el valor de la propiedad EnableMultiChannel.

**¿La propiedad común de clúster PlumbAllCrossSubnetRoutes respeta en una red de varias NIC?**
Sí.

## <a name="additional-references"></a>Referencias adicionales
- [Novedades de los clústeres de conmutación por error de Windows Server](whats-new-in-failover-clustering.md)