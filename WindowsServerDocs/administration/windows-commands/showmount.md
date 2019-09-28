---
title: showmount
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1d197072db93130de880b5ec52d1875720b1d26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383915"
---
# <a name="showmount"></a>showmount

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar **showmount** para mostrar los directorios montados.  
  
## <a name="syntax"></a>Sintaxis  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descripción  
El comando **showmount** @ no__t-1line utilidad muestra información acerca de los sistemas de archivos montados exportados por servidor para NFS en el equipo especificado por *servidor*. Si no se proporciona el *servidor* , **showmount** muestra información sobre el equipo en el que se ejecuta el comando **showmount** .  
  
Debe proporcionar una de las siguientes opciones:  
  
- **\-E** -muestra todos los sistemas de archivos exportados en el servidor.  
- **\-A** -muestra todos los clientes de Network File System \(NFS @ no__t-3 y los directorios del servidor que se han montado.  
- **\-D** : muestra todos los directorios del servidor montados actualmente por clientes NFS.  
  
## <a name="see-also"></a>Vea también  
[Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)  