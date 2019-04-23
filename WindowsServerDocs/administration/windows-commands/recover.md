---
title: recover
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 805f63e95bcb72416cdacea4ba792af8c9a96c06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813106"
---
# <a name="recover"></a>recover



Recupera información legible de un disco o defectuoso.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName>|Especifica la ubicación y el nombre del archivo que desea recuperar. *Nombre de archivo* es necesario.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **recuperar** comando lee un archivo, el sector por sector y recupera datos de los sectores buena. Se pierden datos en los sectores defectuosos.
-   Sectores dañados notifican por **chkdsk** se han marcado como "malos" cuando el disco se preparó para la operación. No suponen peligro, y **recuperar** no afecta a ellos.
-   Porque todos los datos de los sectores dañados se pierden al recuperar un archivo, debe recuperar un único archivo a la vez.
-   No se puede usar caracteres comodín (**&#42;** y **?**) con el **recuperar** comando. Debe especificar un archivo (y la ubicación del archivo si no está en el directorio actual).

## <a name="BKMK_examples"></a>Ejemplos

Para recuperar el archivo historia.txt en el directorio \Fiction en la unidad D, escriba:
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)