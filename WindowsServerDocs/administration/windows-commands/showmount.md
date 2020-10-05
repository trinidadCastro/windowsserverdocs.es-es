---
title: showmount
description: Artículo de referencia para el comando showmount, que muestra información acerca de los sistemas de archivos montados exportados por servidor para NFS en un equipo especificado.
ms.topic: reference
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3af87ba4ed42789cf07a2ae63ce9e720043a196e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718332"
---
# <a name="showmount"></a>showmount

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Puede usar **showmount** para mostrar información acerca de los sistemas de archivos montados exportados por servidor para NFS en un equipo especificado. Si no especifica un servidor, este comando muestra información sobre el equipo en el que se ejecuta el comando **showmount** .

## <a name="syntax"></a>Sintaxis

```
showmount {-e|-a|-d} <server>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -E | Muestra todos los sistemas de archivos exportados en el servidor. |
| -a | Muestra todos los clientes de Network File System (NFS) y los directorios en el servidor que se han montado. |
| -d | Muestra todos los directorios del servidor montados actualmente por clientes NFS. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)