---
title: 'ksetup: servidor'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91549eb78f825264016ec0e03b7035f79132f260
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724591"
---
# <a name="ksetupserver"></a>ksetup: servidor



Permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows para que los cambios realizados mediante **ksetup** actualicen el equipo de destino.

## <a name="syntax"></a>Sintaxis

```
ksetup /server <ServerName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDeServidor>|Nombre completo del equipo en el que la configuración será efectiva, como IPops897.corp.contoso.com.</br>Si se especifica un nombre completo de equipo de dominio completo, se producirá un error en el comando.|

## <a name="remarks"></a>Observaciones

No hay ninguna manera de quitar el nombre del servidor de destino; solo puede volver a cambiarlo al nombre del equipo local, que es el valor predeterminado.

El nombre del servidor de destino se almacena en el registro en **HKEY_LOCAL_MACHINE \system\controlset001\control\lsa\kerberos**. No se registra mediante **ksetup**.

## <a name="examples"></a>Ejemplos

Haga que las configuraciones de **ksetup** sean eficaces en el equipo IPops897 que está conectado en el dominio contoso:
```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)