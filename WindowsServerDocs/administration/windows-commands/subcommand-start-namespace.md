---
title: Subcomando Start-namespace
description: Artículo de referencia para el subcomando Start-namespace, que inicia un espacio de nombres de difusión programada.
ms.topic: reference
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a0dd1b4988977d14a2be68966a6eb53df71ce8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024759"
---
# <a name="subcommand-start-namespace"></a>Subcomando: Start-namespace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicia un espacio de nombres de difusión programada.

## <a name="syntax"></a>Sintaxis
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros

|          Parámetro          |                                                                                                                                                                                             Descripción                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace: <nombre de espacio de nombres| Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-   **Servidor de implementación**: la sintaxis del nombre de espacio de nombres es/NAMSPACE: WDS: <Image group> / <Image name> / <Index> . Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **Servidor de transporte**: este nombre debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
|   [/Server:<Server name>]   |                                                                                                           Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                           |

## <a name="examples"></a>Ejemplos
Para iniciar un espacio de nombres, escriba uno de los siguientes:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-get-allnamespaces-command.md) 
 Get-AllNamespaces [Usar el comando](using-the-new-namespace-command.md) 
 New-namespace [Usar el comando Remove-namespace](using-the-remove-namespace-command.md)
