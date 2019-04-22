---
title: dfsdiag
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819146"
---
# <a name="dfsdiag"></a>dfsdiag



El `Dfsdiag` comando proporciona información de diagnóstico para espacios de nombres DFS.

## <a name="syntax"></a>Sintaxis

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Comprueba la configuración del controlador de dominio.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Comprobaciones de las asociaciones de sitio.|
|[dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Comprueba la configuración de DFS Namespace.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Comprueba la integridad de DFS Namespace.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Comprueba las respuestas de referencia.|
|/?|Muestra la ayuda en el símbolo del sistema.|

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)