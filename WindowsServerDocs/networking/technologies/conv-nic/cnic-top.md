---
title: Guía de configuración de tarjeta de interfaz de red convergente
description: Este tema es parte de la convergido NIC configuración guía para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>Guía de configuración de red convergente interfaz tarjeta \(NIC\)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Tarjeta de interfaz de red convergente \(NIC\) permite exponer RDMA a través de una partición de host\ virtual NIC \(vNIC\) para que los servicios de la partición de host pueden acceder a \(RDMA\) remoto acceso directo de memoria en el mismo NIC que utilizan los invitados Hyper-V para el tráfico TCP/IP.

Antes de la característica convergido NIC, los servicios de administración de \(host partition\) que querían usar RDMA se necesaria para usar NIC RDMA\ capaz dedicadas, incluso si el ancho de banda, no estaba disponible en el NIC que dependían de Hyper-V Switch Virtual.

Con NIC convergido, las cargas de dos trabajo \ (usuarios de administración de traffic\ RDMA y el invitado) pueden compartir el mismo NIC físicas, lo que permite instalar NIC menos en los servidores.

Cuando implementas NIC convergido con hosts de Windows Server 2016 Hyper-V y conmutadores virtuales de Hyper-V, la vNICs en los hosts de Hyper-V exponer servicios RDMA a procesos de host mediante RDMA sobre cualquier tecnología basada en Ethernet\ RDMA.

>[!NOTE]
>Para utilizar la tecnología de NIC convergido, los adaptadores de red certificados en los servidores deben admitir RDMA.

Esta guía proporciona dos conjuntos de instrucciones, uno para las implementaciones en los servidores tienen un adaptador de red instalado, que es una implementación básica de NIC convergido; y otro conjunto de instrucciones donde los servidores tienen dos o más de los adaptadores de red instalados, que es una implementación de NIC convergido en un equipo conmutador incrustado agrupación \(SET\) RDMA\ capaz de adaptadores de red.

Esta guía contiene los siguientes temas.

- [Configuración de NIC convergente con un adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC de equipo](cnic-datacenter.md)
- [Configuración del conmutador físico para convergente NIC](cnic-app-switch-config.md)
- [Solución de problemas convergido configuraciones NIC](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>Requisitos previos

Siguiente es los requisitos previos para las implementaciones básico y Datacenter de convergido NIC

>[!NOTE]
>Para los ejemplos de esta guía, se usa un adaptador de Ethernet Mellanox ConnectX 3 Pro 40 GB/s, pero puedes usar cualquiera de Windows Server certificadas RDMA\ compatibles con adaptadores de red compatibles con esta característica. Para obtener más información acerca de los adaptadores de red compatibles, consulta el tema de catálogo de Windows Server [tarjetas LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Requisitos básicos de convergido NIC

Para realizar los pasos de esta guía para la configuración básica de NIC convergido, debe tener el siguiente.

- Dos de los servidores que ejecutan Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Una RDMA compatibles con, adaptador de red instalado en cada servidor de certificados.
- El rol de Hyper-V server debe instalarse en cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Requisitos previos Datacenter convergido NIC

Para realizar los pasos de esta guía para la configuración de NIC convergido datacenter, debe tener el siguiente.

- Dos de los servidores que ejecutan Windows Server 2016 Datacenter edition o Windows Server 2016 Standard edition.
- Dos RDMA compatibles con, certificadas adaptadores de red instalados en cada servidor.
- El rol de Hyper-V server debe instalarse en cada servidor.
- Debes estar familiarizado con el modificador incrustado agrupación \(SET\), que es una alternativa solución equipo NIC que puedes usar en entornos que incluyen Hyper-V y la pila de Software definido de redes (SDN) en Windows Server 2016. CONJUNTO integra algunas funciones de equipo NIC en el conmutador de Hyper-V Virtual. Para obtener más información, consulta [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

