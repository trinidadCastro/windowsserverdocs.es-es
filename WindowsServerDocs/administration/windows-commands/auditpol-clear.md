---
title: auditpol clear
description: Artículo de referencia del comando Auditpol Clear, que elimina la Directiva de auditoría por usuario para todos los usuarios, restablece (deshabilita) la Directiva de auditoría del sistema para todas las subcategorías y establece todas las opciones de auditoría en deshabilitado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797f26ab9e191176808bbce917ca5ac0fa3d73a3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923793"
---
# <a name="auditpol-clear"></a>auditpol clear

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina la Directiva de auditoría por usuario para todos los usuarios, restablece (deshabilita) la Directiva de auditoría del sistema para todas las subcategorías y establece todas las opciones de auditoría en deshabilitada.

Para realizar operaciones de *borrado* en las directivas *por usuario* y *del sistema* , debe tener el permiso de **control total** o de **escritura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *borrado* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar las operaciones de *borrado* generales.

## <a name="syntax"></a>Sintaxis

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ----------- | --------------- |
| /y | Suprime el mensaje para confirmar si se debe borrar toda la configuración de directiva de auditoría. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para eliminar la Directiva de auditoría por usuario para todos los usuarios, restablezca (deshabilite) la Directiva de auditoría del sistema para todas las subcategorías y establezca la configuración de la Directiva de auditoría en deshabilitado; en un mensaje de confirmación, escriba:

```
auditpol /clear
```

Para eliminar la Directiva de auditoría por usuario para todos los usuarios, restablecer la configuración de la Directiva de auditoría del sistema para todas las subcategorías y establecer la configuración de la Directiva de auditoría como deshabilitada, sin un mensaje de confirmación, escriba:

```
auditpol /clear /y
```

> [!NOTE]
> El ejemplo anterior es útil cuando se usa un script para realizar esta operación.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
