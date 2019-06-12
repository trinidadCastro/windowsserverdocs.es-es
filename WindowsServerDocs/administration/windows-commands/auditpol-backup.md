---
title: copia de seguridad Auditpol
description: Tema de los comandos de Windows para **auditpol backup** -hace copia de seguridad del sistema auditar la configuración de directiva, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditorías en un archivo de texto de valores separados por comas (CSV).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7de5e6dc6d205b7e6749d38ac822e31a78788c6e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435214"
---
# <a name="auditpol-backup"></a>copia de seguridad Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configuración de directiva, la configuración de directiva de auditoría por usuario para todos los usuarios y todas las opciones de auditorías en un archivo de texto de valores separados por comas (CSV) de auditoría hace copia de seguridad del sistema.

## <a name="syntax"></a>Sintaxis
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parámetros

| Parámetro |                                 Descripción                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | Especifica el nombre del archivo al que la directiva de auditoría se realizará una copia. |
|    /?     |                    Muestra la ayuda en el símbolo del sistema.                     |

## <a name="remarks"></a>Comentarios
para las operaciones de copia de seguridad de la directiva de por usuario y la directiva del sistema, se debe escribir o permiso Control total sobre ese objeto se establece en el descriptor de seguridad. También se pueden realizar operaciones de copia de seguridad que posee el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de lista.
## <a name="BKMK_examples"></a>Ejemplos
Para realizar copias de seguridad de auditoría por usuario de configuración de directiva para todos los usuarios, del sistema auditoría configuraciones de directiva y opciones de toda la auditoría en un archivo de texto con formato CSV denominado auditpolicy.csv, tipo:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Si se especifica ninguna unidad, se usa el directorio actual.
> #### <a name="additional-references"></a>Referencias adicionales
> [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> [auditpol restauración](auditpol-restore.md)
