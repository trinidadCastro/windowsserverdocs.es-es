---
title: GETIEPROXY y bitsadmin util
description: Tema de los comandos de Windows para **bitsadmin util y getieproxy** -recupera el uso de proxy para la cuenta de servicio determinado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de4f86340b1163c4d8e3286d9c86c9df794a21c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876876"
---
# <a name="bitsadmin-util-and-getieproxy"></a>GETIEPROXY y bitsadmin util

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera el uso de proxy para la cuenta de servicio determinado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Cuenta|Especifica la cuenta de servicio cuya configuración de proxy que se va a recuperar. Los valores posibles son:<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional se utiliza con el **/Conn** parámetro para especificar la conexión del módem para usar. Si no especifica la **/Conn** parámetro, BITS usa la conexión LAN. Especifique el nombre de conexión de módem inmediatamente después de la **/Conn** parámetro.|

## <a name="remarks"></a>Comentarios

Este modificador muestra el valor de cada uso de proxy, no solo el uso de proxy que especificó para la cuenta de servicio. Para obtener más información sobre cómo establecer el uso de proxy para las cuentas de servicio, consulte el [bitsadmin util y setieproxy](bitsadmin-util-and-setieproxy.md) cambie.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente muestra el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
