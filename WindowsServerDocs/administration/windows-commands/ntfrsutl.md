---
title: ntfrsutl
description: Tema de referencia del comando NTFRSUTL, que vuelca las tablas internas, el subproceso y la información de memoria para el servicio de replicación de archivos de NT (NTFRS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f931d916888a372d66a1cc06cb7543067b9b9d3b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472779"
---
# <a name="ntfrsutl"></a>ntfrsutl

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Vuelca la información de las tablas internas, los subprocesos y la memoria para el servicio de replicación de archivos de NT (NTFRS) de los servidores locales y remotos. La configuración de recuperación de NTFRS en el administrador de control de servicios (SCM) puede ser fundamental para buscar y mantener eventos de registro importantes en el equipo. Esta herramienta proporciona un método práctico para revisar la configuración.

## <a name="syntax"></a>Sintaxis

```
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]
ntfrsutl[memory|threads|stage][<computer>]
ntfrsutl ds[<computer>]
ntfrsutl [sets][<computer>]
ntfrsutl [version][<computer>]
ntfrsutl poll[/quickly[=[<n>]]][/slowly[=[<n>]]][/now][<computer>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| idtable | Especifica la tabla de IDENTIFICADOres. |
| configtable | Especifica la tabla de configuración de FRS. |
| inlog | Especifica el registro de entrada. |
| outlog | Especifica el registro de salida. |
| `<computer>` | Especifica el equipo. |
| memoria | Especifica el uso de memoria. |
| subprocesos | Especifica el uso de memoria. |
| fase | Especifica el uso de memoria. |
| ds | Muestra la vista del servicio NTFRS del DS. |
| conjuntos | Especifica los conjuntos de réplicas activos. |
| version | Especifica las versiones del servicio API y NTFRS. |
| poll | Especifica los intervalos de sondeo actuales.<ul><li>`/quickly`: Sondea rápidamente hasta que recupera una configuración estable.</li><li>`/quickly=`: Sondea rápidamente cada número predeterminado de minutos.</li><li>`/quickly=<n>`: Sondea rápidamente cada *n* minutos.</li><li>`/slowly`: Sondea lentamente hasta que recupera una configuración estable.</li><li>`/slowly=`: Sondea lentamente cada número predeterminado de minutos.</li><li>`/slowly=<n>`: Sondea lentamente cada *n* minutos.</li><li>`/now`: Sondea ahora.</li></ul>|
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para determinar el intervalo de sondeo para la replicación de archivos, escriba:

```
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1
```

Para determinar la versión actual de la interfaz de programación de aplicaciones (API) NTFRS, escriba:

```
C:\Program Files\SupportTools>ntfrsutl version
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
