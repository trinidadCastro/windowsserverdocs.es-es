---
title: Introducción a las redes de contenedor
description: En este tema se ofrece información general sobre la pila de red para contenedores de Windows e incluye vínculos a instrucciones adicionales sobre la creación, configuración y administración de redes de contenedor.
manager: grcusanz
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: anpaul
author: AnirbanPaul
ms.date: 09/04/2018
ms.openlocfilehash: 2ecc240cc1886a0355b1bf1785e2f634388474bf
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716871"
---
# <a name="container-networking-overview"></a>Introducción a las redes de contenedor

>Se aplica a: Windows Server 2019, Windows Server 2016

En este tema, le proporcionamos una visión general de la pila de red para contenedores de Windows y incluimos vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar redes de contenedores.

Los contenedores de Windows Server son un método ligero de virtualización del sistema operativo que separa aplicaciones o servicios de otros servicios que se ejecutan en el mismo host de contenedor. Los contenedores de Windows funcionan de forma similar a las máquinas virtuales. Cuando está habilitado, cada contenedor tiene una vista independiente del sistema operativo, los procesos, el sistema de archivos, el registro y las direcciones IP, que se pueden conectar a las redes virtuales.

Un contenedor de Windows comparte un kernel con el host de contenedor y todos los contenedores que se ejecutan en el host. Debido al espacio compartido del kernel, estos contenedores requieren la misma versión y configuración de kernel. Los contenedores proporcionan aislamiento de aplicaciones a través de la tecnología de aislamiento de procesos y espacios de nombres.

>[!IMPORTANT]
>Los contenedores de Windows no proporcionan un límite de seguridad hostil y no se deben usar para aislar el código que no es de confianza.

Con los contenedores de Windows, puede implementar un host de Hyper-V, donde puede crear una o más máquinas virtuales en los hosts de la máquina virtual. Dentro de los hosts de la máquina virtual, los contenedores se crean y el acceso a la red se realiza a través de un conmutador virtual que se ejecuta dentro de la máquina virtual. Puede usar imágenes reutilizables almacenadas en un repositorio para implementar el sistema operativo y los servicios en contenedores. Cada contenedor tiene un adaptador de red virtual que se conecta a un conmutador virtual, reenviando el tráfico entrante y saliente. Puede adjuntar puntos de conexión de contenedor a una red de host local (como NAT), la red física o la superposición de la red virtual creada a través de la pila de SDN.

Para aplicar el aislamiento entre contenedores en el mismo host, cree un compartimiento de red para cada contenedor de Hyper-V y Windows Server. Los contenedores de Windows Server usan una vNIC de host para conectarse al conmutador virtual. Los contenedores de Hyper-V usan una NIC de máquina virtual sintética (no expuesta a la máquina virtual de utilidad) para conectarse al conmutador virtual.

## <a name="related-topics"></a>Temas relacionados

- [Redes de contenedores de Windows](/virtualization/windowscontainers/container-networking/architecture): Aprenda a crear y administrar redes de contenedor para implementaciones que no son de superposición o de Sdn.

- [Conexión de puntos de conexión de contenedor a una red virtual de inquilino](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Aprenda a crear y administrar redes de contenedor para redes virtuales de superposición con Sdn.