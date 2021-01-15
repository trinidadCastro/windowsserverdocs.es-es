---
title: descarga de pktmon
description: Artículo de referencia para el comando pktmon unload.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: cc7313054445bdc72bd98cb60a7f4f6d11095553
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241195"
---
# <a name="pktmon-unload"></a>descarga de pktmon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

Detenga el servicio del controlador PktMon y descargue PktMon.sys. Es realmente equivalente a ' sc.exe STOP PktMon '. La medida (si está activa) se detendrá inmediatamente y cualquier estado se eliminará (contadores, filtros, etc.).

## <a name="syntax"></a>Sintaxis

```
pktmon unload
```

## <a name="additional-references"></a>Referencias adicionales

- [Pktmon](pktmon.md)
- [Comp. Pktmon](pktmon-comp.md)
- [Contadores de Pktmon](pktmon-counters.md)
- [Filtro de Pktmon](pktmon-filter.md)
- [Pktmon agregar filtro](pktmon-filter-add.md)
- [Formato Pktmon](pktmon-format.md)
- [Lista de Pktmon](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Restablecimiento de Pktmon](pktmon-reset.md)
- [Inicio de Pktmon](pktmon-start.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
