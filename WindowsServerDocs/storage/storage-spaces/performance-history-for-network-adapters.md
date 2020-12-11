---
description: 'Más información acerca de: historial de rendimiento de los adaptadores de red'
title: Historial de rendimiento de los adaptadores de red
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: c47b5f2ce60a952eb8c7773284d976e6b99d1477
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040093"
---
# <a name="performance-history-for-network-adapters"></a>Historial de rendimiento de los adaptadores de red

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para los adaptadores de red. El historial de rendimiento del adaptador de red está disponible para todos los adaptadores de red físicos en todos los servidores del clúster. El historial de rendimiento de acceso directo a memoria remota (RDMA) está disponible para todos los adaptadores de red físicos con RDMA habilitado.

   > [!NOTE]
   > No se puede recopilar el historial de rendimiento de los adaptadores de red en un servidor que está inactivo. La recopilación se reanudará automáticamente cuando el servidor vuelva a realizar la copia de seguridad.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada adaptador de red válido:

| Serie                               | Unidad            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits por segundo |
| `netadapter.bandwidth.outbound`      | bits por segundo |
| `netadapter.bandwidth.total`         | bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | bits por segundo |
| `netadapter.bandwidth.rdma.total`    | bits por segundo |

   > [!NOTE]
   > El historial de rendimiento del adaptador de red se registra en **bits** por segundo, no en bytes por segundo. El adaptador de red 1 10 GbE puede enviar y recibir aproximadamente 1 mil millones bits = 125 millones bytes = 1,25 GB por segundo en su máximo teórico.

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                               | Cómo interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Velocidad de los datos recibidos por el adaptador de red.                         |
| `netadapter.bandwidth.outbound`      | Velocidad de datos enviados por el adaptador de red.                             |
| `netadapter.bandwidth.total`         | Velocidad total de los datos recibidos o enviados por el adaptador de red.           |
| `netadapter.bandwidth.rdma.inbound`  | Velocidad de los datos recibidos a través de RDMA por el adaptador de red.               |
| `netadapter.bandwidth.rdma.outbound` | Velocidad de datos enviados a través de RDMA por el adaptador de red.                   |
| `netadapter.bandwidth.rdma.total`    | Velocidad total de los datos recibidos o enviados a través de RDMA por el adaptador de red. |

## <a name="where-they-come-from"></a>Desde dónde provienen

Las `bytes.*` series se recopilan del `Network Adapter` contador de rendimiento establecido en el servidor donde está instalado el adaptador de red, una instancia por cada adaptador de red.

| Serie                           | Contador de origen           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Las `rdma.*` series se recopilan del `RDMA Activity` contador de rendimiento establecido en el servidor donde está instalado el adaptador de red, una instancia por cada adaptador de red con RDMA habilitado.

| Serie                               | Contador de origen           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *suma de lo anterior*   |

   > [!NOTE]
   > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si el adaptador de red está inactivo durante 9 segundos pero transfiere 200 bits en el décimo segundo, se `netadapter.bandwidth.total` registrará como 20 bits por segundo en promedio durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-NetAdapter](/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referencias adicionales

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
