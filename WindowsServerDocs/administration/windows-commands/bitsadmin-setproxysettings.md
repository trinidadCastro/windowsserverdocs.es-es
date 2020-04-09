---
title: bitsadmin setproxysettings
description: Windows Commands topic for bitsadmin setproxysettings, que establece la configuración de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dea72d956d12070b2638f953a7a00dcb1ed7a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849208"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Establece la configuración de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Uso|Uno de los siguientes valores:</br>-Preconfig: Use los valores predeterminados de Internet Explorer del propietario.</br>-NO_PROXY: no use un servidor proxy.</br>-OVERRIDE: usar una lista de proxy explícita y una lista de omisión. Debe seguir una lista de omisión de proxy y proxy.</br>-DETECCIÓN automática: detecta automáticamente la configuración de proxy.|
|Lista|Se usa cuando el parámetro de *uso* está establecido en invalidar: contiene una lista delimitada por comas de servidores proxy que se van a utilizar.|
|Alto|Se usa cuando el parámetro *Usage* se establece en override (contiene una lista delimitada por espacios de nombres de host o direcciones IP, o ambos) para la que las transferencias no se enrutarán a través de un proxy. Se puede **\<> local** para hacer referencia a todos los servidores de la misma LAN. Los valores NULL o se pueden usar para una lista de omisión de proxy vacía.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece la configuración de proxy para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

A continuación se muestran otros ejemplos.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)