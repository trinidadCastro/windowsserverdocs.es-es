---
title: create volume simple
description: Windows Commands tema para Create Volume simple, que crea un volumen simple en el disco dinámico especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c707a92692c5531a0e33c9537705558f2ac309
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846898"
---
# <a name="create-volume-simple"></a>create volume simple

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volumen simple en el disco dinámico especificado.  
  
> [!IMPORTANT]  
> en Windows Vista, este comando DiskPart solo está disponible en las ediciones Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.
  
## <a name="syntax"></a>Sintaxis  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                                                            Descripción                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tamaño\=<n>  |                                                                  Tamaño del volumen en megabytes \(MB\). Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco.                                                                   |
| <n> de\=de disco  |                                                                                El disco dinámico en el que se crea el volumen. Si no se especifica ningún disco, se usa el disco actual.                                                                                |
| alinear\=<n> | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con el número de unidad lógica RAID de hardware \(matrices\) LUN para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|   noerr    |                               solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                |
  
## <a name="remarks"></a>Comentarios  
  
-   Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para crear un volumen de 1000 megabytes de tamaño, en el disco 1, escriba:  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

