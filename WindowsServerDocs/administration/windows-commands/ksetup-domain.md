---
title: dominio ksetup
description: Tema de referencia del comando de dominio ksetup, que establece el nombre de dominio para todas las operaciones de Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d497f2bc76bae8a95b077658c661e0fdc1e93f3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817805"
---
# <a name="ksetup-domain"></a>dominio ksetup

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