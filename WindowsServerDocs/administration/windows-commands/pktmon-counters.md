---
title: Contadores de pktmon
description: Artículo de referencia para el comando pktmon Counters.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: df61499d96ec8b9eee6ee2939860e95431478b52
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241222"
---
# <a name="pktmon-counters"></a>Contadores de pktmon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

Los contadores de Pktmon permiten consultar y mostrar los contadores actuales por componente para confirmar la presencia del tráfico esperado y obtener una vista de alto nivel de cómo se transmite el tráfico en la máquina.

## <a name="syntax"></a>Sintaxis

```
pktmon [comp] counters [-t { all | drop | flow }] [-z] [--json]
```

### <a name="parameters"></a>Parámetros

| **Parámetro** | **Descripción** |
| ------------- | --------------- |
| **-t,--Counter-Type** | Seleccione los tipos de contadores que desea mostrar. Los valores admitidos son todos los contadores (valor predeterminado), solo quita o solo los flujos. |
| **-z,--Mostrar-ceros** | Mostrar los contadores que son cero en ambas direcciones. |
| **-i,--Mostrar-oculto** | Mostrar los componentes que están ocultos de forma predeterminada. |
| **--JSON** | Los contadores se generan en formato JSON. |

## <a name="additional-references"></a>Referencias adicionales

- [Pktmon](pktmon.md)
- [Comp. Pktmon](pktmon-comp.md)
- [Filtro de Pktmon](pktmon-filter.md)
- [Pktmon agregar filtro](pktmon-filter-add.md)
- [Formato Pktmon](pktmon-format.md)
- [Lista de Pktmon](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Restablecimiento de Pktmon](pktmon-reset.md)
- [Inicio de Pktmon](pktmon-start.md)
- [Descarga de Pktmon](pktmon-unload.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
