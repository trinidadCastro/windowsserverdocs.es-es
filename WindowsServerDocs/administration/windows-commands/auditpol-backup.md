---
title: copia de seguridad Auditpol
description: 'Tema de comandos de Windows para copia de seguridad de **Auditpol** : realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b98a05740d3ce1bfe14eda4c5d97ba6c09ff32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382450"
---
# <a name="auditpol-backup"></a>copia de seguridad Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza una copia de seguridad de la configuración de la Directiva de auditoría del sistema, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditoría en un archivo de texto de valores separados por comas (CSV).

## <a name="syntax"></a>Sintaxis
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parámetros

| Parámetro |                                 Descripción                                 |
|-----------|-----------------------------------------------------------------------------|
|   /File   | Especifica el nombre del archivo en el que se realizará la copia de seguridad de la Directiva de auditoría. |
|    /?     |                    Muestra la ayuda en el símbolo del sistema.                     |

## <a name="remarks"></a>Comentarios
en el caso de las operaciones de copia de seguridad de la Directiva por usuario y de la Directiva del sistema, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de copia de seguridad con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de lista.
## <a name="BKMK_examples"></a>Example
Para realizar una copia de seguridad de la configuración de la Directiva de auditoría por usuario para todos los usuarios, la configuración de la Directiva de auditoría del sistema y todas las opciones de auditoría en un archivo de texto con formato CSV denominado AuditPolicy. csv, escriba:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Si no se especifica ninguna unidad, se usa el directorio actual.
> #### <a name="additional-references"></a>Referencias adicionales
> [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> [Auditpol restauración](auditpol-restore.md)
