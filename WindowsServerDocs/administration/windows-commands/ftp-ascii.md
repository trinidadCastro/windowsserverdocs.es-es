---
title: ftp ascii
description: Artículo de referencia del comando ASCII de FTP, que establece el tipo de transferencia de archivos en ASCII.
ms.topic: reference
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 75b7918451836c1cda67fe5b5f6d8d8d3b73fde0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638560"
---
# <a name="ftp-ascii"></a>ftp ascii

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece el tipo de transferencia de archivos en ASCII. El comando **FTP** admite los tipos de transferencia de archivos de imagen ASCII (predeterminada) y binaria, pero se recomienda el uso de ASCII al transferir archivos de texto. En el modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, en función del sistema operativo de destino.

## <a name="syntax"></a>Sintaxis

```
ascii
```

### <a name="examples"></a>Ejemplos

Para establecer el tipo de transferencia de archivo en ASCII, escriba:

```
ascii
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
