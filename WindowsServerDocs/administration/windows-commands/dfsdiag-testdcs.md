---
title: dfsdiag testdcs
description: Artículo de referencia para el comando dfsdiag testdcs, que comprueba la configuración de los controladores de dominio en el dominio especificado.
ms.topic: reference
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54bb3f2a1c724a77ab3a55c6f5158bda81de94f8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024029"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag testdcs

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba la configuración de los controladores de dominio mediante la realización de las siguientes pruebas en cada controlador de dominio del dominio especificado:

- Comprueba que el servicio de espacio de nombres Sistema de archivos distribuido (DFS) se está ejecutando y que su tipo de inicio está establecido en **automático**.

- Comprueba la compatibilidad de las referencias con costo de sitio para NETLOGON y SYSvol.

- Comprueba la coherencia de la Asociación del sitio por el nombre de host y la dirección IP.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /testdcs [/domain:<domain_name>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /Domain`<domain_name>` | Nombre del dominio que se va a comprobar. Este parámetro es opcional. El valor predeterminado es el dominio local al que se une el host local. |

## <a name="examples"></a>Ejemplos

Para comprobar la configuración de los controladores de dominio en el dominio *contoso.com* , escriba:

```
dfsdiag /testdcs /domain:contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando dfsdiag](dfsdiag.md)
