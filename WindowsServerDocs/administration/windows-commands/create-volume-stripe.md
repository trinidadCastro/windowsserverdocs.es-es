---
title: Crear volumen seccionado
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ed731df4613e215fb4d0954a5b8424035b1166
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433999"
---
# <a name="create-volume-stripe"></a>Crear volumen seccionado

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea un volumen seccionado mediante dos o más discos dinámicos especificados.  
  
> [!IMPORTANT]  
> para Windows Vista, este comando DiskPart sólo está disponible en las ediciones de Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parámetros  
  
|         Parámetro         |                                                                                                                            Descripción                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |             La cantidad de espacio en disco, en megabytes \(MB\), que ocupará el volumen en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio libre restante en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos.             |
| disco\=<n>,<n>\[,<n>,...\] |                                  Los discos dinámicos en el que se crea el volumen seccionado. Necesita al menos dos discos dinámicos para crear un volumen seccionado. Cantidad de espacio igual a **tamaño\= <n>**  en cada disco se asigna.                                   |
|        Alinear\=<n>         | Alinea todas las extensiones de volumen para el límite de alineación más cercano. Se utiliza normalmente con el número de unidad lógica de RAID de hardware \(LUN\) matrices para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco para el límite de alineación más cercano. |
|           noerr           |                               sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                                |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco automáticamente.  
  
## <a name="BKMK_examples"></a>Ejemplos  
Para crear un volumen seccionado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

