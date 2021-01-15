---
title: Inicio de pktmon
description: Artículo de referencia para el comando de inicio de pktmon.
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 92e8048958dbba1e4d8b71addbc010f5ba8bc31c
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241172"
---
# <a name="pktmon-start"></a>Inicio de pktmon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

Inicia la supervisión de paquetes.

## <a name="syntax"></a>Sintaxis

```
pktmon start [-c { all | nics | [ids...] }] [-d] [--etw [-p size] [-k keywords]]
             [-f] [-s] [--log-mode {circular | multi-file | real-time | memory}]
```

### <a name="parameters"></a>Parámetros

| **Parámetro** | **Descripción** |
| ------------- | --------------- |
| **-c,--componentes** | Seleccione los componentes que desea supervisar. Puede ser todos los componentes, solo NIC o una lista de identificadores de componente. Su valor predeterminado es ALL. |
| **-d,--solo quitar** | Solo notifica los paquetes descartados. De forma predeterminada, también se muestra la propagación correcta del paquete. |
| **--ETW** | Inicie una sesión de registro para la captura de paquetes. |
| **-p,--Packet-size** | Número de bytes que se van a registrar en cada paquete. Para registrar siempre el paquete completo, establézcalo en 0. El valor predeterminado es 128 bytes. |
| **-k,--Keywords** | Máscara de máscara hexadecimal (es decir, la suma de las marcas siguientes) que controla qué eventos se registran. El valor predeterminado es 0x012. Vea las marcas de palabra clave a continuación. |
| **-f,--nombre-archivo** | archivo de registro. ETL. El valor predeterminado es PktMon. ETL. |
| **-s,--tamaño de archivo** | Tamaño máximo del archivo de registro en megabytes. El valor predeterminado es 512 MB. |
| **-l,--modo de registro** | Seleccione modo de registro. El valor predeterminado es circular. Vea modos de registro a continuación. |

### <a name="keyword-flags"></a>Marcas de palabra clave

Las marcas siguientes se aplican al parámetro **-k** o **--Keywords** (vea más arriba).

| **Marca** | **Descripción** |
| --------- | ----------- |
| **0x001** | Errores internos del monitor de paquetes.
| **0x002** | Información acerca de los componentes, los contadores y los filtros. Esta información se agrega al final del archivo de registro.
| **0x004** | Información de origen y de destino para el primer paquete del NET_BUFFER_LIST grupo.
| **0x008** | Seleccione metadatos de paquetes en NDIS_NET_BUFFER_LIST_INFO enumeración.
| **0x010** | Paquete sin formato, truncado al tamaño especificado en el parámetro [--Packet-size].

### <a name="logging-modes"></a>Modos de registro

El modo de registro se establece mediante el parámetro **-l** o **--log-Mode** (vea más arriba).

| **Modo** | **Descripción** |
| -------- | --------------- |
| **circular** | Los nuevos eventos sobrescriben los más antiguos cuando se alcanza el tamaño máximo de archivo. |
| **varios archivos** | Se crea un nuevo archivo de registro cuando se alcanza el tamaño máximo de archivo. Los archivos de registro se numeran secuencialmente: PktMon1. ETL, PktMon2. ETL, etc. |
| **en tiempo real** | Mostrar eventos y paquetes en la pantalla en tiempo real. No se crea ningún archivo de registro. Presione **Ctrl + C** para detener la supervisión. |
| **memoria** | Los eventos se escriben en un búfer de memoria circular. El tamaño del búfer se especifica en el parámetro [--File-size]. El contenido del búfer se escribe en un archivo de registro durante la operación de detención. |

## <a name="examples"></a>Ejemplos

El comando siguiente inicia la captura y habilita el registro de paquetes:

```PowerShell
C:\Test> pktmon start --etw
```

El comando siguiente inicia la captura, habilita el registro de paquetes y selecciona el modo de registro en tiempo real:

```PowerShell
C:\Test> pktmon start --etw -l real-time
```

El siguiente comando capturará los contadores de paquetes de todos los adaptadores de red sin registrar los paquetes:

```PowerShell
C:\Test> pktmon start -c nics
```

El siguiente comando capturará solo los paquetes descartados que pasan a través de los componentes 4 y 5 y los registra:

```PowerShell
C:\Test> pktmon start --etw -c 4,5 -d
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
- [Descarga de Pktmon](pktmon-unload.md)
- [Información general del monitor de paquetes](/windows-server/networking/technologies/pktmon/pktmon)
