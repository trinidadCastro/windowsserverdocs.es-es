---
title: Historial de rendimiento para adaptadores de red
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849986"
---
# <a name="performance-history-for-network-adapters"></a>Historial de rendimiento para adaptadores de red

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para adaptadores de red. Historial de rendimiento del adaptador de red está disponible para cada adaptador de red físico en cada servidor del clúster. Historial de rendimiento de acceso directo a memoria (RDMA) remoto está disponible para cada adaptador de red físico con RDMA habilitado.

   > [!NOTE]
   > Historial de rendimiento no se pueden recopilar para adaptadores de red en un servidor que está inactivo. Colección reanudará automáticamente cuando el servidor vuelve a activarse.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada adaptador de red aptos:

| serie                               | Unidad            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | Bits por segundo |
| `netadapter.bandwidth.outbound`      | Bits por segundo |
| `netadapter.bandwidth.total`         | Bits por segundo |
| `netadapter.bandwidth.rdma.inbound`  | Bits por segundo |
| `netadapter.bandwidth.rdma.outbound` | Bits por segundo |
| `netadapter.bandwidth.rdma.total`    | Bits por segundo |

   > [!NOTE]
   > Historial de rendimiento del adaptador de red se registra en **bits** por segundo, no en bytes por segundo. Un adaptador de red 10 GbE puede enviar y recibir aproximadamente 1.000.000.000 bits = 125,000,000 bytes = 1,25 GB por segundo a su máximo teórico.

## <a name="how-to-interpret"></a>Cómo interpretar

| serie                               | Cómo interpretar                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Velocidad de datos recibidos por el adaptador de red.                         |
| `netadapter.bandwidth.outbound`      | Velocidad de datos enviados por el adaptador de red.                             |
| `netadapter.bandwidth.total`         | Tasa total de los datos recibidos o enviados por el adaptador de red.           |
| `netadapter.bandwidth.rdma.inbound`  | Velocidad de los datos recibidos a través de RDMA por el adaptador de red.               |
| `netadapter.bandwidth.rdma.outbound` | Velocidad de datos enviados a través de RDMA mediante el adaptador de red.                   |
| `netadapter.bandwidth.rdma.total`    | Tasa total de los datos recibidos o enviados a través de RDMA mediante el adaptador de red. |

## <a name="where-they-come-from"></a>Procedencia

El `bytes.*` serie se recopila desde el `Network Adapter` contador de rendimiento que se establezca en el servidor donde está instalado el adaptador de red, una instancia por el adaptador de red.

| serie                           | Contador de origen           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

El `rdma.*` serie se recopila desde el `RDMA Activity` contador de rendimiento que se establezca en el servidor donde está instalado el adaptador de red, una instancia por el adaptador de red con RDMA habilitado.

| serie                               | Contador de origen           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 x *suma de los anteriores*   |

   > [!NOTE]
   > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si el adaptador de red está inactivo durante 9 segundos, pero las transferencias bits 200 en la segunda de 10, su `netadapter.bandwidth.total` se registrarán como 20 bits por segundo promedio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
