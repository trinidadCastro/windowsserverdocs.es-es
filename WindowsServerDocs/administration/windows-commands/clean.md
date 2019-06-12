---
title: clean
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd5eb2ec1bde4523eb6f0f919e09b9711b2654fb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434321"
---
# <a name="clean"></a>clean

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando Diskpart limpio quita cualquier partición o volumen del disco que tiene el foco de formato.
## <a name="syntax"></a>Sintaxis
```
clean [all]
```
## <a name="parameters"></a>Parámetros

| Parámetro |                                                        Descripción                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    all    | Especifica que cada sector del disco se establece en cero, lo que elimina completamente todos los datos contenidos en el disco. |

## <a name="remarks"></a>Comentarios
- En discos de arranque maestro registro (MBR), solo la particiones MBR información y sectores ocultos se sobrescriben.
- En los discos de tabla de particiones GUID (gpt), se sobrescribe la información, incluido el MBR de protección, de particiones gpt. No hay ninguna información de sectores ocultos.
- Debe seleccionarse un disco para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.
  ## <a name="BKMK_examples"></a>Ejemplos
  Para quitar todo el formato desde el disco seleccionado, escriba:
  ```
  clean
  ```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
