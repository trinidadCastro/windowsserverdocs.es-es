---
title: New-namespace
description: Tema de comandos de Windows para New-namespace, que crea y configura un espacio de nombres nuevo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f616c33525d033827d52925e07764761e7c7b44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830638"
---
# <a name="new-namespace"></a>New-namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea y configura un espacio de nombres nuevo. Debe usar esta opción si solo tiene instalado el servicio de función servidor de transporte. Si tiene instalado el servicio de rol servidor de implementación y el servicio de rol servidor de transporte (que es el valor predeterminado), use [el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md). Tenga en cuenta que debe registrar el proveedor de contenido antes de usar esta opción.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/FriendlyName:<Friendly name>|Especifica el nombre descriptivo del espacio de nombres.|
|/Description<Description>]|Establece la descripción del espacio de nombres.|
|/Namespace:<Namespace name>|Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-   **servicio de rol servidor de implementación**: la sintaxis de esta opción es/namespace: WDS:<Image group>/<Image name>/<Index>. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **servicio de función de servidor de transporte**: este valor debe coincidir con el nombre especificado cuando se creó el espacio de nombres en el servidor.|
|/ContentProvider:<Name>]|Especifica el nombre del proveedor de contenido que proporcionará el contenido del espacio de nombres.|
|[/ConfigString:<Configuration string>]|Especifica la cadena de configuración del proveedor de contenido.|
|/Namespacetype: {autocast &#124; ScheduledCast}|Especifica la configuración de la transmisión. La configuración se especifica mediante las siguientes opciones:<p>-[/Time: <time>]: establece el tiempo que la transmisión debe comenzar con el siguiente formato: AAAA/MM/DD: HH: mm. Esta opción solo se aplica a las transmisiones de difusión programadas.<br />-[/Clients: <Number of clients>]: establece el número mínimo de clientes que hay que esperar antes de que se inicie la transmisión. Esta opción solo se aplica a las transmisiones de difusión programadas.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para crear un espacio de nombres de conversión automática, escriba:
```
wdsutil /New-Namespace /FriendlyName:Custom AutoCast Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Para crear un espacio de nombres de difusión programada, escriba:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:Custom Scheduled Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:2006/11/20:17:00 /Clients:20
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usar el comando Remove-namespace](using-the-remove-namespace-command.md)
[subcomando: Start-namespace](subcommand-start-namespace.md)
