---
title: pktmon pcapng
description: Artículo de referencia para el comando pktmon pcapng.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 7b906efd5548b6aa22168294668a1bde6da3c3c8
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241155"
---
# <a name="pktmon-pcapng"></a>pktmon pcapng

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

Pktmon pcapng convierte los archivos de registro en formato pcapng. Los paquetes descartados no se incluyen de forma predeterminada.

## <a name="syntax"></a>Sintaxis

```
pktmon pcapng log.etl [-o log.pcapng]
```

### <a name="parameters"></a>Parámetros

| **Parámetro** | **Descripción** |
| ------------- | --------------- |
| **-o,--out** | Nombre del archivo pcapng con formato. |
| **-d,--solo quitar** | Solo se convierten los paquetes descartados. |
| **-c,--ID. de componente** | Filtre paquetes por un identificador de componente específico. |

## <a name="example"></a>Ejemplo

En el siguiente ejemplo solo se convierten los paquetes quitados de las tarjetas de interfaz de red en el archivo de registro *PktMon. ETL* al formato pcapng:

```powershell
C:\Test> pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

## <a name="additional-references"></a>Referencias adicionales

- [Pktmon](pktmon.md)
- [Comp. Pktmon](pktmon-comp.md)
- [Contadores de Pktmon](pktmon-counters.md)
- [Filtro de Pktmon](pktmon-filter.md)
- [Pktmon agregar filtro](pktmon-filter-add.md)
- [Formato Pktmon](pktmon-format.md)
- [Lista de Pktmon](pktmon-list.md)
- [Descarga de Pktmon](pktmon-unload.md)
- [Restablecimiento de Pktmon](pktmon-reset.md)
- [Inicio de Pktmon](pktmon-start.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
