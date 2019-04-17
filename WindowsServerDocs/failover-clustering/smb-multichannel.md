---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: "Simplificada multicanal SMB y NIC de varias redes de clúster"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>Simplificada multicanal SMB y NIC de varias redes de clúster

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Simplificado SMB multicanal y Multi -<abbr title="tarjeta de interfaz de red">NIC</abbr> redes del clúster es una nueva característica en Windows Server 2016 que permite el uso de varias NIC en la misma subred de clúster y multicanal SMB se habilita automáticamente.  

Simplificada SMB multicanal y NIC de varias redes de clúster ofrece las siguientes ventajas:  
- Clústeres de conmutación por error reconoce automáticamente todas las NIC en los nodos que se usa el mismo modificador / misma subred - ninguna configuración adicional que sea necesitada.  
- Multicanal SMB se habilita automáticamente.  
- Redes que solo tienen recursos de direcciones IP locales de IPv6 vínculo (fe80) se reconocen en solo clúster redes (privadas).  
- Un único recurso de dirección IP se configura en nombre de red para cada punto de acceso de clúster (PAC) (NN) de manera predeterminada.  
- Validación de clúster ya no envía mensajes de advertencia cuando varias NIC se encuentran en la misma subred.  

## <a name="requirements"></a>Requisitos  
-   Varias NIC por servidor, con el mismo modificador / subred.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Cómo sacar provecho de varias NIC clústeres de redes y multicanal SMB simplificada  
Esta sección describe cómo sacar provecho de las nuevas redes de clústeres de múltiples NIC y características de multicanal SMB simplificadas de Windows Server 2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Usar al menos dos redes de clústeres de conmutación por error   
Aunque es poco frecuente, conmutadores de red pueden producir un error, es recomendable seguir para usar al menos dos redes de clústeres de conmutación por error. Todas las redes que se encuentran se usan para latidos de clúster. Evita usar una sola red para el clúster de conmutación por error con el fin de evitar un único punto de error. En teoría, debería haber varias rutas de acceso física de comunicación entre los nodos del clúster y ningún punto único de error.  

![Ilustración de dos redes para clústeres de conmutación por error](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Usar al menos dos redes de clústeres de conmutación por error**  

### <a name="use-multiple-nics-across-clusters"></a>Usar varias NIC entre clústeres  

Máximo beneficio de la simplificada multicanal SMB se logra cuando se usan varias NIC de clústeres: en el almacenamiento y clústeres de carga de trabajo de almacenamiento. Esto permite que los clústeres de carga de trabajo (Hyper-V, instancia de clúster de conmutación por error de SQL Server, réplica de almacenamiento, etc.) usar multicanal SMB y los resultados en un uso más eficaz de la red. En convergente (también conocida como desagregadas) la configuración en un clúster de servidor de archivos de escalado se usa para almacenar datos de carga de trabajo para una Hyper-V o clúster de instancia de clúster de conmutación por error de SQL Server, esta red se suele denominar "la subred norte sur" de clúster / de red. Muchos clientes maximizar el rendimiento de esta red invirtiendo en modificadores y tarjetas NIC capaces de RDMA.  

![Ilustración de una subred SMB del Norte-Sudáfrica](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Para lograr rendimiento de la red máximo, usa varias NIC en el clúster de servidor de archivos de escalado y el clúster Hyper-V o instancia de clúster de conmutación por error de SQL Server - que comparten la subred del Norte-Sudáfrica**  

![ScreenCap de dos clústeres con varias NIC en la misma subred para sacar provecho multicanal SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Dos clústeres (servidor de archivos de escalado para el almacenamiento, SQL Server <abbr title="instancia de clústeres de conmutación por error">FCI</abbr> de carga de trabajo) ambos usan varias NIC en la misma subred aprovechar SMB multicanal y lograr un mejor rendimiento de red.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconocimiento automático de redes privadas IPv6 locales de vínculo  
Cuando se detectan redes privadas (solo en clúster) con varias NIC, el clúster reconocerá automáticamente direcciones IP locales de IPv6 vínculo (fe80) para cada NIC en cada subred. Este administradores ahorra tiempo ya que ya no tendrán que configurar manualmente los recursos de la dirección IP Local de IPv6 vínculo (fe80).  

Al usar más de una red privada (solo en clúster), comprueba la configuración de enrutamiento IPv6 para asegurarse de que el enrutamiento no está configurado para que se cruzan subredes, dado que este modo reduce el rendimiento de la red.  

![ScreenCap automático de configuración de red en la UI del Administrador de clúster de conmutación por error](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configuración de recursos de automática de direcciones IPv6 vínculo Local (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Rendimiento y la tolerancia a errores  
Windows Server 2016 detecta las funcionalidades NIC e intentará usar cada NIC la configuración más rápido posible automáticamente. NIC que se asoció NIC con RSS y NIC con funcionalidad RDMA pueden utilizarse. La siguiente tabla resume las ventajas y desventajas, al usar estas tecnologías. Máximo rendimiento se logra al usar varias NIC capaces de RDMA. Para obtener más información, consulta [los conceptos básicos de SMB multicanal](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Ilustración de rendimiento y tolerancia a errores en las diferentes configuraciones de NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: Rendimiento y tolerancia a errores para diversos conifigurations NIC**   

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes  
**¿Todas las NIC en una red multi-NIC para se utilizan muy resistente corazón de clúster?**  
    Sí.  

**¿Una red multi-NIC sirve para la comunicación de clúster solo? ¿O bien, puede solo puede usarse para comunicación de cliente y el clúster?**  
    Ya sea configuración funcionará: todas las funciones de red de clúster funcionará en una red multi-NIC.  

**¿SMB multicanal también se usa para el tráfico CSV y clúster?**  
    Sí, de manera predeterminada todo clúster y tráfico CSV usarán redes multi-NIC disponible. Los administradores pueden usar los cmdlets de PowerShell de clústeres de conmutación por error o una interfaz de usuario del Administrador de clústeres de conmutación por error para cambiar el rol de la red.  

**¿Cómo puedo ver la configuración de SMB multicanal?**  
    Usa el **Get SMBServerConfiguration** cmdlet, busca el valor de la propiedad EnableMultiChannel.  

**¿Es la propiedad común de clúster que plumballcrosssubnetroutes respetan en una red multi-NIC?**  
     Sí.  

## <a name="see-also"></a>Consulta también  
- [Novedades de conmutación por error en Windows Server](whats-new-in-failover-clustering.md)  
