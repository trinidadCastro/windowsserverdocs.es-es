---
title: subst
description: Obtenga información sobre cómo asociar una ruta de acceso a una letra de unidad.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3010d1e58fbd360b8311512e6664873b020c12b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383747"
---
# <a name="subst"></a>subst



Asocia una ruta de acceso a una letra de unidad. Si se usa sin parámetros, **subst** muestra los nombres de las unidades virtuales en vigor.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|> \<unidad1:|Especifica la unidad virtual a la que desea asignar una ruta de acceso.|
|[\<Unidad2 >:]\<ruta de acceso >|Especifica la unidad física y la ruta de acceso que desea asignar a una unidad virtual.|
|/d|Elimina una unidad (virtual) sustituida.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Los siguientes comandos no funcionan y no deben usarse en las unidades que se especifican en el comando **subst** :

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   El parámetro *unidad1* debe estar en el intervalo especificado por el comando **LASTDRIVE** . Si no es así, **subst** muestra el siguiente mensaje de error:

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Example

Para crear una unidad virtual Z para la ruta de acceso B:\User\Betty\Forms, escriba:
```
subst z: b:\user\betty\forms 
```
En lugar de escribir la ruta de acceso completa, puede llegar a este directorio escribiendo la letra de la unidad virtual seguida de dos puntos, como se indica a continuación:
```
z: 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)