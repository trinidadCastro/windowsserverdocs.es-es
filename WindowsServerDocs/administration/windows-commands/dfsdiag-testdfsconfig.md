---
title: dfsdiag TestDFSConfig
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8008e02d588edaa6fe7700a331c43f9680d89431
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378419"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de un Sistema de archivos distribuido \(el espacio de nombres\) DFS mediante la realización de las siguientes acciones:  
  
-   comprueba que el servicio de espacio de nombres DFS se está ejecutando y que su tipo de inicio está establecido en automático en todos los servidores de espacio de nombres.  
  
-   comprueba que la configuración del registro DFS es coherente entre los servidores de espacio de nombres.  
  
-   Valida las siguientes dependencias en servidores de espacio de nombres en clúster que ejecutan Windows Server 2008 o posterior:  
  
    -   Dependencia de recursos raíz de espacio de nombres en el recurso de nombre de red.  
  
    -   Dependencia de recurso de nombre de red en el recurso de dirección IP.  
  
    -   Dependencia de recursos raíz del espacio de nombres en el recurso de disco físico.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parámetros  
  
|       Parámetro       |               Descripción               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | Espacio de nombres \(\) raíz DFS que se va a diagnosticar. |
  
## <a name="BKMK_Examples"></a>Example  
En TBD, escriba:  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

