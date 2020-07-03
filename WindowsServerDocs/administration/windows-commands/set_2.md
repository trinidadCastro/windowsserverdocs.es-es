---
title: set_2
description: Artículo de referencia para obtener set_2, que establece el contexto, las opciones, el modo detallado y el archivo de metadatos para la creación de instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b25b30ad729eb4e1cbf455f02cdacc76c0a3ab3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934630"
---
# <a name="set_2"></a>set_2

Establece el contexto, las opciones, el modo detallado y el archivo de metadatos para la creación de la instantánea. Si se usa sin parámetros, **set** muestra todos los valores de configuración actuales.

## <a name="syntax"></a>Sintaxis

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Establecer subcomandos

|Subcomando|Descripción|
|-----------|-----------|
|context|Establece el contexto para la creación de la instantánea. Vea [establecer el contexto](set-context.md) para la sintaxis y los parámetros.|
|Opción|Establece las opciones para crear la instantánea. Vea la [opción set](set-option.md) para ver la sintaxis y los parámetros.|
|verbose|Activa o desactiva el modo de salida detallado. Vea [set verbose](set-verbose.md) para la sintaxis y los parámetros.|
|metadata|Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas. Vea [establecer metadatos](set-metadata.md) para la sintaxis y los parámetros.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)