---
title: literal_1 de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 693916507488a9a480315a8e9299baa93a223b8a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438658"
---
# <a name="ftp-literal1"></a>FTP: literal_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envía textuales argumentos para el servidor ftp remoto. Se devuelve un código de respuesta única de ftp.   

## <a name="syntax"></a>Sintaxis  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se enviará al servidor ftp. |

## <a name="remarks"></a>Comentarios  
El **literal** comando es idéntico a la **oferta** comando.  
## <a name="BKMK_Examples"></a>Ejemplos  
Enviar un **salga** comando al servidor ftp remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: quote](ftp-quote.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
