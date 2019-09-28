---
title: bitsadmin setnotifycmdline
description: Temas de comandos de Windows para * * * *-bitsadmin setnotifycmdlineSets comando de línea de comandos que se ejecutará cuando el trabajo termine de transferir datos o cuando un trabajo entre en un estado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a307fe552e7d8ec5852de953a3a439cb02246ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380482"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Establece el comando de línea de comandos que se ejecutará cuando el trabajo finalice la transferencia de datos o cuando un trabajo entre en un estado.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Nombreprograma|Nombre del comando que se va a ejecutar cuando se complete el trabajo.|
|ProgramParameters|Parámetros que se van a pasar a *nombreprograma*.|

## <a name="remarks"></a>Comentarios

Puede especificar NULL para *nombreprograma* y *ProgramParameters*. Si *nombreprograma* es null, *PROGRAMPARAMETERS* debe ser null.

> [!IMPORTANT]
> Si *ProgramParameters* no es null, el primer parámetro de *ProgramParameters* debe coincidir con *nombreprograma*.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el comando de línea de comandos que usa el servicio para ejecutar el Bloc de notas cuando se completa el trabajo denominado *myDownloadJob* .
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)