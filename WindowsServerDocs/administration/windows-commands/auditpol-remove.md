---
title: quitar Auditpol
description: Tema de los comandos de Windows para **auditpol quitar** -quita la directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818306"
---
# <a name="auditpol-remove"></a>quitar Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita la directiva de auditoría por usuario para una cuenta especificada o todas las cuentas.

## <a name="syntax"></a>Sintaxis
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/user|Especifica el identificador de seguridad (SID) o nombre de usuario para el usuario para los que la directiva de auditoría por el usuario se va a eliminar.|
|/ALLUSERS|Quita la directiva de auditoría por usuario para todos los usuarios.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
Para quitar las operaciones para la directiva por usuario, debe tener permiso de escritura o Control total en ese objeto establecido en el descriptor de seguridad. También se pueden realizar operaciones de eliminación que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de eliminación.
## <a name="BKMK_examples"></a>Ejemplos
Para quitar la directiva de auditoría por usuario para el usuario Migueldom por su nombre, escriba:
```
auditpol /remove /user:mikedan
```
Para quitar la directiva de auditoría por usuario para el usuario Migueldom mediante SID, escriba:
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
Para quitar la directiva de auditoría por usuario para todos los usuarios, escriba:
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
