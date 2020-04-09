---
title: break
description: El tema comandos de Windows para break_2, que desasocia un volumen de instantáneas de VSS y hace que sea accesible como volumen normal.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6683c44c84f4baae5f016f7df62d5bd6591cff70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848258"
---
# <a name="break"></a>break

Desasocia un volumen de instantáneas de VSS y hace que sea accesible como volumen normal. A continuación, se puede tener acceso al volumen mediante una letra de unidad (si se asigna) o un nombre de volumen. Si se usa sin parámetros, **break** muestra la ayuda en el símbolo del sistema.

> [!NOTE]
> Este comando solo es relevante para las instantáneas de hardware después de la importación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
break [writable] <SetID>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|edita|Habilita el acceso de lectura y escritura en el volumen.|
|\<SetId >|Especifica el identificador del conjunto de instantáneas.|

## <a name="remarks"></a>Comentarios

-   Los volúmenes expuestos, como las instantáneas de las que se originan, son de solo lectura de forma predeterminada.
-   El alias del identificador de instantánea, que se almacena como una variable de entorno mediante el comando **cargar metadatos** , se puede usar en el parámetro *SetID* .

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para hacer una instantánea con el nombre de alias Alias1 accesible como volumen grabable en el sistema operativo, escriba:
```
break writable %Alias1%
```

> [!NOTE]
> El acceso al volumen se realiza directamente en el proveedor de hardware sin registrar el volumen que ha sido una instantánea.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)