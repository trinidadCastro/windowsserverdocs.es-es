---
title: 'ksetup: servidor'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7889e1a03d3c0eec1958bf1d6356c67e9371a80f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841448"
---
# <a name="ksetupserver"></a>ksetup: servidor



Permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows para que los cambios realizados mediante **ksetup** actualicen el equipo de destino. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName >|Nombre completo del equipo en el que la configuración será efectiva, como IPops897.corp.contoso.com.</br>Si se especifica un nombre completo de equipo de dominio completo, se producirá un error en el comando.|

## <a name="remarks"></a>Comentarios

No hay ninguna manera de quitar el nombre del servidor de destino; solo puede volver a cambiarlo al nombre del equipo local, que es el valor predeterminado.

El nombre del servidor de destino se almacena en el registro en **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. No se registra mediante **ksetup**.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Haga que las configuraciones de **ksetup** sean eficaces en el equipo IPops897 que está conectado en el dominio contoso:
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)