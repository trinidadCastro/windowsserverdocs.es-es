---
title: auditpol list
description: Artículo de referencia del comando de lista Auditpol, que enumera las categorías y subcategorías de la Directiva de auditoría, o enumera los usuarios para los que se define una directiva de auditoría por usuario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0ce67b9907fa4c5207d75422dc972d70f5e6eea
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923712"
---
# <a name="auditpol-list"></a>auditpol list

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Enumera las categorías y subcategorías de directivas de auditoría, o muestra los usuarios para los que se ha definido una directiva de auditoría por usuario.

Para realizar operaciones de *lista* en la directiva *por usuario* , debe tener permiso de **lectura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *lista* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar las operaciones de *lista* generales.

## <a name="syntax"></a>Sintaxis

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /User | Recupera todos los usuarios para los que se ha definido la Directiva de auditoría por usuario. Si se usa con el parámetro/v, también se muestra el identificador de seguridad (SID) del usuario. |
| /categoría | Muestra los nombres de las categorías que entiende el sistema. Si se usa con el parámetro/v, también se muestra el identificador único global (GUID) de la categoría. |
| /subcategory | Muestra los nombres de las subcategorías y su GUID asociado. |
| /v | Muestra el GUID con la categoría o subcategoría, o cuando se usa con/user, muestra el SID de cada usuario. |
| /r | Muestra el resultado como un informe en formato de valores separados por comas (CSV). |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para enumerar todos los usuarios que tienen una directiva de auditoría definida, escriba:

```
auditpol /list /user
```

Para enumerar todos los usuarios que tienen una directiva de auditoría definida y su SID asociado, escriba:

```
auditpol /list /user /v
```

Para enumerar todas las categorías y subcategorías en formato de informe, escriba:

```
auditpol /list /subcategory:* /r
```

Para enumerar las subcategorías de las categorías de seguimiento detallado y acceso de DS, escriba:

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
