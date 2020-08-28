---
title: ksetup domain
description: Artículo de referencia del comando de dominio ksetup, que establece el nombre de dominio para todas las operaciones de Kerberos.
ms.topic: reference
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9e89023e127318139672581cbab267a34c67a58
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025539"
---
# <a name="ksetup-domain"></a>ksetup domain

Establece el nombre de dominio para todas las operaciones de Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<domainname>` | Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como contoso.com o contoso.|

### <a name="examples"></a>Ejemplos

Para establecer una conexión a un dominio válido, como Microsoft, con el `ksetup /mapuser` subcomando, escriba:

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

Después de una conexión correcta, recibirá un TGT nuevo o se actualizará un TGT existente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup mapuser, comando](ksetup-mapuser.md)