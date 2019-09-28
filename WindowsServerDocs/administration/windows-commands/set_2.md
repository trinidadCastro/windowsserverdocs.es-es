---
title: set_2
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e91bee5f0d351e461d16ccd22478d67f26887728
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370911"
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
|Contexto|Establece el contexto para la creación de la instantánea. Vea [establecer el contexto](set-context.md) para la sintaxis y los parámetros.|
|Desea|Establece las opciones para crear la instantánea. Vea la [opción set](set-option.md) para ver la sintaxis y los parámetros.|
|detallado|Activa o desactiva el modo de salida detallado. Vea [set verbose](set-verbose.md) para la sintaxis y los parámetros.|
|Repositorio|Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas. Vea [establecer metadatos](set-metadata.md) para la sintaxis y los parámetros.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)