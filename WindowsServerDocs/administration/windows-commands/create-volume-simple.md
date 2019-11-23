---
title: crear volumen simple
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1afb97c5bdb167eaf6ecfcd34ca3607b7b5a4c71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378882"
---
# <a name="create-volume-simple"></a>crear volumen simple

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un volumen simple en el disco dinámico especificado.  
  
> [!IMPORTANT]  
> en Windows Vista, este comando DiskPart solo está disponible en las ediciones Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                                                            Descripción                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tamaño\=<n>  |                                                                  Tamaño del volumen en megabytes \(MB\). Si no se proporciona ningún tamaño, el nuevo volumen ocupará el espacio libre restante en el disco.                                                                   |
| <n> de\=de disco  |                                                                                El disco dinámico en el que se crea el volumen. Si no se especifica ningún disco, se usa el disco actual.                                                                                |
| alinear\=<n> | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(matrices\) LUN para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|   Noerr    |                               solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                |
  
## <a name="remarks"></a>Observaciones  
  
-   Después de crear el volumen, el foco se desplaza automáticamente al nuevo volumen.  
  
## <a name="BKMK_examples"></a>Example  
Para crear un volumen de 1000 megabytes de tamaño, en el disco 1, escriba:  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

