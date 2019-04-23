---
title: bitsadmin setnotifycmdline
description: Tema de los comandos de Windows para ***-bitsadmin setnotifycmdlineSets la línea de comandos que se ejecutará cuando el trabajo finaliza la transferencia de datos o cuando un trabajo entra en un estado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859076"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Establece la línea de comandos que se ejecutará cuando el trabajo finaliza la transferencia de datos o cuando un trabajo entra en un estado.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|ProgramName|Nombre del comando que se ejecutará cuando se complete el trabajo.|
|ProgramParameters|Los parámetros que se van a pasar al *NombrePrograma*.|

## <a name="remarks"></a>Comentarios

Puede especificar NULL para *NombrePrograma* y *ProgramParameters*. Si *NombrePrograma* es NULL, *ProgramParameters* debe ser NULL.

> [!IMPORTANT]
> Si *ProgramParameters* no es NULL y, a continuación, el primer parámetro de *ProgramParameters* debe coincidir con *NombrePrograma*.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece la línea de comandos utilizada por el servicio para ejecutar el Bloc de notas cuando el trabajo denominado *myDownloadJob* se complete.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)