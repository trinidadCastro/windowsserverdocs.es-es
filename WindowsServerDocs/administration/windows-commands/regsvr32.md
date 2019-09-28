---
title: regsvr32
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 444af0ccf7c9bbe21c013f32b396997b7cb2e00f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371631"
---
# <a name="regsvr32"></a>regsvr32



Registra los archivos. dll como componentes de comando en el registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/u|Anula el registro del servidor.|
|/s|Ejecuta **regsvr32** sin mostrar mensajes.|
|/n|Ejecuta **regsvr32** sin llamar a **DllRegisterServer**. (Requiere el parámetro **/i** ).|
|/i: @no__t 0cmdline >|Pasa una cadena de línea de comandos (*cmdline*) opcional a **DllInstall**. Si usa este parámetro junto con el parámetro **/u** , llama a **DllUninstall**.|
|@no__t 0DllName >|Nombre del archivo. dll que se va a registrar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Example

Para registrar el archivo. dll para el esquema de Active Directory, escriba:
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)