---
title: nslookup set srchlist
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb93a9f7cf969161536e88bec929b7e6ba0f0e5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372770"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre de dominio y la lista de búsqueda predeterminados del sistema de nombres de dominio (DNS).

## <a name="syntax"></a>Sintaxis
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                        Descripción                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica los nombres nuevos para el dominio DNS y la lista de búsqueda predeterminados. El valor de nombre de dominio predeterminado se basa en el nombre de host. Puede especificar un máximo de seis nombres separados por barras diagonales (/). |
| {ayuda &#124; ?} |                                                                   Muestra un breve resumen de los subcomandos de **nslookup** .                                                                   |

## <a name="remarks"></a>Comentarios
- El comando **set srchlist**invalida el nombre de dominio DNS y la lista de búsqueda predeterminados del comando **set Domain** . Use el comando **establecer todo** para mostrar la lista.
  ## <a name="BKMK_examples"></a>Example
  En el ejemplo siguiente se establece el dominio DNS en mfg.widgets.com y la lista de búsqueda en los tres nombres:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup establecer dominio](nslookup-set-domain.md)
  [nslookup establecer todo](nslookup-set-all.md)
