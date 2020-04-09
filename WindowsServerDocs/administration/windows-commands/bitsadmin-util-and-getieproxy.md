---
title: bitsadmin util y GETIEPROXY
description: Tema de comandos de Windows para bitsadmin util y GETIEPROXY, que recupera el uso de proxy para la cuenta de servicio especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d2a8634f3b42d655632a280cb9b998111c800b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848978"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util y GETIEPROXY

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera el uso de proxy para la cuenta de servicio especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /GetIEProxy <Account> [/Conn <ConnectionName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Cuenta|Especifica la cuenta de servicio cuya configuración de proxy desea recuperar. Los valores posibles son:<p>-LOCALSYSTEM<br />-NETWORKSERVICE<br />-LOCALSERVICE|
|ConnectionName|Opcional se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no especifica el parámetro **/Conn** , bits usa la conexión LAN. Especifique el nombre de la conexión del módem inmediatamente después del parámetro **/Conn** .|

## <a name="remarks"></a>Comentarios

Este modificador muestra el valor de cada uso del proxy, no solo el uso de proxy especificado para la cuenta de servicio. Para obtener más información sobre cómo establecer el uso del proxy para las cuentas de servicio, vea el modificador de la opción [bitsadmin y SETIEPROXY](bitsadmin-util-and-setieproxy.md)

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se muestra el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /Util /GetIEProxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
