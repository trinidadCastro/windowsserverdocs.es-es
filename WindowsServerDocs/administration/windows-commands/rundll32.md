---
title: rundll32
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835036"
---
# <a name="rundll32"></a>rundll32



Carga y ejecuta las bibliotecas de vínculos dinámicos (DLL) de 32 bits. No hay ningún valor configurable para Rundll32. Se proporciona información de ayuda para un archivo DLL específico que se ejecuta con la **rundll32** comando.

Debe ejecutar el **rundll32** ejecutado desde un símbolo del sistema con privilegios elevados. Para abrir un símbolo del sistema con privilegios elevados, haga clic en **iniciar**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Comandos

|Parámetro|Descripción|
|---------|-----------|
|[Rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|Muestra la interfaz de usuario de impresora|

## <a name="remarks"></a>Comentarios

Rundll32 sólo puede llamar a funciones desde un archivo DLL que se escriben explícitamente para ser llamado por Rundll32. Para obtener más información sobre Rundll32 requisitos, consulte [artículo 164787](https://go.microsoft.com/fwlink/?LinkID=165773) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)