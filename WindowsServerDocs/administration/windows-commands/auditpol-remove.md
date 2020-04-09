---
title: quitar Auditpol
description: Temas de comandos de Windows para **quitar Auditpol**, que quita la Directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eda43d6708a31b2966022d2ae2c162bbfc888cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851178"
---
# <a name="auditpol-remove"></a>quitar Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita la Directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.

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
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

En el caso de las operaciones de eliminación de la Directiva por usuario, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de eliminación con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de eliminación.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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
