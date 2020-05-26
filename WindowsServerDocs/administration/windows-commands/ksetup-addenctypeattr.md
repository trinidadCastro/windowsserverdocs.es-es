---
title: ksetup addenctypeattr
description: Tema de referencia del comando ksetup addenctypeattr, que agrega el atributo de tipo de cifrado a la lista de posibles tipos para el dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32cc87d7-b9e1-4d14-9eb7-3b439c55aa3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7162e35c88cea0cfa2828e12cc4af59eaed66c9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818155"
---
# <a name="ksetup-addenctypeattr"></a>ksetup addenctypeattr

Agrega el atributo de tipo de cifrado a la lista de posibles tipos para el dominio. Se muestra un mensaje de estado cuando se completa correctamente o con errores.

## <a name="syntax"></a>Sintaxis

```
ksetup /addenctypeattr <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
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

Para agregar el tipo de cifrado *AES-256-CTS-HMAC-SHA1-96* a la lista de posibles tipos para el dominio *Corp.contoso.com*, escriba:

```
ksetup /addenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```

Para establecer el atributo de tipo de cifrado en *AES-256-CTS-HMAC-SHA1-96* para el dominio *Corp.contoso.com*, escriba:

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

- [ksetup setenctypeattr, comando](ksetup-setenctypeattr.md)

- [ksetup getenctypeattr, comando](ksetup-getenctypeattr.md)

- [ksetup delenctypeattr, comando](ksetup-delenctypeattr.md)
