---
title: nlbmgr
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2843e303b296beca24132b62073b6776a343544b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373157"
---
# <a name="nlbmgr"></a>nlbmgr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mediante el administrador de equilibrio de carga de red, puede configurar y administrar los clústeres de equilibrio de carga de red y todos los hosts del clúster desde un solo equipo, y también puede replicar la configuración de clúster en otros hosts. Puede iniciar el administrador de equilibrio de carga de red desde la línea de comandos mediante el comando **nlbmgr. exe**, que se instala en la carpeta **systemroot\System32**
## <a name="syntax"></a>Sintaxis
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                                Descripción                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                    |
|         /noping         | Impide que el administrador de equilibrio de carga de red haga ping a los hosts antes de intentar ponerse en contacto con ellos a través de Instrumental de administración de Windows (WMI). Utilice esta opción si ha deshabilitado el protocolo de mensajes de control de Internet (ICMP) en todos los adaptadores de red disponibles. Si el administrador de equilibrio de carga de red intenta ponerse en contacto con un host que no está disponible, se producirá un retraso al usar esta opción. |
|  /hostlist <filename>   |                                                                                                                                                                Carga los hosts especificados en el nombre de archivo en el administrador de equilibrio de carga de red.                                                                                                                                                                 |
| /autorefresh <interval> |                                                                                                          Hace que el administrador de equilibrio de carga de red actualice su host y la información del clúster cada <interval> segundos. Si no se especifica ningún intervalo, la información se actualiza cada 60 segundos.                                                                                                          |
|           /?            |                                                                                                                                                                                   Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                    |

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

