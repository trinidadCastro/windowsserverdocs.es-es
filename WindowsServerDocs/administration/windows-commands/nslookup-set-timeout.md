---
title: nslookup set timeout
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7a35a4a8e9e9cc10ea5548875f710fb5a86036
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837816"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el número de segundos que espera una respuesta a una solicitud de búsqueda inicial.
## <a name="syntax"></a>Sintaxis
```
set timeout=<Number>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<Number>|Especifica el número de segundos que esperar una respuesta. El número predeterminado de segundos de espera es 5.|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   Cuando no se recibe una respuesta a una solicitud dentro del período de tiempo especificado, se duplica el tiempo de espera y se volverá a enviar la solicitud. Puede usar el **conjunto reintento** comandos para controlar el número de reintentos.
## <a name="BKMK_examples"></a>Ejemplos
El ejemplo siguiente establece el tiempo de espera para obtener una respuesta a 2 segundos:
```
set timeout=2
```
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup establece reintento](nslookup-set-retry.md)
