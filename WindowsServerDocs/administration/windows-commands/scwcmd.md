---
title: Scwcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19d631f97c194a78819491f32955e391d3be5a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883886"
---
# <a name="scwcmd"></a>Scwcmd

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

La herramienta de línea de comandos de Scwcmd.exe incluida con el Asistente para configuración de seguridad (SCW) puede usarse para realizar las siguientes tareas:
-   Configure uno o varios servidores con una directiva de SCW generado.
-   Analizar uno o varios servidores con una directiva de SCW generado.
-   Ver los resultados del análisis en formato HTML.
-   Revertir las directivas de SCW.
-   Transformar una directiva de SCW generado en archivos nativos que son compatibles con la directiva de grupo.
-   Registrar una extensión de la base de datos de configuración de seguridad con SCW.

Cuando usas **scwcmd** para configurar, analizar o revertir una directiva en un servidor remoto, SCW debe estar instalada en el servidor remoto.

## <a name="syntax"></a>Sintaxis

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Parámetros

|Subcomando|Descripción|
|----------|-----------|
|/ analyze|Determina si un equipo es conforme a una directiva.</br>Consulte [Scwcmd: analizar](scwcmd-analyze.md) sintaxis y opciones.|
|/configure|Se aplica una directiva de seguridad generados por SCW a un equipo.</br>Consulte [Scwcmd: configurar](scwcmd-configure.md) sintaxis y opciones.|
|/register|Amplía o personaliza la base de datos de configuración de seguridad de SCW mediante el registro de un archivo de base de datos de configuración de seguridad que contiene la función, tarea, servicio o las definiciones de puerto.</br>Consulte [Scwcmd: registrar](scwcmd-register.md) sintaxis y opciones.|
|/Rollback|Se aplica la directiva de deshacer más reciente disponible y, a continuación, elimina dicha directiva.</br>Consulte [Scwcmd: reversión](scwcmd-rollback.md) sintaxis y opciones.|
|/ Transform|Transforma un archivo de directiva de seguridad generado mediante el uso de SCW en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services.</br>Consulte [Scwcmd: transformar](scwcmd-transform.md) sintaxis y las opciones.|
|/View|Representa un archivo .xml mediante una transformación XSL especificado.</br>Consulte [Scwcmd: vista](scwcmd-view.md) sintaxis y opciones.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
