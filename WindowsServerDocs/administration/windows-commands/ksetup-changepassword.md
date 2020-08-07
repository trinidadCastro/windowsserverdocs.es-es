---
title: ksetup changepassword
description: Artículo de referencia para el comando ksetup ChangePassword, que usa el valor de Centro de distribución de claves (KDC) Password (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión.
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69f92dc7b3f37e08e035d635a46c9fc5fc57e1a7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888052"
---
# <a name="ksetup-changepassword"></a>ksetup changepassword

Usa el valor de la contraseña del Centro de distribución de claves (KDC) (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión. La salida del comando le informa del estado de éxito o de error.

Puede comprobar si se ha establecido el valor de **Kpasswd** ; para ello, ejecute el `ksetup /dumpstate` comando y vea la salida.


## <a name="syntax"></a>Sintaxis

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<oldpassword>` | Especifica la contraseña existente del usuario que ha iniciado sesión. |
| `<newpassword>` | Especifica la nueva contraseña del usuario que ha iniciado sesión. Esta contraseña debe cumplir todos los requisitos de contraseña establecidos en este equipo. |

#### <a name="remarks"></a>Observaciones

- Si la cuenta de usuario no se encuentra en el dominio actual, el sistema le pedirá que proporcione el nombre de dominio en el que reside la cuenta de usuario.

- Si desea forzar un cambio de contraseña en el siguiente inicio de sesión, este comando permite el uso del asterisco (*), por lo que se solicitará al usuario una nueva contraseña.

-

### <a name="examples"></a>Ejemplos

Para cambiar la contraseña de un usuario que ha iniciado sesión actualmente en este equipo en este dominio, escriba:

```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```

Para cambiar la contraseña de un usuario que ha iniciado sesión actualmente en el dominio de Contoso, escriba:

```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```

Para forzar que el usuario que ha iniciado la sesión actual cambie la contraseña en el siguiente inicio de sesión, escriba:

```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup dumpstate, comando](ksetup-dumpstate.md)

- [ksetup addkpasswd, comando](ksetup-addkpasswd.md)

- [ksetup delkpasswd, comando](ksetup-delkpasswd.md)

- [ksetup dumpstate, comando](ksetup-dumpstate.md)
