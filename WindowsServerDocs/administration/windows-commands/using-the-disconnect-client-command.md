---
title: Mediante el comando de desconexión del cliente
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841756"
---
# <a name="using-the-disconnect-client-command"></a>Mediante el comando de desconexión del cliente



Desconecta a un cliente de una transmisión por multidifusión o espacio de nombres. A menos que especifique **/Force**, el cliente se revertirá a otro método de transferencia (si es compatible con el cliente).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ ClientId:\<Id. de cliente >|Especifica el identificador del cliente que se va a desconectar. Para ver el identificador de un cliente, escriba **WDSUTIL /get-multicasttransmission/show: Clients**.|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|[/Force]|Detiene la instalación por completo y no utiliza un método de reserva. Tenga en cuenta que Wdsmcast.exe no admite ningún mecanismo de reserva. Si no usa esta opción, el comportamiento predeterminado es como sigue:</br>-Si usa al cliente de servicios de implementación de Windows, el cliente continúa la instalación mediante el uso de unidifusión.</br>-Si no utiliza al cliente de servicios de implementación de Windows, se produce un error en la instalación.</br>Importante: Debe usar esta opción con precaución porque se producirá un error en la instalación y el equipo podría quedar en un estado inutilizable.|

## <a name="BKMK_examples"></a>Ejemplos

Para desconectar a un cliente, escriba:
```
WDSUTIL /Disconnect-Client /ClientId:1
```
Para desconectar a un cliente y forzar un error en la instalación, escriba:
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)