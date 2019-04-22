---
title: ksetup:server
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f370d4dede278e1facdda829503ea3793502b9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814576"
---
# <a name="ksetupserver"></a>ksetup:server



Le permite especificar un nombre para un equipo que ejecuta el sistema operativo de Windows para que los cambios realizaron mediante el uso de **ksetup** actualizará el equipo de destino. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /server <ServerName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ServerName>|El nombre completo del equipo en el que la configuración será efectiva, por ejemplo, IPops897.corp.contoso.com.</br>Si un incompleta completo se especifica el nombre de equipo del dominio, se producirá un error en el comando.|

## <a name="remarks"></a>Comentarios

No hay ninguna manera de quitar el nombre del servidor de destino; solo podrá cambiarla hasta que el nombre del equipo local, que es el valor predeterminado.

El nombre del servidor de destino se almacena en el registro en **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos**. No se notifica mediante el uso de **ksetup**.

## <a name="BKMK_Examples"></a>Ejemplos

Realice su **ksetup** configuraciones efectivas en el equipo de IPops897 que está conectado en el dominio Contoso:
```
ksetup /server IPops897.corp.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)