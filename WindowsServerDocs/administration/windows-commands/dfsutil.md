---
title: Dfsutil
description: Artículo de referencia para el comando Dfsutil, que administra los espacios de nombres DFS, los servidores y los clientes.
ms.topic: reference
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9fb3883403dd0f4a88b612655247c4f4a2df7de7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635048"
---
# <a name="dfsutil"></a>Dfsutil

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando Dfsutil administra los espacios de nombres DFS, los servidores y los clientes.

## <a name="functionality-available-in-powershell"></a>Funcionalidad disponible en PowerShell

El módulo [DFSN](/powershell/module/dfsn/?view=win10-ps) PowerShell proporciona una funcionalidad equivalente a los siguientes parámetros de Dfsutil.

| Parámetro | Descripción |
| --------- | ----------- |
| root | Muestra, crea, quita, importa y exporta raíces de espacio de nombres. |
| link | Muestra, crea, quita o mueve carpetas (vínculos). |
| Destino | Muestra, crea, quita el destino de carpeta o el servidor de espacio de nombres. |
| propiedad | Muestra o modifica un destino de carpeta o un servidor de espacio de nombres. |
| server | Muestra o modifica la configuración del espacio de nombres. |
| dominio | Muestra todos los espacios de nombres basados en dominio de un dominio. |

## <a name="functionality-available-only-in-dfsutil"></a>Funcionalidad disponible solo en Dfsutil

La funcionalidad siguiente solo está disponible como parámetros de Dfsutil:

| Parámetro | Descripción |
| --------- | ----------- |
| cliente | Muestra o modifica la información del cliente o las claves del registro. |
| diagnóstico | Realizar diagnósticos o ver dfsdirs/dfspath. |
| caché | Muestra o vacía la memoria caché del cliente. |

Para obtener más información acerca de cada uno de estos comandos, abra un símbolo del sistema en un servidor que tenga instaladas las herramientas de administración de espacios de nombres DFS y, a continuación, escriba `dfsutil client /?` , `dfsutil diag /?` o `dfsutil cache /?` .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
