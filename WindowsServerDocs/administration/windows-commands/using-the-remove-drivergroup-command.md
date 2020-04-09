---
title: Remove-DriverGroup
description: Windows Commands topic for Remove-DriverGroup, que quita un grupo de controladores de un servidor.
ms.prod: windows-server
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56622c30b8b0af88a57c476eb4f03d598703d603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830528"
---
# <a name="remove-drivergroup"></a>Remove-DriverGroup

Quita un grupo de controladores de un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/DriverGroup: nombre del grupo de\<>|Especifica el nombre del grupo de controladores que se va a quitar.|
|[/Server:\<nombre de servidor >]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para quitar un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)