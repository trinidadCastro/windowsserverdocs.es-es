---
title: pktmon
description: Artículo de referencia de la herramienta de diagnóstico de red de pktmon para Windows que se puede usar para la captura de paquetes, la detección de paquetes, el filtrado de paquetes y el recuento.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 55b8ad90ca49349482e5e4b9ccb2ad11132a044d
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241156"
---
# <a name="pktmon"></a>pktmon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) es una herramienta de diagnóstico de red integrada y entre componentes para Windows. Se puede usar para la captura de paquetes, la detección de eliminación, el filtrado y el recuento. Pktmon es especialmente útil en escenarios de virtualización como la red de contenedores y SDN, ya que proporciona visibilidad dentro de la pila de red.

## <a name="syntax"></a>Sintaxis

```
pktmon { filter | comp | reset | counters | format | list | start | stop | pcapng | unload | help } [options]
```

### <a name="commands"></a>Comandos

| **Comando** | **Descripción** |
| --------- | ----------- |
| [filtro de pktmon](pktmon-filter.md) | Administrar filtros de paquetes. |
| [comp. pktmon](pktmon-comp.md) | Administrar componentes registrados. |
| [restablecimiento de pktmon](pktmon-reset.md) | Restablezca los contadores a cero. |
| [Contadores de pktmon](pktmon-counters.md) | Consultar los contadores de paquetes. |
| [formato pktmon](pktmon-format.md) | Convertir el archivo de registro en texto. |
| [lista de pktmon](pktmon-list.md) | Enumera todos los componentes activos. |
| [Inicio de pktmon](pktmon-start.md) | Inicie la supervisión de paquetes. |
| pktmon detener | Detenga la supervisión de paquetes. |
| [pktmon pcapng](pktmon-pcapng.md) | Convierte el archivo de registro en formato pcapng. |
| [descarga de pktmon](pktmon-unload.md) | Descargue el controlador pktmon. |
| ayuda de pktmon | Muestra un breve resumen de los subcomandos. |

## <a name="additional-references"></a>Referencias adicionales

- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
- [Compatibilidad de Pktmon con Monitor de red de Microsoft (Netmon)](/windows-server/networking/technologies/pktmon/pktmon-netmon-support)
