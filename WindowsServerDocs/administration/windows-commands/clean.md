---
title: clean
description: Windows Commands tema para el comando Clean de DiskPart, que quita todas las particiones o el formato de volumen del disco que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d573a0480c24a2a622618197dea7dccaeddac271
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847758"
---
# <a name="clean"></a>clean

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de limpieza de Diskpart quita todos los formatos de partición o volumen del disco que tiene el foco.

## <a name="syntax"></a>Sintaxis
```
clean [all]
```
### <a name="parameters"></a>Parámetros

| Parámetro |                                                        Descripción                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    all    | Especifica que cada sector y cada sector del disco se establezcan en cero, lo que elimina completamente todos los datos contenidos en el disco. |

## <a name="remarks"></a>Comentarios
- En discos de registro de arranque maestro (MBR), sólo se sobrescribe la información de particiones MBR y de sectores ocultos.
- En los discos de tabla de particiones GUID (GPT), se sobrescribe la información de particiones GPT, incluido el MBR de protección. No hay información de sectores ocultos.
- Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example
  Para quitar todo el formato del disco seleccionado, escriba:
  ```
  clean
  ```

## <a name="additional-references"></a>Referencias adicionales
[Borrar disco](https://technet.microsoft.com/library/hh848661.aspx)
