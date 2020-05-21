---
title: rpcinfo
description: Obtenga información acerca de cómo mostrar los programas en un equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4f9effa0955247ac87301f3a3933d080f7a34863
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436230"
---
# <a name="rpcinfo"></a>rpcinfo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Enumera programas en equipos remotos. La utilidad de línea de comandos **rpcinfo** realiza una llamada a procedimiento remoto (RPC) a un servidor RPC y notifica lo que encuentra.

## <a name="syntax"></a>Sintaxis
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/p [ \< Node>]|enumera todos los programas registrados con el asignador de puertos en el host especificado. Si no especifica un nombre de nodo (equipo), el programa consulta el asignador de puertos en el host local.|
|/b \< versión del programa>|Solicita una respuesta de todos los nodos de red que tienen el programa y la versión especificados registrados en el asignador de puertos. Debe especificar un nombre de programa o un número y un número de versión.|
|/t \<> de programa de nodo [ \< versión>]|Utiliza el protocolo de transporte TCP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones.|
|/u \< nodo> de programa [ \< versión>]|Utiliza el protocolo de transporte UDP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos
Para enumerar todos los programas registrados en el asignador de puertos, escriba:
```
rpcinfo /p [<Node>]
```
Para solicitar una respuesta de los nodos de red que tienen un programa especificado, escriba:
```
rpcinfo /b <Program version>
```
Para usar el protocolo de control de transmisión (TCP) para llamar a un programa, escriba:
```
rpcinfo /t <Node Program> [<version>]
```
Usar el protocolo de datagramas de usuario (UDP) para llamar a un programa:
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
