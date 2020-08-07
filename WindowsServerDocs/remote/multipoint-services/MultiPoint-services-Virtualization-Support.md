---
title: Soporte de virtualización de MultiPoint Services
description: Describe cómo usar Multipoint Services con Hyper-V
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2a07a71703887c9636cd02ca642be4cc4ddb47b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951810"
---
# <a name="multipoint-services-virtualization-support"></a>Soporte de virtualización de MultiPoint Services
Multipoint Services es compatible con el rol de Hyper-V de dos maneras:

-   Multipoint Services se puede implementar como un sistema operativo invitado en un servidor que ejecuta Hyper-V.

-   Multipoint Services se puede usar como un servidor de virtualización.

La ejecución de Multipoint Services en una máquina virtual proporciona el uso de las herramientas de Hyper-V para administrar los sistemas operativos. Estas herramientas incluyen características de punto de comprobación y reversión, y permiten exportar e importar máquinas virtuales. En el caso de las instalaciones de mayor tamaño, puede consolidar los servidores mediante la ejecución de varios equipos virtuales de Multipoint Services en un único servidor físico. Los posibles escenarios incluyen:

-   Un único aula o laboratorio tiene más de 20 puestos. En lugar de implementar varios equipos físicos que ejecutan Multipoint Services, puede implementar varias máquinas virtuales en un solo equipo físico.

    > [!NOTE]
    > Puede administrar varios servidores Multipoint, ya sean físicos o virtuales, a través de una única consola de Multipoint Manager.

-   MultiPoint Server se está ejecutando en una máquina virtual con otra infraestructura de servidor en el mismo equipo físico. En ese caso, esta infraestructura de servidor centraliza el dominio, la seguridad y los datos de la red. MultiPoint Server proporciona Servicios de Escritorio remoto y centraliza los equipos de escritorio.

> [!NOTE]
> Al ejecutar Multipoint Services en una máquina virtual, se admiten las estaciones de cliente USB a través de Ethernet y RDP. No se admiten las estaciones conectadas del cliente USB y el vídeo directo.

Para obtener más información sobre el rol Hyper-V, consulte [Hyper-v](../../virtualization/hyper-v/hyper-v-on-windows-server.md).