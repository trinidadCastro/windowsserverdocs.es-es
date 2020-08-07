---
title: showmount
description: Artículo de referencia para showmount, que muestra los directorios montados.
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0be623eadd56a55a87f2df57fec9b4c6558770c9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882408"
---
# <a name="showmount"></a>showmount

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Puede usar **showmount** para mostrar los directorios montados.

## <a name="syntax"></a>Sintaxis
```
showmount {-e|-a|-d} <Server>
```

## <a name="description"></a>Descripción
La **showmount** utilidad de \- línea de comandos showmount muestra información acerca de los sistemas de archivos montados exportados por servidor para NFS en el equipo especificado por *servidor*. Si no se proporciona el *servidor* , **showmount** muestra información sobre el equipo en el que se ejecuta el comando **showmount** .

Debe proporcionar una de las siguientes opciones:

- ** \- e** -muestra todos los sistemas de archivos exportados en el servidor.
- ** \- a** : muestra todos los clientes NFS de Network File System \( \) y los directorios en el servidor que se han montado.
- ** \- d** -muestra todos los directorios del servidor que actualmente están montados por clientes NFS.

## <a name="see-also"></a>Consulte también
[Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)