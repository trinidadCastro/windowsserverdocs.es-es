---
title: extend
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c5dfeeaec966bfab3c1de2bb91bf79c9d870401
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725659"
---
# <a name="extend"></a>extend

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

extiende el volumen o la partición con el foco y su sistema de \(archivos a\) un espacio libre sin asignar en un disco.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro  |                                                                                             Descripción                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ajusta\=<n>  |      Especifica la cantidad de espacio en megabytes \(MB\) que se va a agregar al volumen o partición actual. Si no se proporciona ningún tamaño, se usa todo el espacio libre contiguo que está disponible en el disco.       |
| discos\=<n>  |                          Especifica el disco en el que se extiende el volumen o la partición. Si no se especifica ningún disco, el volumen o la partición se amplía en el disco actual.                          |
| Systems |                                   extiende el sistema de archivos del volumen que tiene el foco. Para su uso solo en discos en los que el sistema de archivos no se extendió con el volumen.                                    |
|   noerr    | solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |
  
## <a name="remarks"></a>Observaciones  
  
-   En los discos básicos, el espacio disponible debe estar en el mismo disco que el volumen o la partición que tiene el foco. También debe seguir inmediatamente el volumen o la partición con el \(foco, que debe comenzar en el siguiente desplazamiento\)del sector.  
  
-   En los discos dinámicos con volúmenes simples o distribuidos, se puede extender un volumen a cualquier espacio disponible en cualquier disco dinámico. Con este comando, puede convertir un volumen dinámico simple en un volumen dinámico distribuido. Los volúmenes reflejados\-, RAID 5 y seccionados no se pueden extender.  
  
-   Si se formateó la partición anteriormente con el sistema de archivos NTFS, el sistema de archivos se extiende automáticamente para rellenar la partición más grande y no se producirá ninguna pérdida de datos.  
  
-   Si se formateó la partición anteriormente con un sistema de archivos distinto de NTFS, el comando produce un error sin cambiar la partición.  
  
-   Si no se formateó la partición anteriormente con un sistema de archivos, se seguirá extendiendo la partición.  
  
-   La partición debe tener un volumen asociado para poder extenderla.  
  
## <a name="examples"></a>Ejemplos  
Para extender el volumen o la partición con el foco en 500 megabytes, en el disco 3, escriba:  
  
```  
extend size=500 disk=3  
```  
  
Para extender el sistema de archivos de un volumen una vez extendido, escriba:  
  
```  
extend filesystem  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

