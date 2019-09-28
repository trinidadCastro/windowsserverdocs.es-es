---
title: seleccionar partición
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a186e2678fde64396a8b4b57a2d14e4b0b7bf26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371072"
---
# <a name="select-partition"></a>seleccionar partición

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona la partición especificada y desplaza el foco a ella. Este comando también se puede usar para mostrar la partición que tiene actualmente el foco en el disco seleccionado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parámetros  
  
|   Parámetro    |                                                                                    Descripción                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partition @ no__t-0 @ no__t-1 | Número de la partición que va a recibir el foco. Puede ver los números de todas las particiones del disco seleccionado actualmente mediante el comando **List Partition** en Diskpart. |
  
## <a name="remarks"></a>Comentarios  
  
-   Antes de poder seleccionar una partición, primero debe seleccionar un disco con el comando **Seleccionar disco** .  
  
-   Si no se especifica ningún número de partición, este comando muestra la partición que tiene actualmente el foco en el disco seleccionado.  
  
-   Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.  
  
-   Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.  
  
## <a name="BKMK_examples"></a>Example  
Para desplazar el foco a la partición 3, escriba:  
  
```  
select partitition=3  
```  
  
Para mostrar la partición que tiene actualmente el foco en el disco seleccionado, escriba:  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

