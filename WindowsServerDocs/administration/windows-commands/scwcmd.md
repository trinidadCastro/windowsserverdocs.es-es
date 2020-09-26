---
title: scwcmd
description: Artículo de referencia para la herramienta de línea de comandos de scwcmd.exe incluida en el Asistente para configuración de seguridad (SCW).
ms.topic: reference
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 87bd2c718bec18810026c5701b955d5ef655b53e
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388925"
---
# <a name="scwcmd"></a>scwcmd

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

La herramienta de línea de comandos Scwcmd.exe incluida en el Asistente para configuración de seguridad (SCW) se puede usar para realizar las tareas siguientes:

- Analizar uno o varios servidores con una directiva generada por SCW.

- Configure uno o varios servidores con una directiva generada por SCW.

- Registra una extensión de base de datos de configuración de seguridad con SCW.

- Revertir directivas de SCW.

- Transforme una directiva generada por SCW en archivos nativos compatibles con directiva de grupo.

- Ver resultados de análisis en formato HTML.

> [!NOTE]
> Si usa **scwcmd** para configurar, analizar o revertir una directiva en un servidor remoto, SCW debe estar instalado en el servidor remoto.

## <a name="syntax"></a>Sintaxis

```
scwcmd analyze
scwcmd configure
scwcmd register
scwcmd rollback
scwcmd transform
scwcmd view
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [scwcmd analyze](scwcmd-analyze.md) | Determina si un equipo cumple con una directiva. |
| [scwcmd configure](scwcmd-configure.md) | Aplica una directiva de seguridad generada por SCW a un equipo.|
| [scwcmd register](scwcmd-register.md) | Extiende o Personaliza la base de datos de configuración de seguridad de SCW registrando un archivo de base de datos de configuración de seguridad que contiene definiciones de roles, tareas, servicios o puertos. |
| [scwcmd rollback](scwcmd-rollback.md) | Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión. |
| [scwcmd transform](scwcmd-transform.md) | Transforma un archivo de directiva de seguridad generado mediante el uso de SCW en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services. |
| [scwcmd view](scwcmd-view.md) | Representa un archivo. XML mediante una transformación. xsl especificada. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
