---
title: Guía de configuración de tarjeta de interfaz de red (NIC) convergente
description: Tarjeta de interfaz de red convergente (NIC) le permite exponer RDMA a través de una NIC virtual (vNIC) de la partición host para que los servicios de host de partición pueden tener acceso a remoto acceso de memoria directa (RDMA) en la mismas NIC que usan los invitados de Hyper-V para el tráfico de TCP/IP.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838846"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Convergente Network Interface Card \(NIC\) instrucciones de configuración

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Tarjeta de interfaz de red convergente \(NIC\) permite exponer RDMA a través de un host\-NIC virtual de la partición \(vNIC\) para que los servicios de la partición host puedan obtener acceso a memoria directa remota \(RDMA\) en la mismas NIC que usan los invitados de Hyper-V para el tráfico de TCP/IP.

Antes de la característica de NIC convergente, administración \(hospedar partición\) servicios que deseaban usar RDMA tenían que usar RDMA dedicada\-NIC compatibles con, incluso si el ancho de banda, no estaba disponible en la NIC que se enlazaron a Hyper-V Conmutador virtual.

Con estos convergen de NIC, las dos cargas de trabajo \(usuarios de administración de tráfico de invitado y RDMA\) puede compartir la mismas NIC físicas, lo que permite instalar NIC menos en los servidores.

Al implementar convergente NIC con hosts de Windows Server 2016 Hyper-V y los conmutadores virtuales de Hyper-V, las VNIC en los hosts de Hyper-V exponen los servicios RDMA para procesos de host mediante RDMA sobre cualquier Ethernet\-en función de la tecnología RDMA.

>[!NOTE]
>Para utilizar la tecnología de NIC convergente, los adaptadores de red certificada en los servidores deben admitir RDMA.

Esta guía proporciona dos conjuntos de instrucciones, uno para las implementaciones donde los servidores tienen un único adaptador de red instalado, que es una implementación básica de NIC convergente; y otro conjunto de instrucciones donde los servidores tienen dos o más adaptadores de red instalados, que es una implementación de NIC convergente a través de un Switch Embedded Teaming \(establecer\) team de RDMA\-adaptadores de red compatibles con.


## <a name="prerequisites"></a>Requisitos previos

Estos son los requisitos previos para las implementaciones de Basic y el centro de datos de convergente NIC.

>[!NOTE]
>Los ejemplos proporcionados, usamos un adaptador de Ethernet Mellanox ConnectX-3 Pro 40 Gbps, pero puede usar alguno de los servidores de Windows certified RDMA\-adaptadores de red habilitados para que admitan esta característica. Para obtener más información acerca de los adaptadores de red compatibles, vea el tema de Windows Server Catalog [tarjetas LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Requisitos básicos de NIC convergente

Para llevar a cabo los pasos de esta guía para la configuración de NIC convergente básica, debe tener lo siguiente.

- Dos servidores que ejecutan Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Uno compatible con RDMA, adaptador de red instalado en cada servidor de certificados.
- Rol de servidor Hyper-V instalado en cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Requisitos previos del centro de datos convergente NIC

Para llevar a cabo los pasos de esta guía para la configuración de NIC convergente de centro de datos, debe tener lo siguiente.

- Dos servidores que ejecutan Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Dos compatibles con RDMA, certified adaptadores de red instalados en cada servidor.
- Rol de servidor Hyper-V instalado en cada servidor.
- Debe estar familiarizado con Switch Embedded Teaming \(establecer\), una solución que se usa en entornos que incluyen Hyper-V y la pila de redes definidas por Software (SDN) en Windows Server 2016 de formación de equipos de NIC alternativo. CONJUNTO integra alguna funcionalidad de formación de equipos NIC en el conmutador Virtual de Hyper-V. Para obtener más información, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Temas relacionados
- [Configuración de NIC convergentes con un único adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC asociadas](cnic-datacenter.md)
- [Configuración del conmutador físico de NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas convergente configuraciones de NIC](cnic-app-troubleshoot.md)

---