---
title: nslookup set srchlist
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3bc06f82f557f136850872180a5c430f70da5fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888486"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre y la búsqueda de la lista de los dominios de Domain Name System (DNS) de forma predeterminada.

## <a name="syntax"></a>Sintaxis
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<DomainName>|Especifica los nombres nuevos para la lista de dominios y búsqueda DNS de forma predeterminada. El valor de nombre de dominio predeterminado se basa en el nombre de host. Puede especificar un máximo de seis nombres separados por barras diagonales (/).|
|{help &#124; ?}|Muestra un resumen breve de **nslookup** subcomandos.|
## <a name="remarks"></a>Comentarios
-   El **establecer srchlist**comando invalida la DNS dominio nombre y la búsqueda de lista predeterminada de la **dominio conjunto** comando. Use la **establecer todo** comando para mostrar la lista.
## <a name="BKMK_examples"></a>Ejemplos
El ejemplo siguiente establece el dominio DNS a mfg.widgets.com y la lista de búsqueda a los nombres de tres:
```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[nslookup establece dominio](nslookup-set-domain.md)
[nslookup establece todas](nslookup-set-all.md)
