---
title: set_2
description: Windows Commands topic for set_2, que establece el contexto, las opciones, el modo detallado y el archivo de metadatos para la creación de instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa467625997824a11b2303572a063d591f59bdd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834388"
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
|contexto|Establece el contexto para la creación de la instantánea. Vea [establecer el contexto](set-context.md) para la sintaxis y los parámetros.|
|opción|Establece las opciones para crear la instantánea. Vea la [opción set](set-option.md) para ver la sintaxis y los parámetros.|
|detallado|Activa o desactiva el modo de salida detallado. Vea [set verbose](set-verbose.md) para la sintaxis y los parámetros.|
|metadata|Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas. Vea [establecer metadatos](set-metadata.md) para la sintaxis y los parámetros.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)