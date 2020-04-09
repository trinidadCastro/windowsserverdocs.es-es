---
title: lista Auditpol
description: Tema comandos de Windows para la **lista Auditpol**, que enumera las categorías y subcategorías de la Directiva de auditoría, o enumera los usuarios para los que se define una directiva de auditoría por usuario.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0aff46e62ea4e4259360b78aae223dfcd66ef7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851188"
---
# <a name="auditpol-list"></a>lista Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Enumera las categorías de directivas de auditoría y/o las subcategorías, o enumera los usuarios para los que se define una directiva de auditoría por usuario.

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
| /category | Muestra los nombres de las categorías que entiende el sistema. Si se usa con el parámetro/v, también se muestra el identificador único global (GUID) de la categoría. |
| /subcategory | Muestra los nombres de las subcategorías y su GUID asociado. |
| /v | Muestra el GUID con la categoría o subcategoría, o cuando se usa con/user, muestra el SID de cada usuario. |
| /r | Muestra el resultado como un informe en formato de valores separados por comas (CSV). |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

Para todas las operaciones de lista de la Directiva por usuario, debe tener permiso de lectura en ese conjunto de objetos en el descriptor de seguridad. También puede realizar operaciones de lista con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de lista.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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
