---
title: ¿Manage-bde desbloquear
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814606"
---
# <a name="manage-bde-unlock"></a>¿Manage-bde: desbloquear



Desbloquea una unidad protegida con BitLocker con una contraseña de recuperación o una clave de recuperación. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|Descripción|
|---------|-----|-----------|
|-recoverypassword||Especifica que se utilizará una contraseña de recuperación para desbloquear la unidad. Abreviatura: - rp|
||\<Contraseña >|Representa la contraseña de recuperación que puede usarse para desbloquear la unidad.|
|-recoverykey||Especifica que se utilizará un archivo de clave externa de recuperación para desbloquear la unidad. Abreviatura: - rk|
||\<PathToExternalKeyFile>|Representa el archivo de clave externa de recuperación que puede usarse para desbloquear la unidad.|
||\<Drive>|Representa la letra de una unidad seguida del signo de dos puntos.|
|-certificado||El certificado de usuario local para un certificado de BitLocker en unclock el volumen se encuentra en el almacén de certificados de usuario local. Abreviatura:-cert|
||<-cf PathToCertificateFile >|Ruta de acceso al archivo cerficate|
||<-ct CertificateThumbprint>|Huella digital del certificado que se puede incluir opcionalmente el PIN (-pin).|
|-contraseña||Presenta un símbolo del sistema para la contraseña para desbloquear el volumen. Abreviatura: - pw|
|-computername||Especifica que se utilizará Manage-bde.exe para modificar la protección de BitLocker en un equipo diferente. Abreviatura: - cn|
||\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.|
|-? ¿o /?||Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h||Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **-desbloquear** comando para desbloquear la unidad E con un archivo de clave de recuperación que se ha guardado en una carpeta de copia de seguridad en otra unidad.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [¿Manage-bde](manage-bde.md)