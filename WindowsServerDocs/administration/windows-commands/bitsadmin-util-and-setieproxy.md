---
title: bitsadmin util y SETIEPROXY
description: Tema de comandos de Windows para bitsadmin util y SETIEPROXY, que establece la configuración de proxy que se va a usar al transferir archivos mediante una cuenta de servicio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e7d8a9ff4e2388b61ee5ae00ae7afe421de68e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848888"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util y SETIEPROXY

Establezca la configuración de proxy que se va a usar al transferir archivos mediante una cuenta de servicio.

**BITSAdmin 1,5 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Cuenta|Especifica el tipo de cuenta de servicio cuya configuración de proxy desea definir. Los valores posibles son:</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Uso|Especifica la forma de detección del proxy que se va a usar. Los valores posibles son:</br>-NO_PROXY: no use un servidor proxy.</br>-DETECCIÓN automática: detecta automáticamente la configuración de proxy.</br>-MANUAL_PROXY: usar una lista de proxy explícita y una lista de omisión. Especifique la lista de proxy y la lista de omisión inmediatamente después de la etiqueta Usage. Por ejemplo, MANUAL_PROXY proxy1, Proxy2 NULL.</br>    -La lista de proxy es una lista delimitada por comas de servidores proxy que se usarán.</br>    -La lista de omisión es una lista delimitada por espacios de nombres de host o direcciones IP, o ambos, para las que las transferencias no se van a enrutar a través de un proxy. Se puede \<> local para hacer referencia a todos los servidores de la misma LAN. Los valores NULL o se pueden usar para una lista de omisión de proxy vacía.</br>-Autoscript: igual que la detección automática, salvo que también ejecuta un script. Especifique la dirección URL del script inmediatamente después de la etiqueta Usage. Por ejemplo, autoscript http://server/proxy.js.</br>-RESET: igual que NO_PROXY, excepto en que quita las direcciones URL de proxy manuales (si se especifican) y las direcciones URL detectadas mediante la detección automática.|
|ConnectionName|Opcional: se usa con el parámetro **/Conn** para especificar la conexión del módem que se va a usar. Si no especifica el parámetro **/Conn** , bits usa la conexión LAN. Especifique el nombre de la conexión del módem inmediatamente después del parámetro **/Conn** .|

## <a name="remarks"></a>Comentarios

Cada llamada sucesiva con este modificador reemplaza el uso previamente especificado, pero no los parámetros del uso definido previamente. Por ejemplo, si especifica NO_PROXY, detección automática y MANUAL_PROXY en llamadas independientes, BITS usa el último uso proporcionado, pero mantiene los parámetros del uso definido previamente.

> [!IMPORTANT]
> Debe ejecutar este comando desde un símbolo del sistema con privilegios elevados para que se complete correctamente.

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece el uso de proxy para la cuenta de servicio de red.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

A continuación se muestran más ejemplos.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)