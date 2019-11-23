---
title: nslookup ls
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecc419a72599b661865af6283821129a7021938a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373092"
---
# <a name="nslookup-ls"></a>nslookup ls

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información de un dominio de sistema de nombres de dominio (DNS).
## <a name="syntax"></a>Sintaxis
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | En la tabla siguiente se enumeran las opciones válidas.<br /><br />--t: enumera todos los registros del tipo especificado. Para obtener una descripción de <querytype>, consulte **SetQueryType** en referencias adicionales.<br />--a: enumera los alias de los equipos del dominio DNS. Este parámetro es un sinónimo de **-t CNAME**<br />--d: enumera todos los registros del dominio DNS. Este parámetro es un sinónimo de **-t any**<br />--h: muestra información de CPU y del sistema operativo para el dominio DNS. Este parámetro es un sinónimo de **-t HINFO**<br />--s: enumera los servicios conocidos de los equipos del dominio DNS. Este parámetro es un sinónimo de **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Especifica el dominio DNS para el que desea obtener información.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Especifica un nombre de archivo en el que se guardará la salida. Puede usar los caracteres mayor que (>) y Double mayor que (> >) para redirigir la salida de la manera habitual.                                                                                                                                                                                                                                  |
| {ayuda &#124; ?} |                                                                                                                                                                                                                                                                                          Muestra un breve resumen de los subcomandos de **nslookup** .                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Observaciones
- La salida predeterminada contiene los nombres de equipo y sus direcciones IP. Cuando la salida se dirige a un archivo, se imprimen marcas de hash por cada 50 registros recibidos del servidor.
  ## <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup Set QueryType](nslookup-set-querytype.md)
