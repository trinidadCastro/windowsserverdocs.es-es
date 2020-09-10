---
title: nslookup set domain
description: Artículo de referencia del comando Nslookup Set Domain, que cambia el nombre de dominio del sistema de nombres de dominio (DNS) predeterminado por el nombre especificado.
ms.topic: reference
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6bc71bc61d5329f73cf5a5dc82993cdfb0f77b50
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639623"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de dominio del sistema de nombres de dominio (DNS) predeterminado por el nombre especificado.

## <a name="syntax"></a>Sintaxis

```
set domain=<domainname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<domainname>` | Especifica un nuevo nombre para el nombre de dominio DNS predeterminado. El valor predeterminado es el nombre del host. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- El nombre de dominio DNS predeterminado se anexa a una solicitud de búsqueda según el estado de las opciones **defname** y **Search** .

- La lista de búsqueda de dominios DNS contiene los elementos primarios del dominio DNS predeterminado si tiene al menos dos componentes en su nombre. Por ejemplo, si el dominio DNS predeterminado es mfg.widgets.com, la lista de búsqueda se denomina mfg.widgets.com y widgets.com.

- Use el comando [nslookup Set srchlist](nslookup-set-srchlist.md) para especificar una lista diferente y el comando [nslookup SET ALL](nslookup-set-all.md) para mostrar la lista.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup set srchlist](nslookup-set-srchlist.md)

- [nslookup set all](nslookup-set-all.md)
