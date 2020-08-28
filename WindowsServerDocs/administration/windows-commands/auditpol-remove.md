---
title: auditpol remove
description: Artículo de referencia del comando Auditpol Remove, que quita la Directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.
ms.topic: reference
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eee48822a082c3e7f5aa37cbd09a24059c94624e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029033"
---
# <a name="auditpol-remove"></a>auditpol remove

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita la Directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.

Para realizar operaciones de *eliminación* en la directiva *por usuario* , debe tener permisos de **control total** o de **escritura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *eliminación* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar las operaciones de *eliminación* generales.

## <a name="syntax"></a>Sintaxis

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /User | Especifica el identificador de seguridad (SID) o el nombre de usuario del usuario para el que se va a eliminar la Directiva de auditoría por usuario. |
| /allusers | Quita la Directiva de auditoría por usuario para todos los usuarios. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para quitar la Directiva de auditoría por usuario para Mikedan de usuario por nombre, escriba:

```
auditpol /remove /user:mikedan
```

Para quitar la Directiva de auditoría por usuario para el usuario Mikedan por SID, escriba:

```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```

Para quitar la Directiva de auditoría por usuario para todos los usuarios, escriba:

```
auditpol /remove /allusers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
