---
title: Historial de rendimiento de los adaptadores de red
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239242"
---
# Historial de rendimiento de los adaptadores de red

> Se aplica a: Windows Server Insider Preview

Este tema secundarias de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) se describe con detalle el historial de rendimiento recopilado para adaptadores de red. Historial de rendimiento de adaptador de red está disponible para todos los adaptadores de red física en cada servidor del clúster. Historial de rendimiento de acceso directo a memoria (RDMA) remoto está disponible para todos los adaptadores de red física con RDMA habilitado.

   > [!NOTE]
   > Historial de rendimiento no se pueden recopilar para los adaptadores de red en un servidor que está inactivo. Colección reanudará automáticamente cuando el servidor se vuelve.

## Unidades y nombres de las series

Estas series se reciben para cada adaptador de red apto:

| Serie                               | Unidad            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits por segundo |
| `netadapter.bandwidth.outbound`      | bits por segundo |
| `netadapter.bandwidth.total`         | bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | bits por segundo |
| `netadapter.bandwidth.rdma.total`    | bits por segundo |

   > [!NOTE]
   > Historial de rendimiento de adaptador de red se registra en **bits** por segundo, no en bytes por segundo. Un adaptador de red de 10 GbE puede enviar y recibir aproximadamente 1.000.000.000 bits = 125,000,000 bytes = 1,25 GB por segundo en su máximo teórico.

## Cómo interpretar

| Serie                               | Cómo interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Velocidad de datos recibidos por el adaptador de red.                         |
| `netadapter.bandwidth.outbound`      | Velocidad de datos enviados por el adaptador de red.                             |
| `netadapter.bandwidth.total`         | Velocidad total de datos recibidos o enviados por el adaptador de red.           |
| `netadapter.bandwidth.rdma.inbound`  | Velocidad de los datos recibidos a través de RDMA mediante el adaptador de red.               |
| `netadapter.bandwidth.rdma.outbound` | Velocidad de datos enviados a través de RDMA por el adaptador de red.                   |
| `netadapter.bandwidth.rdma.total`    | Velocidad total de los datos recibidos o enviados a través de RDMA mediante el adaptador de red. |

## Donde proceden de

El `bytes.*` serie se recopila desde el `Network Adapter` contador de rendimiento que se establece en el servidor donde está instalado el adaptador de red, una instancia por adaptador de red.

| Serie                           | Contador de origen           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 x `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 x `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 x `Bytes Total/sec`    |

El `rdma.*` serie se recopila desde el `RDMA Activity` contador de rendimiento que se establece en el servidor donde está instalado el adaptador de red, una instancia por adaptador de red con RDMA habilitado.

| Serie                               | Contador de origen           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 x `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 x `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 x *suma de las anteriores.*   |

   > [!NOTE]
   > Contadores se miden a través de todo el intervalo, que no se muestrea. Por ejemplo, si el adaptador de red está inactivo 9 segundos, pero las transferencias de bits de 200 en el segundo 10, su `netadapter.bandwidth.total` se registrará como 20 bits por segundo por término medio durante este intervalo de 10 segundos. Esto garantiza su historia de rendimiento captura toda la actividad y es eficaz al ruido.

## Uso de PowerShell

Usa el cmdlet [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## Ver también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
