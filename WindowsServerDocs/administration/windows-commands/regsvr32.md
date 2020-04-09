---
title: regsvr32
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b775b0c49e4191a9fee6dc9e2e91f968142085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836218"
---
# <a name="regsvr32"></a>regsvr32



Registra los archivos. dll como componentes de comando en el registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/u|Anula el registro del servidor.|
|/s|Ejecuta **regsvr32** sin mostrar mensajes.|
|/n|Ejecuta **regsvr32** sin llamar a **DllRegisterServer**. (Requiere el parámetro **/i** ).|
|/i:\<cmdline >|Pasa una cadena de línea de comandos (*cmdline*) opcional a **DllInstall**. Si usa este parámetro junto con el parámetro **/u** , llama a **DllUninstall**.|
|\<DllName >|Nombre del archivo. dll que se va a registrar.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para registrar el archivo. dll para el esquema de Active Directory, escriba:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)