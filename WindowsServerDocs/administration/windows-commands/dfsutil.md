---
title: Dfsutil
description: Temas de comandos de Windows para Dfsutil, que administra los espacios de nombres DFS, los servidores y los clientes. los comandos Dfsutil usan la terminología de Sistema de archivos distribuido original, con la terminología de espacios de nombres DFS actualizada que se proporciona como explicación para la mayoría de los comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30415bc85fd8a4a4804946a3d4a168d6a7d1433a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845588"
---
# <a name="dfsutil"></a>Dfsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando Dfsutil administra espacios de nombres DFS, servidores y clientes. En su lugar, la mayoría de las veces, puede usar los cmdlets de PowerShell de los espacios de nombres DFS más recientes, aunque hay algunos comandos que aún requieren Dfsutil.

## <a name="parameters-available-in-powershell"></a>Parámetros disponibles en PowerShell

Puede usar los siguientes parámetros de PowerShell:

| Parámetro | Descripción |
| --------- | ----------- |
| root | Muestra, crea, quita, importa y exporta raíces de espacio de nombres. |
| link | Muestra, crea, quita o mueve carpetas (vínculos). |
| target | Muestra, crea, quita el destino de carpeta o el servidor de espacio de nombres. |
| propiedad | Muestra o modifica un destino de carpeta o un servidor de espacio de nombres. |
| servidor | Muestra o modifica la configuración del espacio de nombres. |
| dominio | Muestra todos los espacios de nombres basados en dominio de un dominio. |

## <a name="parameters-only-available-in-dfsutil"></a>Los parámetros solo están disponibles en Dfsutil

Puede usar los siguientes parámetros solo de Dfsutil.

| Parámetro | Descripción |
| --------- | ----------- |
| cliente | Muestra o modifica la información del cliente o las claves del registro. |
| diagnóstico | Realizar diagnósticos o ver dfsdirs/dfspath. |
| almacenar | Muestra o vacía la memoria caché del cliente. |

Para obtener más información acerca de cada uno de estos comandos, abra un símbolo del sistema en un servidor que tenga instaladas las herramientas de administración de espacios de nombres DFS y, a continuación, escriba `dfsutil client /?`, `dfsutil diag /?`o `dfsutil cache /?`.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)