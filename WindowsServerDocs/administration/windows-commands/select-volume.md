---
title: select volume
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98f42324dbd4c6b3add3333cf4687d1613b1f700
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441426"
---
# <a name="select-volume"></a>select volume

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

selecciona el volumen especificado y cambia el foco a ella. Este comando también se puede usar para mostrar el volumen que tiene actualmente el foco en el disco seleccionado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro |                                                                               Descripción                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | El número del volumen para recibir el foco. Puede ver los números para todos los volúmenes en el disco seleccionado actualmente mediante el uso de la **lista volumen** comando DiskPart. |
|    <d>    |                                                 La unidad montaje o letra de punto de ruta de acceso del volumen para recibir el foco.                                                 |
  
## <a name="remarks"></a>Comentarios  
  
-   Si se especifica ningún volumen, este comando muestra el volumen que tiene actualmente el foco en el disco seleccionado.  
  
-   En un disco básico, al seleccionar un volumen también recibe el foco a la partición correspondiente.  
  
-   Si se selecciona un volumen con una partición correspondiente, se seleccionará automáticamente la partición.  
  
-   Si se ha seleccionado una partición con un volumen correspondiente, se seleccionará automáticamente el volumen.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para desplazar el foco a volumen 2, escriba:  
  
```  
select volume=2  
```  
  
Para desplazar el foco a la unidad C, escriba:  
  
```  
select volume=c  
```  
  
Para cambiar el foco en el volumen montado en una carpeta denominada "mountpath", escriba:  
  
```  
select volume=c:\mountpath  
```  
  
Para mostrar el volumen que tiene actualmente el foco en el disco seleccionado, escriba:  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

