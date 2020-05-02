---
title: regsvr32
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: beadc9e9e614e2fe4cffad5dc263cfb1d4aecf67
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722484"
---
# <a name="regsvr32"></a>regsvr32



Registra los archivos. dll como componentes de comando en el registro.



## <a name="syntax"></a>Sintaxis

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <DllName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/U|Anula el registro del servidor.|
|/s|Ejecuta **regsvr32** sin mostrar mensajes.|
|/n|Ejecuta **regsvr32** sin llamar a **DllRegisterServer**. (Requiere el parámetro **/i** ).|
|/i:\<> cmdline|Pasa una cadena de línea de comandos (*cmdline*) opcional a **DllInstall**. Si usa este parámetro junto con el parámetro **/u** , llama a **DllUninstall**.|
|\<DllName>|Nombre del archivo. dll que se va a registrar.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para registrar el archivo. dll para el esquema de Active Directory, escriba:
```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)