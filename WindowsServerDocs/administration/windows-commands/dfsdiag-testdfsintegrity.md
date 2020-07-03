---
title: dfsdiag testdfsintegrity
description: Artículo de referencia para el comando dfsdiag testdfsintegrity, que comprueba la integridad del espacio de nombres Sistema de archivos distribuido (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7896174bb58c957e4c24b1c3f7e1b2bacc9f95f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930630"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag testdfsintegrity

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba la integridad del espacio de nombres de Sistema de archivos distribuido (DFS) mediante la realización de las pruebas siguientes:

- Comprueba si hay incoherencias o daños en los metadatos DFS entre controladores de dominio.

- Valida la configuración de la enumeración basada en el acceso para asegurarse de que es coherente entre los metadatos DFS y el recurso compartido de servidor de espacio de nombres.

- Detecta carpetas DFS superpuestas (vínculos), carpetas duplicadas y carpetas con destinos de carpeta superpuestos.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /testdfsintegrity /DFSroot: <DFS root path> [/recurse] [/full]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /DFSroot:`<DFS root path>` | Espacio de nombres DFS que se va a diagnosticar. |
| /recurse | Realiza las pruebas, incluidos los intervínculos de espacios de nombres. |
| /Full | Comprueba la coherencia de las ACL del recurso compartido y NTFS, junto con la configuración del lado cliente en todos los destinos de carpeta. También comprueba que la propiedad online esté establecida. |

## <a name="examples"></a>Ejemplos

Para comprobar la integridad y la coherencia de los espacios de nombres de Sistema de archivos distribuido (DFS) en *contoso. com\MyNamespace*, incluidos los intervínculos, escriba:

```
dfsdiag /testdfsintegrity /DFSRoot:\contoso.com\MyNamespace /recurse /full
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando dfsdiag](dfsdiag.md)
