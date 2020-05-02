---
title: crear reflejo de volumen
description: Tema de referencia para crear reflejo de volumen, que crea un reflejo de volumen mediante el uso de los dos discos dinámicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eda0a6d799fc88c8382e128df1bd260b9e9426b7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716947"
---
# <a name="create-volume-mirror"></a>crear reflejo de volumen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un reflejo de volumen mediante el uso de los dos discos dinámicos especificados.  
  
> [!NOTE]  
> Este comando solo está disponible en Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|         Parámetro         |                                                                                                                                     Descripción                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         ajusta\=<n>         |                 Especifica la cantidad de espacio en disco, en \(megabytes\)MB, que ocupará el volumen en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos.                 |
| disco\=<n><n>,\[,<n>,...\] |                       Especifica los discos dinámicos en los que se crea el volumen reflejado. Necesita dos discos dinámicos para crear un volumen reflejado. En cada disco se asigna una cantidad de espacio igual al tamaño especificado con el parámetro de **tamaño** .                        |
|        alinea\=<n>         | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Este parámetro se usa normalmente con matrices de LUN \(\) de número de unidad lógica RAID de hardware para mejorar el rendimiento. *n* es el número de kilobytes \(KB\) desde el principio del disco hasta el límite de alineación más cercano. |
|           noerr           |                                        Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart se cierre con un error.                                         |
  
## <a name="remarks"></a>Observaciones  
  
-   Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.  
  
## <a name="examples"></a>Ejemplos  
Para crear un volumen reflejado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

