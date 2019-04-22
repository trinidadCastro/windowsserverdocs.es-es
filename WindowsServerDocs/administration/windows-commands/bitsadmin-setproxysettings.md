---
title: bitsadmin setproxysettings
description: Tema de los comandos de Windows para **setproxysettings bitsadmin** -establece la configuración de proxy para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825596"
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
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Uso|Uno de los siguientes valores:</br>-PRECONFIG: usar valores predeterminados de Internet Explorer del propietario.</br>-NO_PROXY: no use un servidor proxy.</br>-INVALIDAR: utilizar una lista de proxy explícito y lista de omisión. Deben seguir un servidor proxy y la lista de omisión de proxy.</br>-Detección automática: detectar automáticamente la configuración de proxy.|
|Lista|Se usa cuando el *uso* parámetro se establece en la INVALIDACIÓN, contiene una lista delimitada por comas de los servidores proxy para usar.|
|Omisión|Se usa cuando el *uso* parámetro se establece en la INVALIDACIÓN, contiene una lista delimitada por espacios de nombres de host o direcciones IP o ambos, para que las transferencias no son va a enrutar a través de un servidor proxy. Esto puede ser  **\<local >** para hacer referencia a todos los servidores en la misma LAN. Valores NULL o "" puede utilizarse para obtener una lista de omisión de proxy vacía.|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece la configuración de proxy del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Estos son algunos otros ejemplos.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)