---
title: nslookup root
description: Artículo de referencia del comando Nslookup root, que cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio del sistema de nombres de dominio (DNS).
ms.topic: reference
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e3dbbdf970143ce1e82f0c817fc1f53c5b22749
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635656"
---
# <a name="nslookup-root"></a>nslookup root

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio del sistema de nombres de dominio (DNS). Actualmente, se usa el servidor de nombres ns.nic.ddn.mil. Puede cambiar el nombre del servidor raíz mediante el comando [nslookup Set root](nslookup-set-root.md) .

> [!NOTE]
> Este comando es igual que `lserver ns.nic.ddn.mil` .

## <a name="syntax"></a>Sintaxis

```
root
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup set root](nslookup-set-root.md)
