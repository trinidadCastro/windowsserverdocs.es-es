---
title: Planear funciones de red de Hyper-V en Windows Server
description: Describe lo que es necesario para la red básica en Hyper-V y proporciona vínculos a instrucciones
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829876"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Planear funciones de red de Hyper-V en Windows Server

>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server, Windows Server 2019 de 2019
  
Un conocimiento básico de las redes de Hyper-V le ayuda a planificar las redes para máquinas virtuales. Este artículo también tratan algunas consideraciones de red cuando se usa la migración en vivo y cuando se usa Hyper-V con otros roles y características de servidor.  
  
## <a name="hyper-v-networking-basics"></a>Conceptos básicos sobre redes de Hyper-V  
Red básica en Hyper-V es bastante sencilla. Usa dos partes: un conmutador virtual y un adaptador de red virtual. Necesitará al menos uno de cada uno para establecer la conexión de red para una máquina virtual. El conmutador virtual se conecta a ninguna red basada en Ethernet. El adaptador de red virtual se conecta a un puerto en el conmutador virtual, lo que permite que una máquina virtual use una red.  
  
Es la manera más fácil de establecer la red básica crear un conmutador virtual al instalar Hyper-V. A continuación, cuando se crea una máquina virtual, puede conectarse al conmutador. Conectarse automáticamente al conmutador, agrega un adaptador de red virtual a la máquina virtual. Para obtener instrucciones, consulte [crear un conmutador virtual para las máquinas virtuales de Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Para administrar distintos tipos de redes, puede agregar adaptadores de red virtual y conmutadores virtuales. Todos los conmutadores son parte del host de Hyper-V, pero solo una máquina virtual que pertenece cada adaptador de red virtual.  
  
El conmutador virtual es un conmutador de red basada en software Ethernet de capa 2. Proporciona características integradas para supervisar, controlar y segmentar el tráfico, así como la seguridad y diagnóstico.  Puede agregar al conjunto de características integradas mediante la instalación de los complementos, también denominados *extensiones*. Están disponibles de fabricantes independientes de software. Para obtener más información acerca del conmutador y extensiones, consulte [conmutador Virtual de Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Opciones de adaptador de red y conmutador  
Hyper-V ofrece tres tipos de conmutadores virtuales y los dos tipos de adaptadores de red virtual. Elegirá uno de cada uno de los que desee al crearla. Puede usar el Administrador de Hyper-V o el módulo de Hyper-V para Windows PowerShell para crear y administrar los adaptadores de red virtual y conmutadores virtuales. Algunas funcionalidades de red avanzadas, como listas de puerto extendido el control de acceso (ACL), solo se pueden administrar mediante cmdlets en el módulo de Hyper-V.  
  
Puede realizar algunos cambios a un conmutador virtual o adaptador de red virtual después de crearlo. Por ejemplo, es posible cambiar un conmutador existente a un tipo diferente, pero esto afecta a las funcionalidades de red de todas las máquinas virtuales conectadas a ese conmutador.  Por lo tanto, que probablemente no hacerlo a menos que los cometió un error o necesite probar algo. Como otro ejemplo, puede conectar un adaptador de red virtual a un conmutador diferente, lo que podría hacer si desea conectarse a una red diferente. Sin embargo, no se puede cambiar un adaptador de red virtual de un tipo a otro. En lugar de cambiar el tipo, podría agregar otro adaptador de red virtual y elija el tipo adecuado.  
  
Tipos de conmutador virtual son:  
  
-   **Conmutador virtual externo** -se conecta a una red con cable física mediante un enlace a un adaptador de red físico.  
  
-   **Conmutador virtual interno** -se conecta a una red que puede usarse únicamente por las máquinas virtuales que se ejecutan en el host que tiene el conmutador virtual y entre el host y las máquinas virtuales.  
  
-   **Conmutador virtual privado** -se conecta a una red que puede usarse únicamente por las máquinas virtuales que se ejecutan en el host que tiene el conmutador virtual, pero no proporciona funciones de red entre el host y las máquinas virtuales.  
  
Tipos de adaptador de red virtual son:  
  
-   **Adaptador de red de Hyper-V específico** : disponible para la generación 1 y máquinas virtuales de generación 2. Está diseñado específicamente para Hyper-V y requiere un controlador que se incluye en los servicios de integración de Hyper-V. Este tipo de adaptador de red más rápida y es la opción recomendada, a menos que se necesita para que arranque en la red o se ejecuta un sistema operativo de invitado no admitido. El controlador necesario se proporciona sólo para sistemas operativos invitados admitidos. Tenga en cuenta en el Administrador de Hyper-V y los cmdlets de red, este tipo solo se conoce como un adaptador de red.  
  
-   **Adaptador de red heredado** , disponible solo en máquinas virtuales de generación 1. Emula un basados en Intel 21140 adaptador Fast Ethernet PCI y se puede usar para arrancar en una red por lo que puede instalar un sistema operativo desde un servicio como servicios de implementación de Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Las redes Hyper-V y las tecnologías relacionadas  
Las versiones recientes de Windows Server presenta mejoras que ofrecen más opciones para la configuración de redes de Hyper-V. Por ejemplo, Windows Server 2012 introdujo la compatibilidad para converger la red. Esto le permite enrutar el tráfico de red a través de un conmutador virtual externo. Windows Server 2016 se basa en esto, ya que permite acceso de memoria directa remota (RDMA) en los adaptadores de red enlazados a un conmutador virtual de Hyper-V. Puede usar esta configuración con o sin Switch Embedded Teaming (SET). Para obtener más información, consulte [acceso a memoria directa remota &#40;RDMA&#41; y Switch Embedded Teaming &#40;establecido&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Algunas características se basan en configuraciones de red específicas o hacerlo mejor en determinadas configuraciones. Tenga en cuenta estos al planear o actualizar la infraestructura de red.  
  
**Agrupación en clústeres de conmutación por error** -es una práctica recomendada para aislar el tráfico del clúster y utilizar calidad de servicio (QoS) de Hyper-V en el conmutador virtual. Para obtener más información, consulte [recomendaciones de red para un clúster de Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migración en vivo** -usar las opciones de rendimiento para reducir la red y el uso de CPU y el tiempo necesario para completar una migración en vivo. Para obtener instrucciones, consulte [configurar hosts para migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Espacios de almacenamiento directo** : esta característica se basa en el protocolo de red SMB3.0 y RDMA. Para obtener más información, consulte [espacios de almacenamiento directo en Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).