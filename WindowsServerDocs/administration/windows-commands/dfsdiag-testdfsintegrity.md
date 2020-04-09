---
title: dfsdiag TestDFSIntegrity
description: Tema de comandos de Windows para **Dfsdiag TestDFSIntegrity**, que comprueba la integridad del espacio de nombres sistema de archivos distribuido (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 714b79369898338a4e4a6e4fad8487709ab4fc60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846278"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la integridad del espacio de nombres de Sistema de archivos distribuido (DFS) mediante la realización de las pruebas siguientes:

- Comprueba si hay incoherencias o daños en los metadatos DFS entre controladores de dominio.

- Valida la configuración de la enumeración basada en el acceso para asegurarse de que es coherente entre los metadatos DFS y el recurso compartido de servidor de espacio de nombres.

- Detecta carpetas DFS superpuestas (vínculos), carpetas duplicadas y carpetas con destinos de carpeta superpuestos.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /TestDFSIntegrity /DFSRoot: <DFS root path> [/Recurse] [/Full]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|-------|--------|
| /DFSRoot: `<DFS root path>`| Espacio de nombres DFS que se va a diagnosticar. |
| /Recurse | Realiza las pruebas, incluyendo los intervínculos de espacios de nombres. |
| /Full | Comprueba la coherencia de las ACL de recursos compartidos y NTFS y la configuración del lado cliente en todos los destinos de carpeta. También comprueba que la propiedad online esté establecida. |

## <a name="examples"></a><a name=BKMK_Examples></a>Example

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

-   [dfsdiag](dfsdiag.md)


