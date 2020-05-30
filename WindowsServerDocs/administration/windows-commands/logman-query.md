---
title: logman query
description: Tema de referencia para el comando Logman Query, que consulta las propiedades del recopilador de datos o del conjunto de recopiladores de datos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f37b78023fd12c1d58234cdecdb69af8a1c6002d
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222817"
---
# <a name="logman-query"></a>logman query

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Consulta las propiedades del recopilador de datos o del conjunto de recopiladores de datos.

## <a name="syntax"></a>Sintaxis

```
logman query [providers|Data Collector Set name] [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s`<computer name>` | Ejecute el comando en el equipo remoto especificado. |
| -config`<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n]`<name>` | Nombre del objeto de destino. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| /? | Muestra la ayuda contextual. |

### <a name="examples"></a>Ejemplos

Para enumerar todos los conjuntos de recopiladores de datos configurados en el sistema de destino, escriba:

```
logman query
```

Para enumerar los recopiladores de datos contenidos en el conjunto de recopiladores de datos denominado *perf_log*, escriba:

```
logman query perf_log
```

Para enumerar todos los proveedores disponibles de recopiladores de datos en el sistema de destino, escriba:

```
logman query providers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman (comando)](logman.md)
