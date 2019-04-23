---
title: nslookup server
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52a846b1084380d0b40d58d81c11d20dacb407bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869156"
---
# <a name="nslookup-server"></a>nslookup server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el servidor predeterminado para el dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
server <DNSDomain>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<DNSDomain>|Obligatorio. Especifica el nuevo dominio DNS para el servidor predeterminado.|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   El **server** comando usa el servidor predeterminado actual para buscar la información acerca del dominio DNS especificado. Esto es por el contrario el **lserver** comando, que utiliza el servidor inicial.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup lserver](nslookup-lserver.md)
