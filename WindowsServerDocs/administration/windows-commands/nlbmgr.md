---
title: nlbmgr
description: Artículo de referencia del comando nlbmgr, que le ayuda a configurar y administrar los clústeres de equilibrio de carga de red y todos los hosts del clúster desde un solo equipo, mediante el administrador de equilibrio de carga de red.
ms.topic: reference
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e9036a5fc4a0941445be4c9e9cf11c8064caa08
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023589"
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
| /hostlist `<filename>` | Carga los hosts especificados en el nombre de archivo en el administrador de equilibrio de carga de red. |
| /autorefresh `<interval>` | Hace que el administrador de equilibrio de carga de red actualice su host y la información del clúster cada `<interval>` segundos. Si no se especifica ningún intervalo, la información se actualiza cada 60 segundos. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de cmdlets de NetworkLoadBalancingClusters](/powershell/module/networkloadbalancingclusters)
