---
title: Introducción a los espacios de almacenamiento
ms.prod: windows-server
ms.author: jgerend
manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 60cb186cda440c96eecade92e63a7e7ed9e509bb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962067"
---
# <a name="storage-spaces-overview"></a>Introducción a los espacios de almacenamiento

Espacios de almacenamiento es una tecnología de Windows y Windows Server que puede ayudar a proteger los datos frente a errores de unidad. Es conceptualmente similar a RAID, implementado en el software. Puede usar espacios de almacenamiento para agrupar tres o más unidades en un grupo de almacenamiento y, a continuación, usar la capacidad de ese grupo para crear espacios de almacenamiento. Normalmente almacenan copias adicionales de los datos, por lo que si se produce un error en una de las unidades, seguirá teniendo una copia intacta de los datos. Si tiene poca capacidad, solo tiene que agregar más unidades al grupo de almacenamiento.

Hay cuatro maneras principales de usar espacios de almacenamiento:

- **En un equipo Windows** (para obtener más información, consulte [espacios de almacenamiento en Windows 10](https://windows.microsoft.com/windows-10/storage-spaces-windows-10)).
- **En un servidor independiente con todo el almacenamiento en un solo servidor** : para obtener más información, consulte [implementación de espacios de almacenamiento en un servidor independiente](deploy-standalone-storage-spaces.md).
- **En un servidor en clúster con espacios de almacenamiento directo con almacenamiento de conexión directa local en cada nodo del clúster** . para obtener más información, vea [información general sobre espacios de almacenamiento directo](storage-spaces-direct-overview.md).
- **En un servidor en clúster con uno o varios contenedores de almacenamiento de SAS compartidos** que contengan todas las unidades: para obtener más información, consulte [la introducción a los espacios de almacenamiento en un clúster con SAS compartidas](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)).
