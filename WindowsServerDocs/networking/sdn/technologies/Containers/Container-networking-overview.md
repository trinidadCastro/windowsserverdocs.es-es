---
title: Introducción a las redes de contenedores
description: En este tema es una introducción a la pila de red para contenedores de Windows e incluye vínculos a guías adicionales sobre cómo crear, configurar y administrar redes de contenedor.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886146"
---
# <a name="container-networking-overview"></a>Introducción a las redes de contenedores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se ofrece información general de la pila de red para contenedores de Windows y se incluyen vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar redes de contenedor.

Contenedores de Windows Server son un método de virtualización del sistema operativo ligero separar las aplicaciones o servicios de otros servicios que se ejecutan en el mismo host de contenedor. Función de los contenedores de Windows de forma similar a las máquinas virtuales. Cuando se habilita, cada contenedor tiene una vista independiente del sistema operativo, los procesos, sistema de archivos, el registro y las direcciones IP, que puede conectarse a redes virtuales. 

Un contenedor de Windows comparte el kernel con el host de contenedor y todos los contenedores que se ejecutan en el host. Dado el espacio de kernel compartido, estos contenedores requieren la misma configuración y versión de kernel. Los contenedores proporcionan aislamiento de aplicaciones mediante tecnología de aislamiento de proceso y espacio de nombres.

>[!IMPORTANT]
>Contenedores de Windows no proporcionan un límite de seguridad hostil y no deben usarse para aislar código no seguro. 

Contenedores de Windows, puede implementar un host de Hyper-V, donde crear uno o más máquinas virtuales en los hosts de máquina virtual. Dentro de los hosts de máquina virtual, se crean contenedores y el acceso de red es a través de un conmutador virtual que se ejecuta dentro de la máquina virtual. Puede usar reutilizables imágenes almacenadas en un repositorio para implementar el sistema operativo y servicios en contenedores. Cada contenedor tiene un adaptador de red virtual que se conecta a un conmutador virtual, reenviar el tráfico entrante y saliente. Puede asociar los puntos de conexión de contenedor a una red de host local (por ejemplo, NAT), la red física o la red virtual superpuesta creadas a través de la pila de SDN.

Para aplicar el aislamiento entre contenedores en el mismo host, cree un compartimento de red para cada contenedor de Hyper-V y Windows Server. Los contenedores de Windows Server usan una vNIC de host para conectarse al conmutador virtual. Los contenedores de Hyper-V usan una NIC de máquina virtual sintética (no expuesta a la máquina virtual de utilidad) para conectarse al conmutador virtual. 

## <a name="related-topics"></a>Temas relacionados 

- [Las redes de contenedor de Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): Aprenda a crear y administrar redes de contenedor para las implementaciones de superposición/SDN.

- [Conectar los puntos de conexión de contenedor a una red virtual de inquilino](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Aprenda a crear y administrar redes de contenedor para las redes virtuales de superposición con SDN. 