---
title: clean
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f871ad1d13e06bf0cbb886ba64a52e7a55a9a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379325"
---
# <a name="clean"></a>clean

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando de limpieza de Diskpart quita todos los formatos de partición o volumen del disco que tiene el foco.
## <a name="syntax"></a>Sintaxis
```
clean [all]
```
## <a name="parameters"></a>Parámetros

| Parámetro |                                                        Descripción                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    all    | Especifica que cada sector y cada sector del disco se establezcan en cero, lo que elimina completamente todos los datos contenidos en el disco. |

## <a name="remarks"></a>Comentarios
- En los discos de registro de arranque maestro (MBR), solo se sobrescribe la información de particiones MBR y la información de sectores ocultos.
- En los discos de tabla de particiones GUID (GPT), se sobrescribe la información de particiones GPT, incluido el MBR de protección. No hay información de sectores ocultos.
- Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.
  ## <a name="BKMK_examples"></a>Example
  Para quitar todo el formato del disco seleccionado, escriba:
  ```
  clean
  ```

[Borrar disco](https://technet.microsoft.com/library/hh848661.aspx)
