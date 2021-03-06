---
title: Guía de configuración de la tarjeta de interfaz de red convergente (NIC)
description: La tarjeta de interfaz de red (NIC) convergente permite exponer RDMA a través de una NIC virtual de partición de host (vNIC) para que los servicios de partición de host puedan tener acceso al acceso directo a memoria remota (RDMA) en las mismas NIC que los invitados de Hyper-V usan para el tráfico TCP/IP.
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 2b077b911d3721907e70b198c62970aafe25e58d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944759"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>\(Guía de configuración de NIC de tarjeta de interfaz de red convergente \)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

\(La NIC de tarjeta de interfaz de red convergente \) permite exponer RDMA a través de un \- VNIC de NIC virtual de partición de host \( \) para que los servicios de partición de host puedan tener acceso a la RDMA de acceso directo a memoria remota \( \) en las mismas NIC que los invitados de Hyper-V usan para el tráfico TCP/IP.

Antes de la característica NIC convergente, los \( servicios de partición del host \) de administración que querían usar RDMA debían usar NIC compatibles con RDMA dedicadas \- , incluso si el ancho de banda estaba disponible en las NIC que estaban enlazadas al conmutador virtual de Hyper-V.

Con la NIC convergente, los dos \( usuarios de administración de cargas de trabajo de RDMA y el tráfico invitado \) pueden compartir las mismas NIC físicas, lo que le permite instalar menos NIC en los servidores.

Al implementar la NIC convergente con hosts de Hyper-v de Windows Server 2016 y conmutadores virtuales de Hyper-V, el VNIC de los hosts de Hyper-V expone los servicios RDMA para hospedar los procesos mediante RDMA sobre cualquier \- tecnología RDMA basada en Ethernet.

>[!NOTE]
>Para usar la tecnología NIC convergente, los adaptadores de red certificados en los servidores deben admitir RDMA.

En esta guía se proporcionan dos conjuntos de instrucciones, uno para las implementaciones en las que los servidores tienen un único adaptador de red instalado, que es una implementación básica de la NIC convergente; y otro conjunto de instrucciones en las que los servidores tienen dos o más adaptadores de red instalados, que es una implementación de una NIC convergente a través de un \( equipo de conjunto de equipos incrustados \) de conmutación de \- adaptadores de red compatibles con RDMA.


## <a name="prerequisites"></a>Requisitos previos

A continuación se indican los requisitos previos para las implementaciones básicas y de centros de datos de la NIC convergente.

>[!NOTE]
>Para los ejemplos proporcionados, usamos un adaptador Ethernet Mellanox ConnectX-3 Pro 40 Gbps, pero puede usar cualquiera de los adaptadores de red compatibles con RDMA con certificación de Windows Server \- que admiten esta característica. Para obtener más información acerca de los adaptadores de red compatibles, vea el tema sobre el catálogo de Windows Server en las [tarjetas LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

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
- Debe estar familiarizado con switch Embedded Teaming \( set \) , una solución de formación de equipos NIC alternativa usada en entornos que incluyen Hyper-V y la pila de redes definidas por software (SDN) en Windows Server 2016. El conjunto integra la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V. Para obtener más información, vea [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Temas relacionados
- [Configuración de NIC convergente con un solo adaptador de red](cnic-single.md)
- [Configuración de NIC en equipo NIC convergente](cnic-datacenter.md)
- [Configuración del conmutador físico para la NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas de configuraciones de NIC convergentes](cnic-app-troubleshoot.md)

---