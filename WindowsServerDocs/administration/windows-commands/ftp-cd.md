---
title: ftp cd
description: Artículo de referencia del comando FTP CD, que cambia el directorio de trabajo en el equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0442e2ad6070b84e84632bc6d8646b979d7f948b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933295"
---
# <a name="ftp-cd"></a>ftp cd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el directorio de trabajo en el equipo remoto.

## <a name="syntax"></a>Sintaxis

```
cd <remotedirectory>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| <remotedirectory> | Especifica el directorio del equipo remoto al que desea cambiar. |

### <a name="examples"></a>Ejemplos

Para cambiar el directorio del equipo remoto a *docs*, escriba:

```
cd Docs
```

Para cambiar el directorio del equipo remoto a *vídeos de mayo*, escriba:

```
cd  May Videos
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
