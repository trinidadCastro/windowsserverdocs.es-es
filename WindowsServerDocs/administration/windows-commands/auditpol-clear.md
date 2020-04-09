---
title: borrado de Auditpol
description: Temas de comandos de Windows para **Auditpol Clear**, que elimina la Directiva de auditoría por usuario para todos los usuarios, restablece (deshabilita) la Directiva de auditoría del sistema para todas las subcategorías y establece todas las opciones de auditoría en deshabilitado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 971f4ba54d787be29cb9e7d710f556c50c69a8dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851208"
---
# <a name="auditpol-clear"></a>borrado de Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina la Directiva de auditoría por usuario para todos los usuarios, restablece (deshabilita) la Directiva de auditoría del sistema para todas las subcategorías y establece todas las opciones de auditoría en deshabilitada.

## <a name="syntax"></a>Sintaxis

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ----------- | --------------- |
| /y | Suprime el mensaje para confirmar si se debe borrar toda la configuración de directiva de auditoría. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

En el caso de las operaciones de borrado de la Directiva por usuario y de la Directiva del sistema, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar la operación de borrado con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de borrado.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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
