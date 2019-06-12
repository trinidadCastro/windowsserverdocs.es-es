---
title: nlbmgr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 757b218ad3a88cc10c4d1bcfed15a83bfd34cc74
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437035"
---
# <a name="nlbmgr"></a>nlbmgr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mediante el Administrador de equilibrio de carga de red, puede configurar y administrar los clústeres de equilibrio de carga de red y todos los hosts del clúster desde un único equipo, y también puede replicar la configuración del clúster a otros hosts. Puede iniciar el Administrador de equilibrio de carga de red desde la línea de comandos mediante el comando **nlbmgr.exe**, que se instala en el **systemroot\System32** carpeta.
## <a name="syntax"></a>Sintaxis
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                                Descripción                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                    |
|         / noping         | Impide que el Administrador de equilibrio de carga de red hacer ping a los hosts antes de intentar ponerse en contacto con ellos a través de Windows Management Instrumentation (WMI). Use esta opción si ha deshabilitado el protocolo de mensajes de Control de Internet (ICMP) en todos los adaptadores de red disponible. Si el Administrador de equilibrio de carga de red intenta ponerse en contacto con un host que no está disponible, experimentarán una demora al utilizar esta opción. |
|  /hostlist <filename>   |                                                                                                                                                                Carga los hosts especificados en el nombre de archivo en el Administrador de equilibrio de carga de red.                                                                                                                                                                 |
| /autorefresh <interval> |                                                                                                          Hace que el Administrador de equilibrio de carga de red actualizar su información de host y clúster cada <interval> segundos. Si no se especifica ningún intervalo, la información se actualiza cada 60 segundos.                                                                                                          |
|           /?            |                                                                                                                                                                                   Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                    |

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

