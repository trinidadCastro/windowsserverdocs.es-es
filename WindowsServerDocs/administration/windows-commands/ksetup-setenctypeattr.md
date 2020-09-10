---
title: ksetup setenctypeattr
description: Artículo de referencia para el comando ksetup setenctypeattr, que establece el atributo de tipo de cifrado para el dominio.
ms.topic: reference
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9027197817b2fa738726fd0d0feeeeadc5aac0b2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634122"
---
# <a name="ksetup-setenctypeattr"></a>ksetup setenctypeattr

Establece el atributo de tipo de cifrado para el dominio. Se muestra un mensaje de estado cuando se completa correctamente o con errores.

Puede ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión; para ello, ejecute el comando **klist** y vea la salida. Puede configurar el dominio para que se conecte y use, mediante la ejecución del `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintaxis

```
ksetup /setenctypeattr <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<domainname>` | Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso. |
| tipo de cifrado | Debe ser uno de los siguientes tipos de cifrado admitidos:<ul><li>DES-CBC-CRC</li><li>DES-CBC-MD5</li><li>RC4-HMAC-MD5</li><li>AES128-CTS-HMAC-SHA1-96</li><li>AES256-CTS-HMAC-SHA1-96</li></ul> |

#### <a name="remarks"></a>Observaciones

- Puede establecer o agregar varios tipos de cifrado separando los tipos de cifrado en el comando con un espacio. Sin embargo, solo puede hacerlo para un dominio a la vez.

### <a name="examples"></a>Ejemplos

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, escriba:

```
klist
```

Para establecer el dominio en corp.contoso.com, escriba:

```
ksetup /domain corp.contoso.com
```

Para establecer el atributo de tipo de cifrado en AES-256-CTS-HMAC-SHA1-96 para el dominio corp.contoso.com, escriba:

```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```

Para comprobar que el atributo de tipo de cifrado se ha establecido como previsto para el dominio, escriba:

```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [klist (comando)](klist.md)

- [ksetup, comando](ksetup.md)

- [comando de dominio ksetup](ksetup-domain.md)

- [ksetup addenctypeattr, comando](ksetup-addenctypeattr.md)

- [ksetup getenctypeattr, comando](ksetup-getenctypeattr.md)

- [ksetup delenctypeattr, comando](ksetup-delenctypeattr.md)
