---
title: ftp prompt
description: Artículo de referencia para el comando FTP prompt, que activa y desactiva el modo de solicitud.
ms.topic: reference
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d045e72ace4364b1ab8832efa35728dc3af2b123
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624767"
---
# <a name="ftp-prompt"></a>ftp prompt

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Activa o desactiva el modo de solicitud. De forma predeterminada, el modo de mensajes está activado. Si el modo de solicitud está activado, el comando FTP solicitará al usuario varias transferencias de archivos para que pueda recuperar o almacenar archivos de forma selectiva.

> [!NOTE]
> Puede usar los comandos [FTP mget](ftp-mget.md) y [FTP mput](ftp-mput_1.md) para transferir todos los archivos cuando el modo de solicitud esté desactivado.

## <a name="syntax"></a>Sintaxis

```
prompt
```

### <a name="examples"></a>Ejemplos

Para activar y desactivar el modo de solicitud, escriba:

```
prompt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
