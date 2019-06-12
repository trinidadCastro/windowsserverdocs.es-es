---
title: dfsdiag TestDFSConfig
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 922b78b87f3bb66765b87348a3bf136e14c9e837
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436130"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de un sistema de archivos distribuido \(DFS\) espacio de nombres mediante la realización de las siguientes acciones:  
  
-   comprueba que se está ejecutando el servicio DFS Namespace y que su tipo de inicio esté establecido en automático en todos los servidores de espacio de nombres.  
  
-   comprueba que la configuración del registro DFS es coherente entre los servidores de espacio de nombres.  
  
-   Valida las siguientes dependencias en los servidores de espacio de nombres en clúster que ejecutan Windows Server 2008 o posterior:  
  
    -   Dependencia de recurso Namespace raíz en el recurso de nombre de red.  
  
    -   Dependencia de recurso de nombre de recurso de dirección IP de red.  
  
    -   Dependencia de recurso Namespace raíz en el recurso de disco físico.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parámetros  
  
|       Parámetro       |               Descripción               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | El espacio de nombres \(raíz DFS\) para diagnosticar. |
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para TBD, escriba:  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

