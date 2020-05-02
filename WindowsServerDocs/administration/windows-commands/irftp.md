---
title: irftp
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34f912878b97bd00fb1c4ea539c7ea4c1423ea59
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724811"
---
# <a name="irftp"></a>irftp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía archivos a través de un vínculo de infrarrojos.    
## <a name="syntax"></a>Sintaxis  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|Unidad:\|especifica la unidad que contiene los archivos que desea enviar a través de un vínculo de infrarrojos.|  
|camino Extensión|Especifica la ubicación y el nombre del archivo o conjunto de archivos que desea enviar a través de un vínculo de infrarrojos. Si especifica un conjunto de archivos, debe especificar la ruta de acceso completa para cada archivo.|  
|/h|Especifica el modo oculto. Cuando se utiliza el modo oculto, los archivos se envían sin mostrar el cuadro de diálogo vínculo inalámbrico.|  
|/s|Abre el cuadro de diálogo vínculo inalámbrico, de modo que puede seleccionar el archivo o el conjunto de archivos que desea enviar sin utilizar la línea de comandos para especificar la unidad, la ruta de acceso y los nombres de archivo.|  

## <a name="remarks"></a>Observaciones  
-   Antes de utilizar este comando, compruebe que los dispositivos que desea comunicar a través de un vínculo de infrarrojos tienen la funcionalidad de infrarrojos habilitada y funciona correctamente, y que se ha establecido un vínculo de infrarrojos entre los dispositivos.  
-   Si se usa sin parámetros o se usa con **/s**, **irftp** abre el cuadro de diálogo **vínculo inalámbrico** , donde puede seleccionar los archivos que desea enviar sin utilizar la línea de comandos.  

## <a name="examples"></a>Ejemplos  
Envíe el ejemplo. txt sobre el vínculo de infrarrojos.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
