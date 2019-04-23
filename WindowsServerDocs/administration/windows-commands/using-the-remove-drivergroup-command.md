---
title: Con el comando remove-DriverGroup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56f162861caf4493550f9e063065e9544e52eae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885756"
---
# <a name="using-the-remove-drivergroup-command"></a>Con el comando remove-DriverGroup



Quita un grupo de controladores de un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ DriverGroup:\<nombre de grupo >|Especifica el nombre del grupo para quitar controlador.|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|

## <a name="BKMK_examples"></a>Ejemplos

Para quitar un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)