---
title: set_2
description: Artículo de referencia para obtener set_2, que establece el contexto, las opciones, el modo detallado y el archivo de metadatos para la creación de instantáneas.
ms.topic: reference
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 685f66692e324b29bb0a33aaaefaec44b52b218d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639473"
---
# <a name="set_2"></a>set_2

Establece el contexto, las opciones, el modo detallado y el archivo de metadatos para la creación de la instantánea. Si se usa sin parámetros, **set** muestra todos los valores de configuración actuales.

## <a name="syntax"></a>Syntax

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