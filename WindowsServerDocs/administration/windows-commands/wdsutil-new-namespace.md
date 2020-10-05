---
title: WDSUtil-nuevo-espacio de nombres
description: Artículo de referencia para WDSUtil New-namespace, que crea y configura un espacio de nombres nuevo.
ms.topic: reference
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67f479af3644cad5c587a4c6eb17dbf43e1fbe0c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730702"
---
# <a name="wdsutil-new-namespace"></a>WDSUtil-nuevo-espacio de nombres

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea y configura un espacio de nombres nuevo. Debe usar esta opción si solo tiene instalado el servicio de función servidor de transporte. Si tiene instalado el servicio de rol servidor de implementación y el servicio de rol servidor de transporte (que es el valor predeterminado), use el [comando wdsutilnew-MulticastTransmission](wdsutil-new-multicasttransmission.md). Tenga en cuenta que debe registrar el proveedor de contenido antes de usar esta opción.
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
|FriendlyName<Friendly name>|Especifica el nombre descriptivo del espacio de nombres.|
|/Description<Description>]|Establece la descripción del espacio de nombres.|
|System.IO<Namespace name>|Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-   **Servicio de rol servidor de implementación**: la sintaxis de esta opción es/namespace: WDS: <Image group> / <Image name> / <Index> . Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **Servicio de función de servidor de transporte**: este valor debe coincidir con el nombre especificado cuando se creó el espacio de nombres en el servidor.|
|/ContentProvider: <Name> ]|Especifica el nombre del proveedor de contenido que proporcionará el contenido del espacio de nombres.|
|[/ConfigString: <Configuration string> ]|Especifica la cadena de configuración del proveedor de contenido.|
|/Namespacetype: {autocast &#124; ScheduledCast}|Especifica la configuración de la transmisión. La configuración se especifica mediante las siguientes opciones:<p>-[/Time: <time> ]-establece el tiempo que la transmisión debe comenzar con el siguiente formato: AAAA/MM/DD: HH: mm. Esta opción solo se aplica a las transmisiones de difusión programadas.<br />-[/Clients: <Number of clients> ]: establece el número mínimo de clientes que hay que esperar antes de que se inicie la transmisión. Esta opción solo se aplica a las transmisiones de difusión programadas.|
## <a name="examples"></a>Ejemplos
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
- [WDSUtil Get-allnamespaces (comando)](wdsutil-get-allnamespaces.md)
- [comando WDSUtil Remove-namespace](wdsutil-remove-namespace.md)
- [comando WDSUtil Start-namespace](wdsutil-start-namespace.md)
