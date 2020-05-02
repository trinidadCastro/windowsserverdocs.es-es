---
title: nslookup set srchlist
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8936daa3505b02295ae2f09c2910dead8d4c0ff8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723553"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

cambia el nombre de dominio y la lista de búsqueda predeterminados del sistema de nombres de dominio (DNS).

## <a name="syntax"></a>Sintaxis
```
Set srchlist=<DomainName>[/...]
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                        Descripción                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica los nombres nuevos para el dominio DNS y la lista de búsqueda predeterminados. El valor de nombre de dominio predeterminado se basa en el nombre de host. Puede especificar un máximo de seis nombres separados por barras diagonales (/). |
| {ayuda &#124;?} |                                                                   Muestra un breve resumen de los subcomandos de **nslookup** .                                                                   |

## <a name="remarks"></a>Observaciones
- El comando **set srchlist**invalida el nombre de dominio DNS y la lista de búsqueda predeterminados del comando **set Domain** . Use el comando **establecer todo** para mostrar la lista.
  ## <a name="examples"></a>Ejemplos
  Para establecer el dominio DNS en mfg.widgets.com y la lista de búsqueda en los tres nombres:
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup establecer dominio](nslookup-set-domain.md)
  [nslookup SET ALL](nslookup-set-all.md)
