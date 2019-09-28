---
title: dfsdiag
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61a6ab9a90e4d0220cfe27d2d21120be19b9ff1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378312"
---
# <a name="dfsdiag"></a>dfsdiag



El comando `Dfsdiag` proporciona información de diagnóstico para los espacios de nombres DFS.

## <a name="syntax"></a>Sintaxis

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Comprueba la configuración del controlador de dominio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Comprueba las asociaciones de sitio.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Comprueba la configuración del espacio de nombres DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Comprueba la integridad del espacio de nombres DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Comprueba las respuestas de referencia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)