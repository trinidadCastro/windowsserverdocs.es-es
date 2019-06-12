---
title: Seleccione la partición
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 79449bc74dd09246b380b3f892acc1b338650d20
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441503"
---
# <a name="select-partition"></a>Seleccione la partición

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona la partición especificada y desplaza el foco a ella. Este comando también se puede usar para mostrar de la partición que tiene actualmente el foco en el disco seleccionado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Parámetros  
  
|   Parámetro    |                                                                                    Descripción                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| partición\=<n> | El número de la partición que se va a recibir el foco. Puede ver los números de todas las particiones en el disco seleccionado actualmente mediante el uso de la **lista partición** comando DiskPart. |
  
## <a name="remarks"></a>Comentarios  
  
-   Para poder seleccionar una partición en primer lugar debe seleccionar un disco mediante el **seleccione disco** comando.  
  
-   Si se especifica ningún número de partición, este comando muestra la partición que tiene actualmente el foco en el disco seleccionado.  
  
-   Si se selecciona un volumen con una partición correspondiente, se seleccionará automáticamente la partición.  
  
-   Si se ha seleccionado una partición con un volumen correspondiente, se seleccionará automáticamente el volumen.  
  
## <a name="BKMK_examples"></a>Ejemplos  
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
  

  

