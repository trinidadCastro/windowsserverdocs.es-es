---
title: Historial de rendimiento de los clústeres
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894324"
---
# <a name="performance-history-for-clusters"></a>Historial de rendimiento de los clústeres

> Se aplica a: Vista previa de Windows Server información confidencial

Este tema subcaracterística de [historial de rendimiento de almacenamiento espacios directa](performance-history.md) describe el historial de rendimiento recopilado para los clústeres.

No hay ninguna serie que se originan en el nivel de clúster. En su lugar, serie de servidor, tales como `clusternode.cpu.usage`, se agregan para todos los servidores del clúster. Serie de volumen, como `volume.iops.total`, se agregan para todos los volúmenes del clúster. Y unidad serie, tales como `physicaldisk.size.total`, se agregan para todas las unidades del clúster.

## <a name="usage-in-powershell"></a>Uso de PowerShell

Use el cmdlet [Get-clúster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de almacenamiento espacios directa](performance-history.md)
