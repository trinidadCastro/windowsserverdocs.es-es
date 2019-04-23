---
title: tipo de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3524c7c772cdcd54a131d8a7e8c8714fad9ce563
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858906"
---
# <a name="ftp-type"></a>FTP: tipo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece o indica el tipo de transferencia de archivos.   
## <a name="syntax"></a>Sintaxis  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<typeName>]|Especifica el tipo de transferencia de archivos.|  
## <a name="remarks"></a>Comentarios  
-   Si *typeName* no se especifica, se muestra el tipo actual.  
-   **FTP** admite dos tipos de transferencia, ASCII y binarios de archivos.  
    El tipo de transferencia de archivo predeterminado es ASCII.  El **ascii** comando se debe usar al transferir los archivos de texto. En modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de la red. Por ejemplo, se convierten los caracteres de fin de línea como necesario, según el sistema operativo en el destino.  
    El **binario** comando se debe usar al transferir los archivos ejecutables. En modo binario, el archivo se mueve en unidades de un byte.  
## <a name="BKMK_Examples"></a>Ejemplos  
Establece el tipo de transferencia de archivos en ASCII.  
```  
type ascii  
```  
Establecer tipo de archivo de la transferencia a binario.  
```  
type binary  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
