---
title: Remove-DriverGroup
description: Artículo de referencia de Remove-DriverGroup, que quita un grupo de controladores de un servidor.
ms.topic: reference
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bfec1636594f69ed9ea173cd2a0fcb95415296f
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524330"
---
# <a name="remove-drivergroup"></a>Remove-DriverGroup

Quita un grupo de controladores de un servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/DriverGroup:\<Group Name>|Especifica el nombre del grupo de controladores que se va a quitar.|
|[/Server:\<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|

## <a name="examples"></a>Ejemplos

Para quitar un grupo de controladores, escriba uno de los siguientes:
```
wdsutil /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
wdsutil /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)