---
title: Compatibilidad con Pktmon para Wireshark (pcapng)
description: Use este tema para analizar registros de pcapng generados por el monitor de paquetes con Wireshark.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: c65604f6e3f4e87b806513063800ee62d52d1feb
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632637"
---
# <a name="pktmon-support-for-wireshark-pcapng"></a>Compatibilidad con Pktmon para Wireshark (pcapng)

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) puede convertir registros en formato pcapng. Estos registros se pueden analizar mediante Wireshark (o cualquier analizador de pcapng); sin embargo, es posible que falte parte de la información crítica en los archivos pcapng. En este tema se explican los resultados esperados y cómo aprovecharlos.

## <a name="pktmon-pcapng-syntax"></a>Sintaxis de Pktmon pcapng

Use los siguientes comandos para convertir la captura pktmon en formato pcapng.

```powershell
C:\Test> pktmon pcapng help
pktmon pcapng log.etl [-o log.pcapng]
    Convert log file to pcapng format.
    Dropped packets are not included by default.

-o, --out
    Name of the formatted pcapng file.

-d, --drop-only
    Convert dropped packets only.

-c, --component-id
    Filter packets by a specific component ID.

Example: pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

## <a name="output-filtering"></a>Filtrado de salida

Toda la información sobre los informes de colocación de paquetes y el flujo de paquetes a través de la pila de red se perderá en la salida de pcapng. Por lo tanto, el contenido del registro se debe filtrar previamente para tal conversión. Por ejemplo:

- El formato Pcapng no distingue entre un paquete de flujo y un paquete descartado. Para separar todos los paquetes de la captura de los paquetes descartados, genere dos archivos pcapng; una que contiene todos los paquetes (" **pktmon pcapng log. ETL--out log-Capture. ETL** ") y otro que solo contiene paquetes descartados (" **pktmon pcapng log. ETL--solo quitar--out log-Drop. ETL** "). De este modo, podrá analizar los paquetes colocados en un registro independiente.
- El formato Pcapng no distingue entre distintos componentes de red en los que se capturó un paquete. Para estos escenarios de varios niveles, especifique el identificador de componente deseado en la salida pcapng " **pktmon pcapng log. ETL--Component-ID 5** ". Repita este comando para cada conjunto de identificadores de componente que le interese.
