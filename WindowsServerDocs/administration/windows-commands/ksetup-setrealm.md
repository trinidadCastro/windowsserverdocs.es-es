---
title: ksetup setrealm
description: Artículo de referencia para el comando ksetup setrealm, que establece el nombre de un dominio Kerberos.
ms.topic: reference
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a06c5fef1127236319b580fbd6810269c58d1377
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627131"
---
# <a name="ksetup-setrealm"></a>ksetup setrealm

Establece el nombre de un dominio Kerberos.

> [!IMPORTANT]
> No se admite el establecimiento del dominio Kerberos en un controlador de dominio. Si intenta hacerlo, se producirá una advertencia y un error de comando.

## <a name="syntax"></a>Sintaxis

```
ksetup /setrealm <DNSdomainname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<DNSdomainname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM. Puede usar el nombre de dominio completo o una forma sencilla del nombre. Si no usa mayúsculas para el nombre DNS, se le pedirá que realice una comprobación para continuar. |

### <a name="examples"></a>Ejemplos

Para establecer el dominio Kerberos de este equipo en un nombre de dominio específico y para restringir el acceso por parte de un controlador que no sea de dominio al dominio Kerberos de CONTOSO, escriba:

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup removerealm](ksetup-removerealm.md)
