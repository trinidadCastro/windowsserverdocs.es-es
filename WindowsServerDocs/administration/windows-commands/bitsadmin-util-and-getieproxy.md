---
title: bitsadmin util y GETIEPROXY
description: 'Tema de comandos de Windows para **bitsadmin util y GETIEPROXY** : recupera el uso de proxy para la cuenta de servicio especificada.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6936e088ddcf467b5a8f931bc8217ba9da4662c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380272"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util y GETIEPROXY

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera el uso de proxy para la cuenta de servicio especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Cuenta|Especifica la cuenta de servicio cuya configuración de proxy desea recuperar. Los valores posibles son:<br /><br />-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no especifica el parámetro **/Conn** , bits usa la conexión LAN. Especifique el nombre de la conexión del módem inmediatamente después del parámetro **/Conn** .|

## <a name="remarks"></a>Comentarios

Este modificador muestra el valor de cada uso del proxy, no solo el uso de proxy especificado para la cuenta de servicio. Para obtener más información sobre cómo establecer el uso del proxy para las cuentas de servicio, vea el modificador de la opción [bitsadmin y SETIEPROXY](bitsadmin-util-and-setieproxy.md)

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se muestra el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
