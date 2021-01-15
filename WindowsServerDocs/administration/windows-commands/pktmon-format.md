---
title: formato pktmon
description: Artículo de referencia para el comando pktmon Format.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: f40ea62f4a15bc52a18a8e9bbccdc45745aaa93b
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241163"
---
# <a name="pktmon-format"></a>formato pktmon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El formato Pktmon convierte los archivos de registro en formato de texto.

## <a name="syntax"></a>Sintaxis

```
pktmon format log.etl [-o log.txt] [-b] [-v [level]] [-x] [-e] [-l [port]
```

### <a name="parameters"></a>Parámetros

| **Parámetro** | **Descripción** |
| ------------- | --------------- |
| **-o,--out** | Nombre del archivo de texto con formato. |
| **-s,--solo estadísticas** | Muestra información estadística del archivo de registro. |
| **-b,--Brief** | Formato de paquetes abreviado. |
| **-v, --verbose** | Nivel de detalle [1.. 3]. |
| **-x,--HEX** | Formato hexadecimal. |
| **-e,--no-Ethernet** | No imprima el encabezado Ethernet. |
| **-l,--vxlan** | Puerto de VXLAN personalizado. |

## <a name="additional-references"></a>Referencias adicionales

- [Pktmon](pktmon.md)
- [Comp. Pktmon](pktmon-comp.md)
- [Contadores de Pktmon](pktmon-counters.md)
- [Filtro de Pktmon](pktmon-filter.md)
- [Pktmon agregar filtro](pktmon-filter-add.md)
- [Lista de Pktmon](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Restablecimiento de Pktmon](pktmon-reset.md)
- [Inicio de Pktmon](pktmon-start.md)
- [Descarga de Pktmon](pktmon-unload.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
