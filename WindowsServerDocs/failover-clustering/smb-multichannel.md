---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB multicanal simplificada y redes de clústeres de varias NIC
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 1b9271ceac99ac9b21cbfac902ba133d66815df4
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476116"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB multicanal simplificada y redes de clústeres de varias NIC

> Se aplica a: Windows Server 2019, Windows Server 2016

SMB multicanal y Multi - simplificada<abbr title="tarjeta de interfaz de red">NIC</abbr> redes del clúster es una característica que permite el uso de varias NIC en la misma subred de red del clúster y habilita automáticamente SMB multicanal.

SMB multicanal simplificada y redes de clústeres de varias NIC proporciona las siguientes ventajas:  
- Agrupación en clústeres de conmutación por error reconoce automáticamente todas las NIC en los nodos que se usan el mismo modificador / misma subred: no necesitan configuraciones adicionales.  
- SMB multicanal está habilitado automáticamente.  
- Las redes que solo tienen recursos de direcciones IP locales de vínculo IPv6 (fe80) se reconocen en clúster solo redes (privadas).  
- Un único recurso de dirección IP se configura en cada nombre de red de punto de acceso de clúster (CAP) (NN) de forma predeterminada.  
- Validación de clúster ya no emite mensajes de advertencia cuando se encuentran varias NIC en la misma subred.  

## <a name="requirements"></a>Requisitos  
-   Varias NIC por servidor, con el mismo conmutador / subred.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>SMB multicanal simplificada y redes de clústeres de cómo aprovechar las ventajas de tener varias NIC  
En esta sección se describe cómo aprovechar las nuevas redes de clústeres de varias NIC y simplificadas características SMB multicanal.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Utilice al menos dos redes para agrupar en clústeres de conmutación por error   
Aunque es poco frecuente, pueden producir un error en los conmutadores de red: es aún mejor práctica usar al menos dos redes para agrupación en clústeres de conmutación por error. Todas las redes que se encuentran se usan para los latidos del clúster. Evite el uso de una sola red para el clúster de conmutación por error con el fin de evitar un único punto de error. Idealmente, debería haber varias rutas de acceso física de la comunicación entre los nodos del clúster y ningún punto único de error.  

![Ilustración de dos redes de clústeres de conmutación por error](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Utilice al menos dos redes para agrupar en clústeres de conmutación por error**  

### <a name="use-multiple-nics-across-clusters"></a>Usar varias NIC a través de clústeres  

Máximo beneficio de la SMB multicanal simplificada se logra cuando se usan varias NIC a través de clústeres - en el almacenamiento y clústeres de carga de trabajo de almacenamiento. Esto permite a los clústeres de carga de trabajo (Hyper-V, instancia de clúster de conmutación por error de SQL Server, réplica de almacenamiento, etc.) para usar SMB multicanal y los resultados en un uso más eficaz de la red. En convergente (también denominada desagregada) configuración donde se utiliza un clúster de servidor de archivos de escalabilidad horizontal para almacenar los datos de carga de trabajo de Hyper-V o clúster de instancia de clúster de conmutación por error de SQL Server, esta red se suele denominar "la subred norte-sur" del clúster o de red . Muchos clientes maximizar el rendimiento de esta red al invertir en conmutadores y las tarjetas NIC compatibles con RDMA.  

![Ilustración de una subred norte-sur SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Para lograr un rendimiento de red máximo, use varias NIC en el clúster de servidor de archivos de escalabilidad horizontal y el clúster de Hyper-V o instancia de clúster de conmutación por error de SQL Server, que comparten la subred norte sur**  

![Captura de pantalla de dos clústeres con varias NIC en la misma subred aprovechar SMB multicanal](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Dos clústeres (servidor de archivos de escalabilidad horizontal para el almacenamiento, SQL Server <abbr title="instancia de agrupación en clústeres de conmutación por error">FCI</abbr> para cargas de trabajo) usan varias NIC en la misma subred aprovechar SMB multicanal y lograr un mejor red rendimiento.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconocimiento automático de las redes privadas locales de vínculo IPv6  
Cuando se detectan: redes privadas (sólo en clúster) con varias NIC, el clúster reconocerá automáticamente las direcciones IP locales de vínculo IPv6 (fe80) para cada NIC en cada subred. Este administradores ahorra tiempo porque ya no tienen que configurar manualmente los recursos de dirección IP Local de vínculo IPv6 (fe80).  

Cuando se usa más de una red privada (sólo en clúster), compruebe la configuración de enrutamiento de IPv6 para asegurarse de que el enrutamiento no está configurado para cruzar subredes, ya que esto reducirá el rendimiento de la red.  

![Captura de pantalla de configuración automática de redes en la UI del Administrador de clústeres de conmutación por error](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configuración automática de recurso de dirección Local de vínculo IPv6 (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Rendimiento y tolerancia a errores  
2019 de Windows Server y Windows Server 2016 automáticamente detectan las capacidades NIC y se intentarán usar cada NIC en la configuración más rápida posible. Las NIC que están agrupados, por NIC mediante RSS y NIC con la funcionalidad RDMA pueden utilizarse. En la tabla siguiente se resume las ventajas y desventajas al usar estas tecnologías. Rendimiento máximo se logra al usar varias NIC compatibles con RDMA. Para obtener más información, consulte [los aspectos básicos de SMB multicanal](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Ver una ilustración de rendimiento y tolerancia a errores en las diferentes configuraciones de NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: Rendimiento y tolerancia a errores para conifigurations de varias NIC**   

## <a name="frequently-asked-questions"></a>Preguntas frecuentes  
**¿Todas las NIC en una red con varias NIC para se utilizan los latidos de clúster?**  
    Sí.  

**¿Una red con varias NIC se puede usar para la comunicación de clúster solo? ¿O bien, solo se puede usar para la comunicación de cliente y el clúster?**  
    Ya sea configuración funcionará: todas las funciones de red de clúster funcionará en una red con varias NIC.  

**¿SMB multicanal también se usa para el tráfico CSV y clúster?**  
    Sí, de forma predeterminada todos los clúster y tráfico de CSV usará las redes disponibles con varias NIC. Los administradores pueden usar los cmdlets de PowerShell de agrupación en clústeres de conmutación por error o la interfaz de usuario de administrador de clústeres de conmutación por error para cambiar el rol de la red.  

**¿Cómo se puede ver la configuración de SMB multicanal?**  
    Use la **Get SMBServerConfiguration** cmdlet, busque el valor de la propiedad EnableMultiChannel.  

**¿Está PlumbAllCrossSubnetRoutes respeta la propiedad común del clúster en una red con varias NIC?**  
     Sí.  

## <a name="see-also"></a>Vea también  
- [Novedades de la conmutación por error en Windows Server](whats-new-in-failover-clustering.md)  
