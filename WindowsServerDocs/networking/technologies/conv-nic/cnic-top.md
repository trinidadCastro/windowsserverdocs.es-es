---
title: Guía de configuración de la tarjeta de interfaz de red convergente (NIC)
description: La tarjeta de interfaz de red (NIC) convergente permite exponer RDMA a través de una NIC virtual de partición de host (vNIC) para que los servicios de partición de host puedan tener acceso al acceso directo a memoria remota (RDMA) en las mismas NIC que los invitados de Hyper-V usan para el tráfico TCP/IP.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d791e0d51278d1f83f344250d38b1c7005c1a14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355440"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Guía de configuración de la tarjeta de interfaz de red convergente \(NIC @ no__t-1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La tarjeta de interfaz de red convergente \(NIC @ no__t-1 permite exponer RDMA a través de una NIC virtual de host @ no__t-2partition \(vNIC @ no__t-4 para que los servicios de partición de host puedan tener acceso a memoria directa remota \(RDMA @ no__t-6 en las mismas NIC que los invitados de Hyper-V usan para el tráfico TCP/IP.

Antes de la característica NIC convergente, los servicios Management \(host Partition @ no__t-1 que querían usar RDMA debían usar NIC de RDMA @ no__t-2capable dedicadas, incluso si el ancho de banda estuviera disponible en las NIC que estaban enlazadas al conmutador virtual de Hyper-V.

Con la NIC convergente, las dos cargas de trabajo @no__t los usuarios de 0management de RDMA y el tráfico invitado @ no__t-1 pueden compartir las mismas NIC físicas, lo que le permite instalar menos NIC en los servidores.

Cuando se implementa una NIC convergente con hosts de Hyper-V de Windows Server 2016 y conmutadores virtuales de Hyper-V, el VNIC de los hosts de Hyper-V expone los servicios RDMA para hospedar los procesos mediante RDMA a través de cualquier tecnología de RDMA de Ethernet @ no__t-0based.

>[!NOTE]
>Para usar la tecnología NIC convergente, los adaptadores de red certificados en los servidores deben admitir RDMA.

En esta guía se proporcionan dos conjuntos de instrucciones, uno para las implementaciones en las que los servidores tienen un único adaptador de red instalado, que es una implementación básica de la NIC convergente; y otro conjunto de instrucciones en las que los servidores tienen dos o más adaptadores de red instalados, que es una implementación de una NIC convergente a través de un equipo de switch Embedded Teaming \(SET @ no__t-1 de los adaptadores de red RDMA @ no__t-2capable.


## <a name="prerequisites"></a>Requisitos previos

A continuación se indican los requisitos previos para las implementaciones básicas y de centros de datos de la NIC convergente.

>[!NOTE]
>Para obtener los ejemplos proporcionados, usamos un adaptador Ethernet ConnectX-3 Pro 40 Gbps de Mellanox, pero puede usar cualquiera de los adaptadores de red RDMA @ no__t-0capable certificados de Windows Server que admiten esta característica. Para obtener más información acerca de los adaptadores de red compatibles, vea el tema sobre el catálogo de Windows Server en las [tarjetas LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Requisitos previos de NIC convergentes básicos

Para realizar los pasos de esta guía para la configuración básica de NIC convergente, debe tener lo siguiente.

- Dos servidores que ejecutan Windows Server 2016 Datacenter Edition o Windows Server 2016 Standard Edition.
- Un adaptador de red compatible con RDMA instalado en cada servidor.
- Rol de servidor de Hyper-V instalado en cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Requisitos previos de la NIC convergente del centro de recursos

Para realizar los pasos de esta guía para la configuración de NIC convergente de Datacenter, debe tener lo siguiente.

- Dos servidores que ejecutan Windows Server 2016 Datacenter Edition o Windows Server 2016 Standard Edition.
- Dos adaptadores de red compatibles con RDMA instalados en cada servidor.
- Rol de servidor de Hyper-V instalado en cada servidor.
- Debe estar familiarizado con switch Embedded Teaming \(SET @ no__t-1, una solución de formación de equipos NIC alternativa usada en entornos que incluyen Hyper-V y la pila de redes definidas por software (SDN) en Windows Server 2016. El conjunto integra la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V. Para obtener más información, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Temas relacionados
- [Configuración de NIC convergente con un solo adaptador de red](cnic-single.md)
- [Configuración de NIC en equipo NIC convergente](cnic-datacenter.md)
- [Configuración del conmutador físico para la NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas de configuraciones de NIC convergentes](cnic-app-troubleshoot.md)

---