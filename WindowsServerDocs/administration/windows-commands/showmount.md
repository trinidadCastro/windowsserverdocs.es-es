---
title: showmount
description: Temas de comandos de Windows para showmount, que muestra los directorios montados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fa61d47bb14cf21d93beec0a6e9257b9f66737b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834228"
---
# <a name="showmount"></a>showmount

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar **showmount** para mostrar los directorios montados.  
  
## <a name="syntax"></a>Sintaxis  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descripción  
La utilidad de línea de\-de comandos **showmount** muestra información acerca de los sistemas de archivos montados exportados por servidor para NFS en el equipo especificado por *servidor*. Si no se proporciona el *servidor* , **showmount** muestra información sobre el equipo en el que se ejecuta el comando **showmount** .  
  
Debe proporcionar una de las siguientes opciones:  
  
- **\-e** : muestra todos los sistemas de archivos exportados en el servidor.  
- **\-a** : muestra todos los clientes del sistema de archivos de red \(NFS\) y los directorios en el servidor montados.  
- **\-d** : muestra todos los directorios del servidor montados actualmente por clientes NFS.  
  
## <a name="see-also"></a>Consulta también  
[Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)  