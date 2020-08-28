---
title: rpcinfo
description: Artículo de referencia del comando rpcinfo, que enumera los programas de un equipo remoto.
ms.topic: reference
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 87f362083e828c5e4378841efd4f084ce84705a6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033753"
---
# <a name="rpcinfo"></a>rpcinfo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Enumera programas en equipos remotos. La utilidad de línea de comandos **rpcinfo** realiza una llamada a procedimiento remoto (RPC) a un servidor RPC y notifica lo que encuentra.

## <a name="syntax"></a>Sintaxis

```
rpcinfo [/p [<node>]] [/b <program version>] [/t <node program> [<version>]] [/u <node program> [<version>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /p `[<node>]` | enumera todos los programas registrados con el asignador de puertos en el host especificado. Si no especifica un nombre de nodo (equipo), el programa consulta el asignador de puertos en el host local. |
| b `<program version>` | Solicita una respuesta de todos los nodos de red que tienen el programa y la versión especificados registrados en el asignador de puertos. Debe especificar un nombre de programa o un número y un número de versión. |
| /t `<node program> [\<version>]` | Utiliza el protocolo de transporte TCP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones. |
| /u `<node program> [\<version>]` | Utiliza el protocolo de transporte UDP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para enumerar todos los programas registrados en el asignador de puertos, escriba:

```
rpcinfo /p [<node>]
```

Para solicitar una respuesta de los nodos de red que tienen un programa especificado, escriba:

```
rpcinfo /b <program version>
```

Para usar el protocolo de control de transmisión (TCP) para llamar a un programa, escriba:

```
rpcinfo /t <node program> [<version>]
```

Usar el protocolo de datagramas de usuario (UDP) para llamar a un programa:

```
rpcinfo /u <node program> [<version>]
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
