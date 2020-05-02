---
title: create volume simple
description: Tema de referencia de Create Volume simple, que crea un volumen simple en el disco dinámico especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f92bd9cf92dea258c6a49dd4cf75c4aae357c88e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716930"
---
# <a name="create-volume-simple"></a>create volume simple

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
| ajusta\=<n>  |                                                                  Tamaño del volumen en megabytes \(MB.\) Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco.                                                                   |
| discos\=<n>  |                                                                                El disco dinámico en el que se crea el volumen. Si no se especifica ningún disco, se usa el disco actual.                                                                                |
| alinea\=<n> | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con matrices de LUN \(\) de número de unidad lógica RAID de hardware para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|   noerr    |                               solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                |
  
## <a name="remarks"></a>Observaciones  
  
-   Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.  
  
## <a name="examples"></a>Ejemplos  
Para crear un volumen de 1000 megabytes de tamaño, en el disco 1, escriba:  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

