---
title: regsvr32
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 87d9291755ddb4484e85248cb01ad78b01a25965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889996"
---
# <a name="regsvr32"></a>regsvr32



Registra los archivos .dll como componentes de comando en el registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/u|Anula el registro de servidor.|
|/s|Ejecuciones **Regsvr32** sin mostrar mensajes.|
|/n|Ejecuciones **Regsvr32** sin llamar a **DllRegisterServer**. (Requiere el **/i** parámetro.)|
|/i:\<cmdline>|Pasa una cadena de línea de comandos opcional (*cmdline*) a **DllInstall**. Si usa este parámetro junto con el **/u** parámetro, llama a **DllUninstall**.|
|\<DllName>|El nombre del archivo .dll que se va a registrar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

Para registrar el archivo .dll para el esquema de Active Directory, escriba:
```
regsvr32 schmmgmt.dll
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)