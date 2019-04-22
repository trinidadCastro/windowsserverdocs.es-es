---
title: Historial de rendimiento para los clústeres
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818776"
---
# <a name="performance-history-for-clusters"></a>Historial de rendimiento para los clústeres

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe el historial de rendimiento recopilado para los clústeres.

No hay ninguna serie que se originan en el nivel de clúster. En su lugar, la serie de servidor, como `clusternode.cpu.usage`, se agregan para todos los servidores del clúster. Serie de volumen, como `volume.iops.total`, se agregan para todos los volúmenes en el clúster. E impulsar la serie, como `physicaldisk.size.total`, se agregan para todas las unidades en el clúster.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
