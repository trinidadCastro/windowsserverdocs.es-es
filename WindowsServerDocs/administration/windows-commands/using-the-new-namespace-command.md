---
title: Usar el comando New-namespace
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df6634bc7598701db050f3d240e41dbb6f06019
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363006"
---
# <a name="using-the-new-namespace-command"></a>Usar el comando New-namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea y configura un espacio de nombres nuevo. Debe usar esta opción si solo tiene instalado el servicio de función servidor de transporte. Si tiene instalado el servicio de rol servidor de implementación y el servicio de rol servidor de transporte (que es el valor predeterminado), use [el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md). Tenga en cuenta que debe registrar el proveedor de contenido antes de usar esta opción.
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
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/FriendlyName: <Friendly name>|Especifica el nombre descriptivo del espacio de nombres.|
|/Description<Description>]|Establece la descripción del espacio de nombres.|
|/Namespace: <Namespace name>|Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<br /><br />**servicio de rol servidor de implementación**-   : La sintaxis de esta opción es/namespace: WDS: <Image group> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />**servicio de función de servidor de transporte**-   : Este valor debe coincidir con el nombre especificado cuando se creó el espacio de nombres en el servidor.|
|/ContentProvider: <Name>]|Especifica el nombre del proveedor de contenido que proporcionará el contenido del espacio de nombres.|
|[/ConfigString: <Configuration string>]|Especifica la cadena de configuración para el proveedor de contenido.|
|/Namespacetype: {autocast &#124; ScheduledCast}|Especifica la configuración de la transmisión. La configuración se especifica mediante las siguientes opciones:<br /><br />-[/Time: <time>]: establece la hora a la que debe comenzar la transmisión con el siguiente formato: AAAA/MM/DD: HH: mm. Esta opción solo se aplica a las transmisiones de difusión programadas.<br />-[/Clients: <Number of clients>]: establece el número mínimo de clientes que hay que esperar antes de que se inicie la transmisión. Esta opción solo se aplica a las transmisiones de difusión programadas.|
## <a name="BKMK_examples"></a>Example
Para crear un espacio de nombres de conversión automática, escriba:
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Para crear un espacio de nombres de difusión programada, escriba:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[mediante el comando Remove-namespace](using-the-remove-namespace-command.md)
[subcomando: Start-namespace](subcommand-start-namespace.md)
