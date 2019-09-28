---
title: 'administrar: desbloqueo de BDE'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 92ed2e00babfad890be83e45827ae8e0080cac40
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373872"
---
# <a name="manage-bde-unlock"></a>Manage-BDE: Unlock



Desbloquea una unidad protegida con BitLocker mediante una contraseña de recuperación o una clave de recuperación. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|Descripción|
|---------|-----|-----------|
|-RecoveryPassword||Especifica que se usará una contraseña de recuperación para desbloquear la unidad. Abreviatura:-RP|
||@no__t 0Password >|Representa la contraseña de recuperación que se puede usar para desbloquear la unidad.|
|-recoverykey||Especifica que se usará un archivo de clave de recuperación externa para desbloquear la unidad. Abreviatura:-RK|
||@no__t 0PathToExternalKeyFile >|Representa el archivo de clave de recuperación externa que se puede usar para desbloquear la unidad.|
||@no__t 0Drive >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-certificado||El certificado de usuario local de un certificado de BitLocker para deshacer el reloj del volumen se encuentra en el almacén de certificados de usuario de búsqueda. Abreviatura:-CERT|
||<-CF PathToCertificateFile >|Ruta de acceso al archivo Cerficate|
||<-CT CertificateThumbprint >|Huella digital del certificado que puede incluir opcionalmente el PIN (-PIN).|
|-contraseña||Presenta un símbolo del sistema para la contraseña para desbloquear el volumen. Abreviatura:-PW|
|-COMPUTERNAME||Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. Abreviatura:-CN|
||\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?||Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h||Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra el uso del comando **-Unlock** para desbloquear la unidad E con un archivo de clave de recuperación que se ha guardado en una carpeta de copia de seguridad de otra unidad.
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)