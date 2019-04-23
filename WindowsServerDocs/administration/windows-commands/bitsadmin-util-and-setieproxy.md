---
title: SETIEPROXY y bitsadmin util
description: Tema de los comandos de Windows para **bitsadmin util y setieproxy** -establecer la configuración de proxy que se usará al transferir archivos mediante una cuenta de servicio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4f6ab2e52284895d2e7918364c24bbb69f2b1c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853516"
---
# <a name="bitsadmin-util-and-setieproxy"></a>SETIEPROXY y bitsadmin util

Establecer la configuración de proxy que se usará al transferir archivos mediante una cuenta de servicio.

**BITSAdmin 1.5 y versiones anterior**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Cuenta|Especifica el tipo de cuenta de servicio cuya configuración de proxy desee definir. Los valores posibles son:</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Uso|Especifica la forma de utilizar la detección de proxy. Los valores posibles son:</br>-NO_PROXY: no use un servidor proxy.</br>-Detección automática: detectar automáticamente la configuración de proxy.</br>-MANUAL_PROXY: utilizar una lista de proxy explícito y lista de omisión. Especifique la lista de proxy e inmediatamente después de la etiqueta de uso de lista de omisión. Por ejemplo, MANUAL_PROXY proxy1, proxy2 NULL.</br>    -La lista de proxy es una lista delimitada por comas de servidores proxy para usar.</br>    -La lista de omisión es una lista delimitada por espacios de nombres de host o direcciones IP o ambos, para que las transferencias no son va a enrutar a través de un servidor proxy. Esto puede ser \<local > para hacer referencia a todos los servidores en la misma LAN. Valores NULL o "" puede utilizarse para obtener una lista de omisión de proxy vacía.</br>-AUTOSCRIPT: Igual que la detección automática, salvo que también ejecuta una secuencia de comandos. Especifique la URL de la secuencia de comandos inmediatamente después de la etiqueta de uso. Por ejemplo, AUTOSCRIPT http://server/proxy.js.</br>-REINICIO: Igual que NO_PROXY, excepto si quita las direcciones URL de proxy manual (si se especifica) y direcciones URL de detección mediante la detección automática.|
|ConnectionName|Opcional: puede usar con el **/Conn** parámetro para especificar la conexión del módem para usar. Si no especifica la **/Conn** parámetro, BITS usa la conexión LAN. Especifique el nombre de conexión de módem inmediatamente después de la **/Conn** parámetro.|

## <a name="remarks"></a>Comentarios

Cada llamada sucesiva con este modificador reemplaza el uso especificado anteriormente, pero no los parámetros del uso definido previamente. Por ejemplo, si especifica NO_PROXY, detección automática y MANUAL_PROXY en llamadas independientes, BITS usa el último uso proporcionado pero mantiene los parámetros desde el uso definido previamente.

> [!IMPORTANT]
> Debe ejecutar este comando desde un símbolo del sistema con privilegios elevados para que se complete correctamente.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Estos son otros ejemplos.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)