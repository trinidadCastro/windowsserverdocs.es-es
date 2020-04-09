---
title: Subcomando Start-namespace
description: Tema de comandos de Windows para el subcomando Start-namespace, que inicia un espacio de nombres de difusión programada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a30220f78d5ae865095149fc7170a6ce9b8fb1b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833768"
---
# <a name="subcommand-start-namespace"></a>Subcomando: Start-namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia un espacio de nombres de difusión programada.

## <a name="syntax"></a>Sintaxis
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros

|          Parámetro          |                                                                                                                                                                                             Descripción                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace: < nombre de espacio de nombres| Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-   **servidor de implementación**: la sintaxis del nombre de espacio de nombres es/NAMSPACE: WDS:<Image group>/<Image name>/<Index>. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **servidor de transporte**: este nombre debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
|   [/Server:<Server name>]   |                                                                                                           Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                           |

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para iniciar un espacio de nombres, escriba uno de los siguientes:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usar el comando New-namespace](using-the-new-namespace-command.md)
[con el comando Remove-namespace](using-the-remove-namespace-command.md)
