---
title: rpcinfo
description: Obtenga información sobre cómo enumerar los programas en un equipo remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4aba1e57d5a61103310fbe7abcac391e543be5aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826376"
---
# <a name="rpcinfo"></a>rpcinfo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra los programas en equipos remotos. El **rpcinfo** utilidad de línea de comandos realiza un procedimiento remoto (RPC) de llamar a un servidor RPC e informa de lo que encuentre. 

## <a name="syntax"></a>Sintaxis
```
rpcinfo [/p [<Node>]] [/b <Program version>] [/t <Node Program> [<version>]] [/u <Node Program> [<version>]]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/p [\<nodo >]|Enumera todos los programas registrados con el asignador de puerto en el host especificado. Si no especifica un nombre de nodo (equipo), el programa consulta al asignador de puerto en el host local.|
|/b \<versión del programa >|Solicita una respuesta de todos los nodos de red que tienen el programa especificado y la versión registrada con el asignador de puerto. Debe especificar un nombre de programa o el número y un número de versión.|
|/t \<nodo programa > [\<versión >]|Usa el protocolo de transporte TCP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones.|
|/u \<nodo programa > [\<versión >]|Usa el protocolo de transporte UDP para llamar al programa especificado. Debe especificar un nombre de nodo (equipo) y un nombre de programa. Si no especifica una versión, el programa llama a todas las versiones.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos
Para obtener una lista de todos los programas registrados con el asignador de puerto, escriba:
```
rpcinfo /p [<Node>]
```
Para solicitar una respuesta de nodos de la red que tienen un programa específico, escriba:
```
rpcinfo /b <Program version>
```
Para usar el protocolo de Control de transmisión (TCP) para llamar a un programa, escriba:
```
rpcinfo /t <Node Program> [<version>]
```
Use el protocolo de datagramas de usuario (UDP) para llamar a un programa:
```
rpcinfo /u <Node Program> [<version>]
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
