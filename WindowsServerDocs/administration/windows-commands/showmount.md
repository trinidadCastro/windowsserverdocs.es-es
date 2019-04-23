---
title: showmount
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852356"
---
# <a name="showmount"></a>showmount

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar **showmount** para mostrar los directorios montados.  
  
## <a name="syntax"></a>Sintaxis  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descripción  
El **showmount** comando\-utilidad de línea muestra información acerca de los sistemas de archivos montado exportados por servidor para NFS en el equipo especificado por *Server*. Si *Server* no se proporciona, **showmount** muestra información acerca del equipo en el que el **showmount** se ejecuta el comando.  
  
Debe proporcionar una de las opciones siguientes:  
  
- **\-e** -muestra todos los sistemas que se exportan en el servidor de archivos.  
- **\-un** -muestra todos los Network File System \(NFS\) los clientes y los directorios en el servidor de cada uno de ellos ha montado.  
- **\-d.** -muestra todos los directorios en el servidor que actualmente se montan los clientes NFS.  
  
## <a name="see-also"></a>Vea también  
[Servicios para la referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)  