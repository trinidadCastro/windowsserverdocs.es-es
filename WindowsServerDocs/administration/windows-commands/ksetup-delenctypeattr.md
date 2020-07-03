---
title: ksetup delenctypeattr
description: Artículo de referencia de ksetup delenctypeattr, que quita el atributo de tipo de cifrado del dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5aabbb99f40d8d22189d6abccc188161b7ee2f5c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925480"
---
# <a name="ksetup-delenctypeattr"></a>ksetup delenctypeattr

Quita el atributo de tipo de cifrado del dominio. Se muestra un mensaje de estado cuando se completa correctamente o con errores.

Puede ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión; para ello, ejecute el comando **klist** y vea la salida. Puede configurar el dominio para que se conecte y use, mediante la ejecución del `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintaxis

```
ksetup /delenctypeattr <domainname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ----------| ----------- |
| `<domainname>` | Nombre del dominio en el que desea establecer una conexión. Puede usar el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso. |

### <a name="examples"></a>Ejemplos

Para determinar los tipos de cifrado actuales que se establecen en este equipo, escriba:

```
klist
```

Para establecer el dominio en mit.contoso.com, escriba:

```
ksetup /domain mit.contoso.com
```

Para comprobar cuál es el atributo de tipo de cifrado del dominio, escriba:

```
ksetup /getenctypeattr mit.contoso.com
```

Para quitar el atributo Set Encryption Type del dominio mit.contoso.com, escriba:

```
ksetup /delenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [klist (comando)](klist.md)

- [ksetup, comando](ksetup.md)

- [comando de dominio ksetup](ksetup-domain.md)

- [ksetup addenctypeattr, comando](ksetup-addenctypeattr.md)

- [ksetup setenctypeattr, comando](ksetup-setenctypeattr.md)
