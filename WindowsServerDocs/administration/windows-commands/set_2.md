---
title: set_2
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862616"
---
# <a name="set2"></a>set_2



Establece el contexto, opciones, el modo detallado y archivo de metadatos para la creación de instantáneas. Si se utiliza sin parámetros, **establecer** enumera todas las configuraciones actuales.

## <a name="syntax"></a>Sintaxis

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Conjunto de subcomandos.

|Subcomando|Descripción|
|-----------|-----------|
|context|Establece el contexto de creación de instantáneas. Consulte [establecer el contexto de](set-context.md) para la sintaxis y los parámetros.|
|Opción|Establece las opciones de creación de instantáneas. Consulte [establecer opción](set-option.md) para la sintaxis y los parámetros.|
|detallado|Activa o desactiva la el modo de salida detallada. Consulte [detallado](set-verbose.md) para la sintaxis y los parámetros.|
|metadata|Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas. Consulte [establecer metadatos](set-metadata.md) para la sintaxis y los parámetros.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)