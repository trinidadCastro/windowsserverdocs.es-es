---
title: etiqueta
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437912"
---
# <a name="label"></a>etiqueta



Crea, cambia o elimina la etiqueta de volumen (es decir, el nombre) de un disco. Si se utiliza sin parámetros, el **etiqueta** comando cambia la etiqueta de volumen actual o elimina la etiqueta existente.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/mp|Especifica que el volumen debe tratarse como un punto de montaje para el nombre de volumen.|
|\<Volume>|Especifica una letra de unidad (seguida de dos puntos), punto de montaje o nombre de volumen. Si se especifica un nombre de volumen, el **/mp** parámetro no es necesario.|
|\<Etiqueta >|Especifica la etiqueta para el volumen.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Windows muestra la etiqueta de volumen y el número de serie (si lo tiene) como parte de la lista de directorios.
- Una etiqueta de volumen NTFS puede ser hasta 32 caracteres de longitud, incluidos los espacios. Las etiquetas de volumen NTFS conservan y mostrar el caso de que se usó cuando creó la etiqueta.
- Si no especifica un valor para el **etiqueta** parámetro, el **etiqueta** comando muestra la salida en el formato siguiente:  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Puede escribir una nueva etiqueta de volumen o presione ENTRAR para mantener la etiqueta actual. Si se presiona ENTRAR y el volumen actualmente tiene una etiqueta, el **etiqueta** comando le pide el mensaje siguiente:  
  ```
  Delete current volume label (Y/N)?
  ```  
  Presione s para eliminar la etiqueta, o presione N para mantener la etiqueta.

## <a name="BKMK_examples"></a>Ejemplos

Para etiquetar un disco de la unidad que contiene la información de ventas de julio, escriba:
```
label a:sales-july
```
Para eliminar la etiqueta actual de la unidad C, siga estos pasos:
1. En la ventana de símbolo del sistema, escriba:  
   ```
   Label
   ```  
   Debe mostrarse un resultado similar al siguiente:  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Presione ENTRAR. Debe mostrarse el mensaje siguiente:  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Presione s para eliminar la etiqueta actual.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)