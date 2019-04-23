---
title: irftp
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d469270b744a9de881efd9b0cfa6f1105f70519a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848426"
---
# <a name="irftp"></a>irftp

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía archivos a través de un vínculo de infrarrojos.    
## <a name="syntax"></a>Sintaxis  
```  
irftp [<Drive>:\] [[<path>] <FileName>] [/h][/s]  
```  

### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|Unidad:\|especifica la unidad que contiene los archivos que desea enviar a través de un vínculo de infrarrojos.|  
|[path]FileName|Especifica la ubicación y el nombre del archivo o conjunto de archivos que desea enviar a través de un vínculo de infrarrojos. Si especifica un conjunto de archivos, debe especificar la ruta de acceso completa para cada archivo.|  
|/h|Especifica el modo oculto. Cuando se utiliza el modo oculto, los archivos se envían sin mostrar el cuadro de diálogo de vínculo inalámbrico.|  
|/s|Abre el cuadro de diálogo vínculo inalámbrico, por lo que puede seleccionar el archivo o conjunto de archivos que desea enviar sin usar la línea de comandos para especificar la unidad, ruta de acceso y nombres de archivo.|  

## <a name="remarks"></a>Comentarios  
-   Antes de usar este comando, compruebe que los dispositivos que desea comunicarse a través de un vínculo de infrarrojos tienen habilitada la funcionalidad de infrarrojos y funciona correctamente y que se ha establecido un vínculo infrarrojo entre los dispositivos.  
-   Utilizado sin parámetros o con **/s**, **irftp** abre el **vínculo inalámbrico** cuadro de diálogo donde puede seleccionar los archivos que desea enviar sin usar la línea de comandos.  

## <a name="BKMK_Examples"></a>Ejemplos  
Enviar Example.txt a través del vínculo infrarrojos.  
```  
irftp c:\example.txt  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
