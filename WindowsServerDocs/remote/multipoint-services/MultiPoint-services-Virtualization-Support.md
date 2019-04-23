---
title: Soporte de virtualización de MultiPoint Services
description: Describe cómo usar MultiPoint Services con Hyper-V
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f0864b8-a087-4890-94ef-05efbd3c4241
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 06d518dcea154ac2bab49a7d0e83a90f96be6e44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872526"
---
# <a name="multipoint-services-virtualization-support"></a>Soporte de virtualización de MultiPoint Services
MultiPoint Services es compatible con el rol de Hyper-V de dos maneras:  
  
-   MultiPoint Services puede implementarse como un sistema operativo en un servidor que ejecuta Hyper-V.  
  
-   MultiPoint Services puede usarse como un servidor de virtualización.   
  
Ejecutando MultiPoint Services en una máquina virtual proporciona el uso de las herramientas de Hyper-V para administrar los sistemas operativos. Estas herramientas incluyen características de punto de control y la reversión, y permiten exportar e importar máquinas virtuales. Para las instalaciones de mayor tamaño, puede consolidar servidores mediante la ejecución de varios equipos virtuales MultiPoint Services en un único servidor físico. Los posibles escenarios incluyen:  
  
-   Una clase única o laboratorio tiene más de 20 puestos. En lugar de implementar varios equipos físicos que ejecuta MultiPoint Services, puede implementar varias máquinas virtuales en un único equipo físico.  
  
    > [!NOTE]  
    > Puede administrar varios servidores MultiPoint, ya sea físico o virtual, a través de una única consola de administrador de MultiPoint.  
  
-   El servidor MultiPoint está ejecutando en una máquina virtual con otra infraestructura de servidor en el mismo equipo físico. En ese caso, esta infraestructura de servidor centraliza el dominio, seguridad y datos de la red. El servidor MultiPoint proporciona servicios de escritorio remoto y centraliza los escritorios.  
  
> [!NOTE]  
> Cuando se ejecuta MultiPoint Services en una máquina virtual, se admiten USB a través de Ethernet y las estaciones de cliente RDP. Vídeo directo y estaciones de cliente conectado USB cero no se admiten.  
  
Para obtener más información sobre el rol de Hyper-V, consulte [Hyper-V](../../virtualization/hyper-v/hyper-v-on-windows-server.md).  