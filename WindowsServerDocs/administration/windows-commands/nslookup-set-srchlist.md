---
title: nslookup set srchlist
description: Tema de referencia del comando Nslookup Set srchlist, que cambia el nombre de dominio del sistema de nombres de dominio (DNS) y la lista de búsqueda predeterminados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed9bbce1910324c4cae5da4228a6d3d1f269d050
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721405"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de dominio y la lista de búsqueda predeterminados del sistema de nombres de dominio (DNS). Este comando invalida el nombre de dominio DNS y la lista de búsqueda predeterminados del comando [nslookup Set Domain](nslookup-set-domain.md) .

## <a name="syntax"></a>Sintaxis

```
set srchlist=<domainname>[/...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<domainname>` | Especifica los nombres nuevos para el dominio DNS y la lista de búsqueda predeterminados. El valor de nombre de dominio predeterminado se basa en el nombre de host. Puede especificar un máximo de seis nombres separados por barras diagonales (/). |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Use el comando [nslookup SET ALL](nslookup-set-all.md) para mostrar la lista.

### <a name="examples"></a>Ejemplos

Para establecer el dominio DNS en *Mfg.widgets.com* y la lista de búsqueda en los tres nombres:

```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup set domain](nslookup-set-domain.md)

- [nslookup set all](nslookup-set-all.md)
