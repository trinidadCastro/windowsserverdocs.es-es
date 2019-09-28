---
title: bitsadmin setproxysettings
description: 'Temas de comandos de Windows para **bitsadmin setproxysettings** : establece la configuración de proxy para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38987c88bcfc93ea9251583b7914f982cf79e057
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380470"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



Establece la configuración de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Uso|Uno de los siguientes valores:</br>-Preconfig: Use los valores predeterminados de Internet Explorer del propietario.</br>-NO_PROXY: no se usa un servidor proxy.</br>-OVERRIDE: usar una lista de proxy explícita y una lista de omisión. Debe seguir una lista de omisión de proxy y proxy.</br>-DETECCIÓN automática: detecta automáticamente la configuración de proxy.|
|Lista|Se usa cuando el parámetro de *uso* está establecido en invalidar: contiene una lista delimitada por comas de servidores proxy que se van a utilizar.|
|Alto|Se usa cuando el parámetro *Usage* se establece en override (contiene una lista delimitada por espacios de nombres de host o direcciones IP, o ambos) para la que las transferencias no se enrutarán a través de un proxy. Esto puede ser **@no__t 1local >** para hacer referencia a todos los servidores de la misma LAN. Los valores NULL o "" se pueden usar para una lista de omisión de proxy vacía.|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece la configuración de proxy para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

A continuación se muestran otros ejemplos.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)