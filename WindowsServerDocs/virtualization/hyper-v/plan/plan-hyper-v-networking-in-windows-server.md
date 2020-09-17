---
title: Planear la red de Hyper-V en Windows Server
description: Describe lo que se necesita para la red básica en Hyper-V y proporciona vínculos a instrucciones.
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: e36ebb6d90dcb4fc05e05c135a49f84e850249b2
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745940"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Planear la red de Hyper-V en Windows Server

>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

Una comprensión básica de las redes en Hyper-V le ayuda a planear las redes para máquinas virtuales. En este artículo también se tratan algunas consideraciones sobre redes al usar la migración en vivo y al usar Hyper-V con otras características y roles de servidor.

## <a name="hyper-v-networking-basics"></a>Aspectos básicos de las redes de Hyper-V
La red básica en Hyper-V es bastante sencilla. Utiliza dos partes: un conmutador virtual y un adaptador de red virtual. Necesitará al menos uno de cada para establecer la red para una máquina virtual. El conmutador virtual se conecta a cualquier red basada en Ethernet. El adaptador de red virtual se conecta a un puerto del conmutador virtual, lo que permite que una máquina virtual use una red.

La forma más fácil de establecer una red básica es crear un conmutador virtual al instalar Hyper-V. A continuación, cuando cree una máquina virtual, puede conectarla al conmutador. Al conectarse al conmutador, se agrega automáticamente un adaptador de red virtual a la máquina virtual. Para obtener instrucciones, consulte [crear un conmutador virtual para máquinas virtuales de Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).

Para controlar los distintos tipos de redes, puede Agregar conmutadores virtuales y adaptadores de red virtuales. Todos los conmutadores forman parte del host de Hyper-V, pero cada adaptador de red virtual pertenece a una sola máquina virtual.

El conmutador virtual es un conmutador de red Ethernet de nivel 2 basado en software. Proporciona características integradas para supervisar, controlar y segmentar el tráfico, así como la seguridad y los diagnósticos.  Puede Agregar al conjunto de características integradas instalando complementos, también denominados  *extensiones*. Están disponibles en proveedores de software independientes. Para obtener más información sobre el conmutador y las extensiones, consulte [conmutador virtual de Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="switch-and-network-adapter-choices"></a>Opciones de conmutadores y adaptadores de red
Hyper-V ofrece tres tipos de conmutadores virtuales y dos tipos de adaptadores de red virtual. Elegirá cuál de cada uno de ellos desea cuando lo cree. Puede usar el administrador de Hyper-V o el módulo de Hyper-V para Windows PowerShell para crear y administrar conmutadores virtuales y adaptadores de red virtuales. Algunas capacidades de red avanzadas, como las listas de control de acceso (ACL) de Puerto extendido, solo se pueden administrar mediante cmdlets en el módulo de Hyper-V.

Puede realizar algunos cambios en un conmutador virtual o un adaptador de red virtual después de crearlo. Por ejemplo, es posible cambiar un conmutador existente a un tipo diferente, pero esto afecta a las capacidades de red de todas las máquinas virtuales conectadas a ese conmutador.  Por lo tanto, es probable que no lo haga a menos que haya cometido un error o necesite probar algo. Como otro ejemplo, puede conectar un adaptador de red virtual a un conmutador diferente, lo que podría hacer si desea conectarse a una red diferente. Sin embargo, no puede cambiar un adaptador de red virtual de un tipo a otro. En lugar de cambiar el tipo, debe agregar otro adaptador de red virtual y elegir el tipo adecuado.

Los tipos de conmutador virtual son:

-   **Conmutador virtual externo** : se conecta a una red física cableada mediante un enlace a un adaptador de red físico.

-   **Conmutador virtual interno** : se conecta a una red que solo pueden usar las máquinas virtuales que se ejecutan en el host que tiene el conmutador virtual y entre el host y las máquinas virtuales.

-   **Conmutador virtual privado** : se conecta a una red que solo pueden usar las máquinas virtuales que se ejecutan en el host que tiene el conmutador virtual, pero no proporciona redes entre el host y las máquinas virtuales.

Los tipos de adaptador de red virtual son:

-   **Adaptador de red específico de Hyper-V** : disponible para las máquinas virtuales de generación 1 y generación 2. Está diseñado específicamente para Hyper-V y requiere un controlador incluido en los servicios de integración de Hyper-V. Este tipo de adaptador de red es más rápido y es la opción recomendada a menos que necesite arrancar en la red o ejecutar un sistema operativo invitado no compatible. El controlador necesario solo se proporciona para los sistemas operativos invitados admitidos. Tenga en cuenta que en el administrador de Hyper-V y los cmdlets de red, este tipo se conoce simplemente como adaptador de red.

-   **Adaptador de red heredado** : solo disponible en máquinas virtuales de generación 1. Emula un adaptador de PCI Fast Ethernet basado en Intel 21140 y se puede usar para arrancar en una red, de forma que pueda instalar un sistema operativo desde un servicio como servicios de implementación de Windows.

## <a name="hyper-v-networking-and-related-technologies"></a>Redes de Hyper-V y tecnologías relacionadas
Las últimas versiones de Windows Server introdujeron mejoras que proporcionan más opciones para configurar las funciones de red de Hyper-V. Por ejemplo, Windows Server 2012 incorporó la compatibilidad con las redes convergentes. Esto le permite enrutar el tráfico de red a través de un conmutador virtual externo. Windows Server 2016 se basa en esto permitiendo el acceso directo a memoria remota (RDMA) en los adaptadores de red enlazados a un conmutador virtual de Hyper-V. Puede usar esta configuración con o sin switch Embedded Teaming (SET). Para obtener más información, vea [acceso directo a memoria remota &#40;RDMA&#41; y switch Embedded teaming &#40;SET&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)

Algunas características se basan en configuraciones de red específicas o en determinadas configuraciones. Tenga esto en cuenta al planear o actualizar la infraestructura de red.

**Clústeres de conmutación por error** : se recomienda aislar el tráfico del clúster y usar la calidad de servicio (QoS) de Hyper-V en el conmutador virtual. Para obtener más información, consulte [recomendaciones de red para un clúster de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11))

**Migración en vivo** : Use las opciones de rendimiento para reducir el uso de la red y la CPU y el tiempo necesario para completar una migración en vivo. Para obtener instrucciones, consulte [configuración de hosts para la migración en vivo sin clústeres de conmutación por error](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).

**Espacios de almacenamiento directo** : esta característica se basa en el protocolo de red SMB 3.0 y RDMA. Para obtener más información, consulte [espacios de almacenamiento directo en Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).