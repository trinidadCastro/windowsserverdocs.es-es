---
title: dfsdiag testdcs
description: Artículo de referencia para el comando dfsdiag testdcs, que comprueba la configuración de los controladores de dominio en el dominio especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eca75d233661d51a36b52b79230ad36b704e203
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930667"
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
