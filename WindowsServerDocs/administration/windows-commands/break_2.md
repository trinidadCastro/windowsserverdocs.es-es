---
title: break
description: 'Tema de los comandos de Windows para **break_2** : desasocia un volumen de instantáneas de VSS y hace que sea accesible como un volumen normal.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886336"
---
# <a name="break"></a>break



Desasocia un volumen de instantáneas de VSS y hace que sea accesible como un volumen normal. A continuación, se puede acceder el volumen mediante una letra de unidad (si se ha asignado) o el nombre del volumen. Si se utiliza sin parámetros, **salto** muestra la Ayuda en el símbolo del sistema.

> [!NOTE]
> Este comando solo es relevante para instantáneas de hardware después de la importación.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
break [writable] <SetID>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[grabable]|Permite el acceso en el volumen de lectura/escritura.|
|\<SetID>|Especifica el identificador del conjunto de copia sombra.|

## <a name="remarks"></a>Comentarios

-   Los volúmenes expuestos, al igual que las instantáneas que se originan, son de solo lectura de forma predeterminada.
-   El alias de la Id. de instantánea, que se almacena como una variable de entorno mediante el **cargar metadatos** de comandos, se pueden usar en el *SetID* parámetro.

## <a name="BKMK_examples"></a>Ejemplos

Para realizar una sombra copia con el nombre de alias Alias1 accesible como un volumen puede escribir en el sistema operativo, escriba:
```
break writable %Alias1%
```

> [!NOTE]
> Acceso al volumen se realiza directamente en el proveedor de hardware sin registro del volumen haber sido una instantánea.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)