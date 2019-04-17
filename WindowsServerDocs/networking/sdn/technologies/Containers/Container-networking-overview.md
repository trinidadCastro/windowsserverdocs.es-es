---
title: Introducción a las redes contenedor
description: En este tema es una descripción general de la pila de red para contenedores de Windows e incluye vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar redes de contenedor.
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
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>Introducción a las redes contenedor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema es una descripción general de la pila de red para contenedores de Windows e incluye vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar redes de contenedor.

Windows Server contenedores son un método de virtualización de sistema operativo ligero usado para separar los servicios o aplicaciones de otros servicios que se ejecutan en el mismo host de contenedor. Para habilitar esto, cada contenedor tiene su propia vista del sistema operativo, procesos, sistema de archivos, registro y direcciones IP.

Función de contenedores de Windows de forma similar a máquinas virtuales en lo que respecta a la red. Cada contenedor tiene un adaptador de red virtual que está conectado a un conmutador virtual, que se reenvía el tráfico entrante y saliente. Para aplicar el aislamiento entre contenedores en el mismo host, se crea un compartimento de la red para cada Windows Server y el contenedor de Hyper-V en la que está instalado el adaptador de red para el contenedor. Contenedores de servidor de Windows usan un vNIC Host para adjuntar al conmutador virtual. Contenedores de Hyper-V se usa una sintéticos NIC de máquina virtual (no se expone a la máquina virtual de utilidad) para adjuntar el conmutador virtual. 

Los extremos de contenedor pueden asociarse a una red de host local (por ejemplo, NAT), la red física o una red virtual superposición creados a través de la pila de Microsoft Software definido de redes (SDN). 

Para obtener más información sobre cómo crear y administrar redes de contenedor para las implementaciones de superposición o SDN, consulte la [Windows contenedor redes](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) guía en MSDN.

Para obtener más información sobre cómo crear y administrar redes de contenedor para las redes virtuales con SDN superposición, consulte [conectar extremos de contenedor a una red virtual del inquilino ](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md). 