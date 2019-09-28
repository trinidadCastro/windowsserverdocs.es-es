---
title: nslookup root
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3eb3375df3a109685fc8dc5d23f0c5008339d09e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373391"
---
# <a name="nslookup-root"></a>nslookup root

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio del sistema de nombres de dominio (DNS).
## <a name="syntax"></a>Sintaxis
```
root 
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                      Descripción                      |
|-----------------|-------------------------------------------------------|
| {ayuda &#124; ?} | Muestra un breve resumen de los subcomandos de **nslookup** . |

## <a name="remarks"></a>Comentarios
- Actualmente, se usa el servidor de nombres ns.nic.ddn.mil. Este comando es un sinónimo de lserver ns.nic.ddn.mil. Puede cambiar el nombre del servidor raíz con el comando **establecer raíz** .
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup establecer raíz](nslookup-set-root.md)
