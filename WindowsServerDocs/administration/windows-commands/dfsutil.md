---
title: Dfsutil
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45545b4e12d31c293ead5b18b83efd50d7bc37bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439650"
---
# <a name="dfsutil"></a>Dfsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El comando dfsutil administra los espacios de nombres DFS, servidores y clientes. comandos de Dfsutil utilizan la terminología del sistema de archivos distribuido original, con la terminología actualizada de espacios de nombres DFS proporcionada como una explicación de la mayoría de los comandos.

Para obtener ejemplos de cómo se puede usar este comando, vea 

## <a name="syntax"></a>Sintaxis

```
command </parameter> </param2>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|[dfsutil Root](dfsutil-root.md)|Muestra, crea, elimina, importa, exporta las raíces de espacio de nombres.|
|[dfsutil Link](dfsutil-link.md)|Muestra, crea, elimina o mueve las carpetas \(vínculos\).|
|[dfsutil Target](dfsutil-target.md)|Muestra, crear, quitar el servidor de destino o espacio de nombres de carpeta.|
|[Dfsutil propiedad](dfsutil-property.md)|Muestra o modifica un servidor de destino o el espacio de nombres de carpeta.|
|[dfsutil Client](dfsutil-client.md)|Muestra o modifica las claves del registro o de información del cliente.|
|[dfsutil Server](dfsutil-server.md)|Muestra o modifica la configuración del espacio de nombres.|
|[Dfsutil Diag](dfsutil-diag.md)|Para realizar diagnósticos o ver dfsdirs\/dfspath.|
|[dfsutil Domain](dfsutil-domain.md)|Muestra todos los dominios\-en función de los espacios de nombres en un dominio.|
|[dfsutil Cache](dfsutil-cache.md)|Muestra o se vacía la caché del cliente.|
|[Dfsutil oldcli](dfsutil-oldcli.md)|Utilice el dfsutil \/oldcli comando con el uso de la sintaxis dfsutil original.|

## <a name="remarks-optional-section"></a>Comentarios <optional section>
Si especifica un objeto \(como un servidor de espacio de nombres\) al final de un comando, la mayoría de los comandos mostrará información sobre el objeto sin necesidad de más parámetros o los comandos. Por ejemplo, cuando se usa el comando raíz dfsutil, puede anexar una raíz de espacio de nombres para el comando para ver información acerca de la raíz.

## <a name="BKMK_Examples"></a>Ejemplos
&lt;Aquí es donde se coloca una descripción detallada del ejemplo.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;Aquí es donde se coloca una descripción detallada del otro ejemplo.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


