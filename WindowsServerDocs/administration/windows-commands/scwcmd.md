---
title: Scwcmd
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fae9476f94af5faa6e942239e7d91cf589bb1776
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384260"
---
# <a name="scwcmd"></a>Scwcmd

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

La herramienta de línea de comandos scwcmd. exe incluida en el Asistente para configuración de seguridad (SCW) se puede usar para realizar las tareas siguientes:
-   Configure uno o varios servidores con una directiva generada por SCW.
-   Analizar uno o varios servidores con una directiva generada por SCW.
-   Ver resultados de análisis en formato HTML.
-   Revertir directivas de SCW.
-   Transforme una directiva generada por SCW en archivos nativos compatibles con directiva de grupo.
-   Registra una extensión de base de datos de configuración de seguridad con SCW.

Al usar **scwcmd** para configurar, analizar o revertir una directiva en un servidor remoto, SCW debe estar instalado en el servidor remoto.

## <a name="syntax"></a>Sintaxis

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Parámetros

|Subcomando|Descripción|
|----------|-----------|
|/ANALYZE|Determina si un equipo cumple con una directiva.</br>Vea [scwcmd: Analyze](scwcmd-analyze.md) para ver la sintaxis y las opciones.|
|/Configure|Aplica una directiva de seguridad generada por SCW a un equipo.</br>Vea [scwcmd: configure](scwcmd-configure.md) para ver la sintaxis y las opciones.|
|/Register|Extiende o Personaliza la base de datos de configuración de seguridad de SCW registrando un archivo de base de datos de configuración de seguridad que contiene definiciones de roles, tareas, servicios o puertos.</br>Vea [scwcmd: Register](scwcmd-register.md) para ver la sintaxis y las opciones.|
|/rollback|Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión.</br>Vea [scwcmd: Rollback](scwcmd-rollback.md) para ver la sintaxis y las opciones.|
|opción/Transform|Transforma un archivo de directiva de seguridad generado mediante el uso de SCW en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services.</br>Vea [scwcmd: transform](scwcmd-transform.md) Syntax y Options.|
|/View|Representa un archivo. XML mediante una transformación. xsl especificada.</br>Vea [scwcmd: View para ver](scwcmd-view.md) la sintaxis y las opciones.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
