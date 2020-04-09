---
title: bitsadmin getpeercachingflags
description: Comando de comandos de Windows para **bitsadmin getpeercachingflags**, que recupera marcas que determinan si los archivos del trabajo pueden almacenarse en caché y servirse a los elementos del mismo nivel, y si bits puede descargar contenido del trabajo desde los elementos del mismo nivel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdf9683d1a65400286b4604bd9420a5ab863d4af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850558"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel, y si BITS puede descargar el contenido del trabajo de los elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getpeercachingflags <job> 
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recuperan las marcas del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getpeercachingflags myJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)