---
title: CD FTP
description: Tema de referencia del comando FTP CD, que cambia el directorio de trabajo en el equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbeff9c2fca654120347f0f616325b700f5c9be3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820005"
---
# <a name="ftp-cd"></a>CD FTP

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
