---
title: create volume stripe
description: Windows Commands tema para Create Volume Stripe, que crea un volumen seccionado con dos o más discos dinámicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8123d2b5852f606398e3be7161ef0341698b7fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846908"
---
# <a name="create-volume-stripe"></a>create volume stripe

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volumen seccionado mediante dos o más discos dinámicos especificados.  
  
> [!IMPORTANT]  
> en Windows Vista, este comando DiskPart solo está disponible en las ediciones Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.

## <a name="syntax"></a>Sintaxis  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|         Parámetro         |                                                                                                                            Descripción                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         tamaño\=<n>         |             La cantidad de espacio en disco, en megabytes \(MB\), que el volumen ocupará en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos.             |
| <n>de\=de disco,<n>\[,<n>,...\] |                                  Los discos dinámicos en los que se crea el volumen seccionado. Necesitará al menos dos discos dinámicos para crear un volumen seccionado. En cada disco se asigna una cantidad de espacio igual al **tamaño\=<n>** .                                   |
|        alinear\=<n>         | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(matrices\) LUN para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|           noerr           |                               solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para crear un volumen seccionado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

