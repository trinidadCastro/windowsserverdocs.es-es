---
title: Disable-Server
description: Artículo de referencia de Disable-Server, que deshabilita todos los servicios de un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fbb6a08aef24b20a9bd1b0124b39624ae4ec919
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038183"
---
# <a name="disable-server"></a>Disable-Server

Deshabilita todos los servicios de un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/Server:\<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|

## <a name="examples"></a>Ejemplos

Para deshabilitar el servidor, ejecute una de las siguientes opciones:
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

