---
title: nslookup ls
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbaef500410d6427beb008109abcc2c339b8d65c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838738"
---
# <a name="nslookup-ls"></a>nslookup ls

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información de un dominio de sistema de nombres de dominio (DNS).
## <a name="syntax"></a>Sintaxis
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | En la tabla siguiente se enumeran las opciones válidas.<p>--t: enumera todos los registros del tipo especificado. Para obtener una descripción de <querytype>, consulte **SetQueryType** en referencias adicionales.<br />--a: enumera los alias de los equipos del dominio DNS. Este parámetro es un sinónimo de **-t CNAME**<br />--d: enumera todos los registros del dominio DNS. Este parámetro es un sinónimo de **-t any**<br />--h: muestra información de CPU y del sistema operativo para el dominio DNS. Este parámetro es un sinónimo de **-t HINFO**<br />--s: enumera los servicios conocidos de los equipos del dominio DNS. Este parámetro es un sinónimo de **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Especifica el dominio DNS para el que desea obtener información.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Especifica un nombre de archivo en el que se guardará la salida. Puede usar los caracteres mayor que (>) y Double mayor que (> >) para redirigir la salida de la manera habitual.                                                                                                                                                                                                                                  |
| {ayuda &#124; ?} |                                                                                                                                                                                                                                                                                          Muestra un breve resumen de los subcomandos de **nslookup** .                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentarios
- La salida predeterminada contiene los nombres de equipo y sus direcciones IP. Cuando la salida se dirige a un archivo, se imprimen marcas de hash por cada 50 registros recibidos del servidor.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup Set QueryType](nslookup-set-querytype.md)
