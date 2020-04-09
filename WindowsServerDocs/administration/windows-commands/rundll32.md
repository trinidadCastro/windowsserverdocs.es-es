---
title: rundll32
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 391607f6e744fe88a60cb556067cf2699eee25f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835438"
---
# <a name="rundll32"></a>rundll32



Carga y ejecuta las bibliotecas de vínculos dinámicos (dll) de 32 bits. No hay ninguna configuración configurable para rundll32. Se proporciona información de ayuda para un archivo DLL específico que se ejecuta con el comando **rundll32** .

Debe ejecutar el comando **rundll32** desde un símbolo del sistema con privilegios elevados. Para abrir un símbolo del sistema con privilegios elevados, haga clic en **Inicio**, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commands

|Parámetro|Descripción|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Muestra la interfaz de usuario de la impresora|

## <a name="remarks"></a>Comentarios

Rundll32 solo puede llamar a funciones de un archivo DLL escrito explícitamente para ser llamado por rundll32.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
