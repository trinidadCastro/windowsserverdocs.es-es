---
title: bitsadmin complete
description: Tema de los comandos de Windows para **bitsadmin completa** -completa el trabajo. Los archivos descargados no están disponibles hasta que se usa este modificador.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561585da370f7e69aa3b83b3ddd7579bfc658a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817326"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa el trabajo. Los archivos descargados no están disponibles hasta que se usa este modificador. Utilice este modificador después de que el trabajo pasa al estado transferido. En caso contrario, sólo los archivos que se han transferido correctamente están disponibles.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

Cuando se TRANSFIERE el estado del trabajo, BITS le ha transferido correctamente todos los archivos en el trabajo. Sin embargo, los archivos no están disponibles hasta que se utiliza el **/ completa** cambie. Si usan varios trabajos *myDownloadJob* como su nombre, debe reemplazar *myDownloadJob* con el GUID del trabajo para identificar de forma única el trabajo.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)