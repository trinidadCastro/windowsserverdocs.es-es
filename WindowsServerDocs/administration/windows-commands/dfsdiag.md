---
title: dfsdiag
description: Temas de comandos de Windows para dfsdiag, que proporciona información de diagnóstico para los espacios de nombres DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c895dabbbafbe8ea253920d3bc6de17f42918e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846198"
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
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)