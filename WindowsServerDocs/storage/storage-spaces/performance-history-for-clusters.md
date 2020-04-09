---
title: Historial de rendimiento de clústeres
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7a5eec986d6e7d633f1917c599ab6fcd244c7008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856208"
---
# <a name="performance-history-for-clusters"></a>Historial de rendimiento de clústeres

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe el historial de rendimiento recopilado para los clústeres.

No hay ninguna serie que se origine en el nivel de clúster. En su lugar, se agregan las series de servidores, como `clusternode.cpu.usage`, para todos los servidores del clúster. Las series de volumen, como `volume.iops.total`, se agregan para todos los volúmenes del clúster. Y la serie de unidades, como `physicaldisk.size.total`, se agregan a todas las unidades del clúster.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
