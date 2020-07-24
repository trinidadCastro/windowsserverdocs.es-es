---
title: nlbmgr
description: Artículo de referencia del comando nlbmgr, que le ayuda a configurar y administrar los clústeres de equilibrio de carga de red y todos los hosts del clúster desde un solo equipo, mediante el administrador de equilibrio de carga de red.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19aca0285eca202b4e43a15b8e880f3672f0c846
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956717"
---
# <a name="nlbmgr"></a>nlbmgr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configure y administre los clústeres de equilibrio de carga de red y todos los hosts del clúster desde un solo equipo, mediante el administrador de equilibrio de carga de red. También puede usar este comando para replicar la configuración del clúster en otros hosts.

Puede iniciar el administrador de equilibrio de carga de red desde la línea de comandos mediante el comando **nlbmgr.exe**, que se instala en la carpeta **systemroot\System32**

## <a name="syntax"></a>Sintaxis

```
nlbmgr [/noping][/hostlist <filename>][/autorefresh <interval>][/help | /?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /noping | Impide que el administrador de equilibrio de carga de red haga ping a los hosts antes de intentar ponerse en contacto con ellos a través de Instrumental de administración de Windows (WMI). Utilice esta opción si ha deshabilitado el protocolo de mensajes de control de Internet (ICMP) en todos los adaptadores de red disponibles. Si el administrador de equilibrio de carga de red intenta ponerse en contacto con un host que no está disponible, experimentará un retraso al usar esta opción. |
| /hostlist`<filename>` | Carga los hosts especificados en el nombre de archivo en el administrador de equilibrio de carga de red. |
| /autorefresh`<interval>` | Hace que el administrador de equilibrio de carga de red actualice su host y la información del clúster cada `<interval>` segundos. Si no se especifica ningún intervalo, la información se actualiza cada 60 segundos. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de cmdlets de NetworkLoadBalancingClusters](/powershell/module/networkloadbalancingclusters)
