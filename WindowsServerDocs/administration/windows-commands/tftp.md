---
title: tftp
description: Transferir archivos a y desde un equipo remoto.
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e085d86ea8dff5261f77505df4d5d7c2c6adb675
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881611"
---
# <a name="tftp"></a>tftp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Transfiere archivos a y desde un equipo remoto, normalmente un equipo que ejecuta UNIX, que ejecuta el servicio o el demonio de File Transfer Protocol trivial (TFTP). TFTP suele usarse en dispositivos o sistemas incrustados que recuperan el firmware, la información de configuración o una imagen del sistema durante el proceso de arranque desde un servidor TFTP.

## <a name="syntax"></a>Sintaxis
```
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]
```

#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|-i|Especifica el modo de transferencia de imagen binaria (también denominado modo de octeto). En el modo de imagen binaria, el archivo se transfiere en unidades de un byte. Utilice este modo al transferir archivos binarios. Si se omite **-i** , el archivo se transfiere en modo ASCII. Este es el modo de transferencia predeterminado. Este modo convierte los caracteres de fin de línea (EOL) a un formato adecuado para el equipo especificado. Utilice este modo al transferir archivos de texto. Si una transferencia de archivos se realiza correctamente, se muestra la velocidad de transferencia de datos.|
|\<Host\>|Especifica el equipo local o remoto.|
|put|Transfiere el *origen* de archivo del equipo local al *destino* del archivo en el equipo remoto. Dado que el protocolo TFTP no admite la autenticación de usuario, el usuario debe haber iniciado sesión en el equipo remoto y los archivos deben poder escribirse en el equipo remoto.|
|get|Transfiere el *destino* de archivo del equipo remoto al *origen* de archivo en el equipo local.|
|\<Source\>|Especifica el archivo que se va a transferir.|
|\<Destination\>|Especifica dónde se debe transferir el archivo.|

## <a name="remarks"></a>Observaciones
-   Puede instalar el cliente de TFTP mediante el Asistente para agregar características.
-   El protocolo TFTP no admite ningún mecanismo de autenticación o cifrado y, por lo tanto, puede suponer un riesgo de seguridad cuando está presente. No se recomienda instalar el cliente TFTP para sistemas conectados a Internet.
-   El cliente TFTP es software opcional y está marcado como en desuso en Windows Vista y versiones posteriores del sistema operativo Windows. Microsoft ya no proporciona un servicio de servidor TFTP por motivos de seguridad.

## <a name="examples"></a>Ejemplos
Copie el archivo **boot. img** del equipo remoto **Host1**.
```
tftp  -i Host1 get boot.img
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
