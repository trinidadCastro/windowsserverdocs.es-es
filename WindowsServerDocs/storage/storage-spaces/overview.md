---
title: Introducción a espacios de almacenamiento
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832916"
---
# <a name="storage-spaces-overview"></a>Introducción a espacios de almacenamiento

Espacios de almacenamiento es una tecnología de Windows y Windows Server que pueden ayudar a proteger los datos de errores de disco. Es conceptualmente similar a RAID, implementado en el software. Puede usar espacios de almacenamiento para agrupar tres o más unidades en un grupo de almacenamiento y, a continuación, utilizar la capacidad de dicho grupo para crear espacios de almacenamiento. Normalmente, estos objetos almacenan copias adicionales de los datos por lo que si se produce un error en una de las unidades, todavía tiene una copia de los datos intacta. Si se está agotando en capacidad, simplemente agregar más unidades de disco al bloque de almacenamiento.

Hay cuatro maneras principales para usar espacios de almacenamiento:

- **En un equipo con Windows** : para obtener más información, consulte [espacios de almacenamiento en Windows 10](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10).
- **En un servidor independiente con todo el almacenamiento en un único servidor** : para obtener más información, consulte [implementar espacios de almacenamiento en un servidor independiente](deploy-standalone-storage-spaces.md).
- **En un servidor en clúster con espacios de almacenamiento directo con almacenamiento local conectado directamente en cada nodo del clúster** : para obtener más información, consulte [información general de espacios de almacenamiento directo](storage-spaces-direct-overview.md).
- **En un servidor en clúster con uno o más compartido SAS contenedores de almacenamiento que contiene todas las unidades** : para obtener más información, consulte [espacios de almacenamiento en un clúster con información general de SAS compartido](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

