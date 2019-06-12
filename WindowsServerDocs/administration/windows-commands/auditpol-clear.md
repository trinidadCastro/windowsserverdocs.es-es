---
title: Borrar Auditpol
description: Tema de los comandos de Windows para **auditpol claro** -elimina la directiva de auditoría por usuario para todos los usuarios, se restablece (deshabilita) el sistema de auditoría de directiva para todas las subcategorías y los conjuntos de opciones de la auditoría en deshabilitado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435149"
---
# <a name="auditpol-clear"></a>Borrar Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina la directiva de auditoría por usuario para todos los usuarios, se restablece (deshabilita) el sistema de auditoría de directiva para todas las subcategorías y los conjuntos de opciones de la auditoría en deshabilitado.

## <a name="syntax"></a>Sintaxis
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parámetros

| Parámetro |                                   Descripción                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Suprime el mensaje para confirmar si se deben borrar todas las configuraciones de directiva de auditoría. |
|    /?     |                       Muestra la ayuda en el símbolo del sistema.                       |

## <a name="remarks"></a>Comentarios
para operaciones de borrado de la directiva de por usuario y la directiva del sistema, se debe escribir o permiso Control total sobre ese objeto se establece en el descriptor de seguridad. También se puede realizar la operación de borrado que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de borrado.
## <a name="BKMK_examples"></a>Ejemplos
Para eliminar la directiva de auditoría por usuario para todos los usuarios, restablecimiento de la directiva de auditoría del sistema para todas las subcategorías (deshabilitar) y establezca la configuración de directiva de auditoría en deshabilitado en un mensaje de confirmación, escriba:
```
auditpol /clear
```
Para eliminar la directiva de auditoría por usuario para todos los usuarios, restablecer la configuración de directiva de auditoría del sistema para todas las subcategorías y establezca todas la auditoría de configuración de directiva en deshabilitado, sin pedir confirmación, escriba:
```
auditpol /clear /y
```
> [!NOTE]
> El ejemplo anterior es útil cuando se usa una secuencia de comandos para realizar esta operación.
> #### <a name="additional-references"></a>Referencias adicionales
> [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
