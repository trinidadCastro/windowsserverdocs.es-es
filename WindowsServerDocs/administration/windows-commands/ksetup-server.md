---
title: 'ksetup: servidor'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd05fd294640c63e633b7b866307197ae6770476
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374958"
---
# <a name="ksetupserver"></a>ksetup: servidor



Permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows para que los cambios realizados mediante **ksetup** actualicen el equipo de destino. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName >|Nombre completo del equipo en el que la configuración será efectiva, como IPops897.corp.contoso.com.</br>Si se especifica un nombre completo de equipo de dominio completo, se producirá un error en el comando.|

## <a name="remarks"></a>Observaciones

No hay ninguna manera de quitar el nombre del servidor de destino; solo puede volver a cambiarlo al nombre del equipo local, que es el valor predeterminado.

El nombre del servidor de destino se almacena en el registro en **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. No se registra mediante **ksetup**.

## <a name="BKMK_Examples"></a>Example

Haga que las configuraciones de **ksetup** sean eficaces en el equipo IPops897 que está conectado en el dominio contoso:
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)