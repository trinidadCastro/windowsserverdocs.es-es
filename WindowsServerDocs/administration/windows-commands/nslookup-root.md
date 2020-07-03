---
title: nslookup root
description: Artículo de referencia del comando Nslookup root, que cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio del sistema de nombres de dominio (DNS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07dbfcf401314145cb0e7553d71480072da4b574
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936285"
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
