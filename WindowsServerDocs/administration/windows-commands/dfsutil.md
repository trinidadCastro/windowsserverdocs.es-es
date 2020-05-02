---
title: Dfsutil
description: Tema de referencia de Dfsutil, que administra los espacios de nombres DFS, los servidores y los clientes. los comandos Dfsutil usan la terminología de Sistema de archivos distribuido original, con la terminología de espacios de nombres DFS actualizada que se proporciona como explicación para la mayoría de los comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 999eef79227d4531ba724c9cac40127297ea38a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719513"
---
# <a name="dfsutil"></a>Dfsutil

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando Dfsutil administra espacios de nombres DFS, servidores y clientes.

>[!NOTE]
>El **módulo espacios de nombres DFS** proporciona reemplazos para algunos de los parámetros de Dfsutil, mientras que otros aún requieren el uso de Dfsutil. Para obtener más información sobre los equivalentes de PowerShell actualizados, vea [DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps).

## <a name="parameters-available-in-powershell"></a>Parámetros disponibles en PowerShell

Puede usar los siguientes parámetros de PowerShell:

| Parámetro | Descripción |
| --------- | ----------- |
| root | Muestra, crea, quita, importa y exporta raíces de espacio de nombres. |
| link | Muestra, crea, quita o mueve carpetas (vínculos). |
| Destino | Muestra, crea, quita el destino de carpeta o el servidor de espacio de nombres. |
| propiedad | Muestra o modifica un destino de carpeta o un servidor de espacio de nombres. |
| server | Muestra o modifica la configuración del espacio de nombres. |
| dominio | Muestra todos los espacios de nombres basados en dominio de un dominio. |

## <a name="parameters-only-available-in-dfsutil"></a>Los parámetros solo están disponibles en Dfsutil

Puede usar los siguientes parámetros solo de Dfsutil.

| Parámetro | Descripción |
| --------- | ----------- |
| cliente | Muestra o modifica la información del cliente o las claves del registro. |
| diagnóstico | Realizar diagnósticos o ver dfsdirs/dfspath. |
| caché | Muestra o vacía la memoria caché del cliente. |

Para obtener más información acerca de cada uno de estos comandos, abra un símbolo del sistema en un servidor que tenga instaladas las herramientas de administración `dfsutil client /?`de `dfsutil diag /?`espacios de `dfsutil cache /?`nombres DFS y, a continuación, escriba, o.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)