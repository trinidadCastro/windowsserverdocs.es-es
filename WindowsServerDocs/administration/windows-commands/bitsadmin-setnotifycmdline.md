---
title: bitsadmin setnotifycmdline
description: Windows Commands topic for bitsadmin setnotifycmdline, que establece el comando de línea de comandos que se ejecutará cuando el trabajo termine de transferir datos o cuando un trabajo entre en un estado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761a7003e44e8dc15cb2dd2f1ce5a1a23be53286
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849338"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Establece el comando de línea de comandos que se ejecutará cuando el trabajo finalice la transferencia de datos o cuando un trabajo entre en un estado.

**BITS 1,2 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Nombreprograma|Nombre del comando que se va a ejecutar cuando se complete el trabajo.|
|ProgramParameters|Parámetros que se van a pasar a *nombreprograma*.|

## <a name="remarks"></a>Comentarios

Puede especificar NULL para *nombreprograma* y *ProgramParameters*. Si *nombreprograma* es null, *PROGRAMPARAMETERS* debe ser null.

> [!IMPORTANT]
> Si *ProgramParameters* no es null, el primer parámetro de *ProgramParameters* debe coincidir con *nombreprograma*.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el comando de línea de comandos que usa el servicio para ejecutar el Bloc de notas cuando se completa el trabajo denominado *myDownloadJob* .
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)