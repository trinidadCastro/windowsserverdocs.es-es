---
title: Dfsutil
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a06806b109bbd324213f935892bbbab415362df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377981"
---
# <a name="dfsutil"></a>Dfsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando Dfsutil administra espacios de nombres DFS, servidores y clientes. los comandos Dfsutil usan la terminología de Sistema de archivos distribuido original, con la terminología de espacios de nombres DFS actualizada que se proporciona como explicación para la mayoría de los comandos.

para obtener ejemplos de cómo se puede usar este comando, vea. 

## <a name="syntax"></a>Sintaxis

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|[Raíz Dfsutil](dfsutil-root.md)|Muestra, crea, quita, importa y exporta raíces de espacio de nombres.|
|[Vínculo Dfsutil](dfsutil-link.md)|Muestra, crea, quita o mueve carpetas \(vínculos\).|
|[Destino Dfsutil](dfsutil-target.md)|Muestra, crea, quita el destino de carpeta o el servidor de espacio de nombres.|
|[Dfsutil (propiedad)](dfsutil-property.md)|Muestra o modifica un destino de carpeta o un servidor de espacio de nombres.|
|[Cliente Dfsutil](dfsutil-client.md)|Muestra o modifica la información del cliente o las claves del registro.|
|[Servidor Dfsutil](dfsutil-server.md)|Muestra o modifica la configuración del espacio de nombres.|
|[Dfsutil Diag](dfsutil-diag.md)|Realizar diagnósticos o ver dfsdirs\/dfspath.|
|[Dominio Dfsutil](dfsutil-domain.md)|Muestra todos los espacios de nombres basados en\-de dominio en un dominio.|
|[Caché Dfsutil](dfsutil-cache.md)|Muestra o vacía la memoria caché del cliente.|
|[Dfsutil oldcli](dfsutil-oldcli.md)|Use el comando Dfsutil \/oldcli para usar la sintaxis de Dfsutil original.|

## <a name="remarks-optional-section"></a>Comentarios <optional section>
Si especifica un objeto \(como un servidor de espacio de nombres\) al final de un comando, la mayoría de los comandos mostrarán información sobre el objeto sin necesidad de seguir parámetros ni comandos. Por ejemplo, al usar el comando Dfsutil root, puede anexar una raíz de espacio de nombres al comando para ver información sobre la raíz.

## <a name="BKMK_Examples"></a>Example
&lt;aquí es donde se coloca una descripción detallada del ejemplo.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;aquí es donde se coloca una descripción detallada de otro ejemplo.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


