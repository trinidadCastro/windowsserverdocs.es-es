---
title: dfsdiag
description: Tema de referencia de dfsdiag, que proporciona información de diagnóstico para los espacios de nombres DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d5a9b147994628ccad6a723311decbccbe82ec6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719550"
---
# <a name="dfsdiag"></a>dfsdiag

Proporciona información de diagnóstico para los espacios de nombres DFS.

## <a name="syntax"></a>Sintaxis

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Comprueba la configuración del controlador de dominio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Comprueba las asociaciones de sitio.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Comprueba la configuración del espacio de nombres DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Comprueba la integridad del espacio de nombres DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Comprueba las respuestas de referencia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)