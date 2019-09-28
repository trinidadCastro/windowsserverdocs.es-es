---
title: Usar el comando Disconnect-Client
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbb96d64b47ec72ff0710bfb3684257c1bda2d04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363473"
---
# <a name="using-the-disconnect-client-command"></a>Usar el comando Disconnect-Client



Desconecta un cliente de un espacio de nombres o transmisión de multidifusión. A menos que especifique **/Force**, el cliente revertirá a otro método de transferencia (si el cliente lo admite).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ClientId: \<Client ID >|Especifica el identificador del cliente que se va a desconectar. Para ver el identificador de un cliente, escriba **WDSUtil/Get-multicasttransmission/show: clients**.|
|[/Server: \<Server nombre >]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Force.|Detiene la instalación por completo y no utiliza un método de reserva. Tenga en cuenta que Wdsmcast. exe no admite ningún mecanismo de reserva. Si no usa esta opción, el comportamiento predeterminado es el siguiente:</br>-Si usa el cliente de servicios de implementación de Windows, el cliente continúa la instalación mediante unidifusión.</br>-Si no usa el cliente de servicios de implementación de Windows, se produce un error en la instalación.</br>Importante: Debe usar esta opción con precaución, ya que se producirá un error en la instalación y el equipo podría quedar en un estado inutilizable.|

## <a name="BKMK_examples"></a>Example

Para desconectar un cliente, escriba:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Para desconectar un cliente y forzar que se produzca un error en la instalación, escriba:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)