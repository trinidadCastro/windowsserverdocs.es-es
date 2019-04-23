---
title: nslookup ls
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 632d25e29c09d7a164668128196964d082e160c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848526"
---
# <a name="nslookup-ls"></a>nslookup ls

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información de un dominio de sistema de nombres de dominio (DNS).
## <a name="syntax"></a>Sintaxis
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<Option>|En la tabla siguiente se enumera las opciones válidas.<br /><br />--t: enumera todos los registros del tipo especificado. Para obtener una descripción de <querytype>, consulte **setquerytype** en referencias adicionales.<br />--r: se enumeran los alias de equipos del dominio DNS. Este parámetro es un sinónimo de **- t CNAME**<br />--d: enumera todos los registros para el dominio DNS. Este parámetro es un sinónimo de **- t ANY**<br />--h: muestra información de CPU y el sistema operativo para el dominio DNS. Este parámetro es un sinónimo de **- t HINFO**<br />--s: enumera los servicios conocidos de los equipos del dominio DNS. Este parámetro es un sinónimo de **-t WKS**.|
|<DNSDomain>|Especifica el dominio DNS para el que desea obtener información.|
|<FileName>|Especifica un nombre de archivo en el que se va a guardar la salida. Puede utilizar el mayor que (>) y doble mayor que (>>) caracteres que se va a redirigir la salida de la manera habitual.|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   La salida predeterminada contiene los nombres de equipo y su IP direcciones. Cuando la salida se dirige a un archivo, se imprimen signos # cada 50 registros recibidos del servidor
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup establece querytype](nslookup-set-querytype.md)
