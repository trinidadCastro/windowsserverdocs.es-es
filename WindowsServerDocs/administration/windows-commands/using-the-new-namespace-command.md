---
title: Con el comando nuevo Namespace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871156"
---
# <a name="using-the-new-namespace-command"></a>Con el comando nuevo Namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea y configura un nuevo espacio de nombres. Debe usar esta opción cuando tenga solo el servicio de rol de servidor de transporte instalado. Si tiene instalan (que es el valor predeterminado) el servicio de rol de servidor de implementación y el servicio de rol de servidor de transporte, use [con el comando nueva-MulticastTransmission](using-the-new-multicasttransmission-command.md). Tenga en cuenta que debe registrar el proveedor de contenido antes de usar esta opción.
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
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|/FriendlyName:<Friendly name>|Especifica el nombre descriptivo del espacio de nombres.|
|[/ Descripción:<Description>]|Establece la descripción del espacio de nombres.|
|/ Namespace:<Namespace name>|Especifica el nombre del espacio de nombres. Tenga en cuenta que esto no es el nombre descriptivo y debe ser único.<br /><br />-   **Servicio de rol de servidor de implementación**: La sintaxis de esta opción es /Namespace:WDS:<Image group>/<Image name>/<Index>. Por ejemplo: **WDS:ImageGroup1/install.wim/1**<br />-   **Servicio de rol servidor de transporte**: Este valor debe coincidir con el nombre que especificó cuando creó el espacio de nombres en el servidor.|
|/ ContentProvider:<Name>]|Especifica el nombre del proveedor de contenido que proporcionará el contenido para el espacio de nombres.|
|[/ConfigString:<Configuration string>]|Especifica la cadena de configuración para el proveedor de contenido.|
|/ Namespacetype: {AutoCast &#124; ScheduledCast}|Especifica la configuración para la transmisión. Especifique la configuración mediante las siguientes opciones:<br /><br />-[/ time: <time>]-establece el tiempo que se debe iniciar la transmisión mediante el formato siguiente: AAAA/MM/número_clientes. Esta opción solo se aplica a las transmisiones de difusión programada.<br />-[/ Clientes: <Number of clients>]-establece el número mínimo de clientes que se esperan antes de inicia la transmisión. Esta opción solo se aplica a las transmisiones de difusión programada.|
## <a name="BKMK_examples"></a>Ejemplos
Para crear un espacio de nombres de difusión automática, escriba:
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
[con el comando remove-Namespace](using-the-remove-namespace-command.md) 
 [ Subcomando: start-Namespace](subcommand-start-namespace.md)
