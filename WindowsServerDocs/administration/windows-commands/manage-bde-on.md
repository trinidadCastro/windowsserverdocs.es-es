---
title: Manage-BDE en
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a95bbc375c0a5b62b96f7c68f7d5ab5e09371d1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373993"
---
# <a name="manage-bde-on"></a>Manage-BDE: activado



Cifra la unidad y activa BitLocker. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<unidad >|Representa la letra de una unidad seguida del signo de dos puntos.|
|-RecoveryPassword|Agrega un protector de contraseña numérica. También puede usar **-RP** como una versión abreviada de este comando.|
|\<NumericalPassword >|Representa la contraseña de recuperación.|
|-recoverykey|Agrega un protector de clave externa para la recuperación. También puede usar **-RK** como una versión abreviada de este comando.|
|\<PathToExternalDirectory >|Representa la ruta de acceso del directorio a la clave de recuperación.|
|-clave|Agrega un protector de clave externa para el inicio. También puede usar **-SK** como una versión abreviada de este comando.|
|\<PathToExternalKeyDirectory >|Representa la ruta de acceso al directorio de la clave de inicio.|
|-certificado|Agrega un protector de clave pública para una unidad de datos. También puede usar **-CERT** como una versión abreviada de este comando.|
|-tpmandpin|Agrega un protector de Módulo de plataforma segura (TPM) y un número de identificación personal (PIN) para la unidad del sistema operativo. También puede usar **-TP** como una versión abreviada de este comando.|
|-tpmandstartupkey|Agrega un protector de clave de inicio y TPM para la unidad del sistema operativo. También puede usar **-TSK** como una versión abreviada de este comando.|
|-tpmandpinandstartupkey|Agrega un protector de TPM, PIN y clave de inicio para la unidad del sistema operativo. También puede usar **-tpsk** como una versión abreviada de este comando.|
|-contraseña|Agrega un protector de clave de contraseña para la unidad de datos. También puede usar **-PW** como una versión abreviada de este comando.|
|-ADAccountOrGroup|Agrega un protector de identidad basado en SID para el volumen. El volumen se desbloqueará automáticamente si el usuario o el equipo tienen las credenciales adecuadas. Al especificar una cuenta de equipo, anexe un **$** al nombre del equipo y especifique **– Service** para indicar que el desbloqueo debe llevarse a cabo en el contenido del servidor BitLocker en lugar de en el usuario. También puede usar **-SID** como una versión abreviada de este comando.|
|-UsedSpaceOnly|Establece el modo de cifrado para el cifrado solo del espacio usado. Se cifrarán las secciones del volumen que contiene el espacio usado, pero no el espacio disponible. Si no se especifica esta opción, se cifrarán todos los espacios usados y el espacio disponible en el volumen. También puede usar **: se usa** como una versión abreviada de este comando.|
|-encryptionMethod|Configura el algoritmo de cifrado y el tamaño de la clave. También puede usar **-em** como una versión abreviada de este comando.|
|-skiphardwaretest|Comienza el cifrado sin una prueba de hardware. También puede usar **-s** como una versión abreviada de este comando.|
|-discoveryvolumetype|Especifica el sistema de archivos que se va a usar para la unidad de datos de detección. La unidad de datos de detección es una unidad oculta que se ha agregado a una unidad de datos extraíbles con formato FAT y protegida por BitLocker que contiene la Lector de BitLocker To Go de modo que los sistemas operativos Windows Vista o Windows XP se pueden usar para ver las unidades protegidas por BitLocker.|
|-ForceEncryptionType|Fuerza a BitLocker a usar el software o el cifrado de hardware. Puede especificar el **hardware** o el **software** como el tipo de cifrado. Si el parámetro de **hardware** está seleccionado, pero la unidad no admite el cifrado de hardware, Manage-BDE devuelve un error. Si directiva de grupo configuración prohíbe el tipo de cifrado especificado, Manage-BDE devuelve un error. También puede usar **-FET** como una versión abreviada de este comando.|
|-RemoveVolumeShadowCopies|Fuerce la deletikon de instantáneas de volumen para el volumen. Después de ejecutar este comando, no podrá restaurar este volumen mediante puntos de restauración del sistema anteriores. También puede usar **-rvsc** como una versión abreviada de este comando.|
|\<FileSystemType >|Especifica los sistemas de archivos que se pueden usar con las unidades de datos de detección: FAT32, predeterminado o ninguno.|
|-COMPUTERNAME|Especifica que Manage-BDE se usa para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.|
|Nombre del \<>|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el siguiente ejemplo se muestra el uso del comando **-on** para Activar BitLocker para la unidad C y agregar una contraseña de recuperación a la unidad.
```
manage-bde –on C: -recoverypassword
```
En el siguiente ejemplo se muestra el uso del comando **-on** para Activar BitLocker para la unidad C, agregar una contraseña de recuperación a la unidad y guardar una clave de recuperación en la unidad E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
En el ejemplo siguiente se muestra el uso del comando **-on** para Activar BitLocker para la unidad C mediante un protector de clave externa (como una clave USB) para desbloquear la unidad del sistema operativo. Este método es necesario si usa BitLocker con equipos que no tienen un TPM.
```
manage-bde -on C: -startupkey E:\
```
En el ejemplo siguiente se muestra el uso del comando **-on** para Activar BitLocker para la unidad de datos e y agregar un protector de clave de contraseña. Manage-BDE le pedirá que escriba la contraseña una vez que se haya escrito este comando.
```
manage-bde –on E: -pw
```
En el ejemplo siguiente se muestra el uso del comando **-on** para Activar BitLocker para la unidad de sistema operativo C y usar el cifrado basado en hardware.
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Manage-BDE](manage-bde.md)