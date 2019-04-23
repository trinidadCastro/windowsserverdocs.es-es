---
title: subst
description: Obtenga información sobre cómo asociar una ruta de acceso con una letra de unidad.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858076"
---
# <a name="subst"></a>subst



Asocia una ruta de acceso con una letra de unidad. Si se utiliza sin parámetros, **subst** muestra los nombres de las unidades de disco virtuales en vigor.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Drive1>:|Especifica la unidad virtual a la que desea asignar una ruta de acceso.|
|[\<Unidad2 >:]\<ruta de acceso >|Especifica la unidad física y la ruta de acceso que desea asignar a una unidad virtual.|
|/d|Elimina una unidad sustituida (virtual).|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Los comandos siguientes no funcionan y no debe usarse en las unidades que se especifican en el **subst** comando:

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   El *unidad1* parámetro debe estar dentro del intervalo especificado por el **lastdrive** comando. Si no es así, **subst** muestra el mensaje de error siguiente:

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Ejemplos

Para crear una unidad virtual Z para la ruta de acceso B:\User\Betty\Forms, escriba:
```
subst z: b:\user\betty\forms 
```
En lugar de escribir la ruta de acceso completa, puede llegar a este directorio escribiendo la letra de la unidad virtual seguida de dos puntos como sigue:
```
z: 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)