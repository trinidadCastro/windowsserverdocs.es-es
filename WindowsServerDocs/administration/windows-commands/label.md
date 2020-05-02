---
title: etiqueta
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63603eda8d23b6f7b89b8d1ba858575a60e3c65c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724511"
---
# <a name="label"></a>etiqueta



Crea, cambia o elimina la etiqueta de volumen (es decir, el nombre) de un disco. Si se usa sin parámetros, el comando **Label** cambia la etiqueta de volumen actual o elimina la etiqueta existente.



## <a name="syntax"></a>Sintaxis

```
label [/mp] [<Volume>] [<Label>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/mp|Especifica que el volumen se debe tratar como un punto de montaje o un nombre de volumen.|
|\<> de volumen|Especifica una letra de unidad (seguida de un signo de dos puntos), un punto de montaje o un nombre de volumen. Si se especifica un nombre de volumen, el parámetro **/MP** no es necesario.|
|\<> de etiqueta|Especifica la etiqueta del volumen.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Windows muestra la etiqueta de volumen y el número de serie (si tiene uno) como parte de la lista de directorios.
- Una etiqueta de volumen NTFS puede tener una longitud de hasta 32 caracteres, incluidos espacios. Las etiquetas de volumen NTFS conservan y muestran el caso que se usó cuando se creó la etiqueta.
- Si no especifica un valor para el parámetro **etiqueta** , el comando **etiqueta** mostrará el resultado en el siguiente formato:  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Puede escribir una nueva etiqueta de volumen o presionar entrar para mantener la etiqueta actual. Si presiona entrar y el volumen tiene actualmente una etiqueta, el comando **etiqueta** le pedirá el mensaje siguiente:  
  ```
  Delete current volume label (Y/N)?
  ```  
  Presione Y para eliminar la etiqueta o presione N para conservar la etiqueta.

## <a name="examples"></a>Ejemplos

Para etiquetar un disco en la unidad A que contiene información de ventas de julio, escriba:
```
label a:sales-july
```
Para eliminar la etiqueta actual de la unidad C, siga estos pasos:
1. En el símbolo del sistema, escriba:  
   ```
   Label
   ```  
   Debe mostrarse una salida similar a la siguiente:  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Presione ENTRAR. Se debe mostrar el siguiente mensaje:  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Presione Y para eliminar la etiqueta actual.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)