---
title: ¿Manage-bde en
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b50cad64025e85824a8f0a27d773ffb614491fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841186"
---
# <a name="manage-bde-on"></a>¿Manage-bde: en



Cifra la unidad y se activa BitLocker. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

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
|\<Drive>|Representa la letra de una unidad seguida del signo de dos puntos.|
|-recoverypassword|Agrega un protector de contraseña numérica. También puede usar **- rp** como una versión abreviada de este comando.|
|\<NumericalPassword>|Representa la contraseña de recuperación.|
|-recoverykey|Agrega un protector de clave externo para la recuperación. También puede usar **- rk** como una versión abreviada de este comando.|
|\<PathToExternalDirectory>|Representa la ruta de acceso de directorio a la clave de recuperación.|
|-startupkey|Agrega un protector de clave externo para el inicio. También puede usar **-sk** como una versión abreviada de este comando.|
|\<PathToExternalKeyDirectory>|Representa la ruta de acceso de directorio a la clave de inicio.|
|-certificado|Agrega un protector de clave público para una unidad de datos. También puede usar **-cert** como una versión abreviada de este comando.|
|-tpmandpin|Agrega un módulo de plataforma segura (TPM) y un protector (PIN) número de identificación personal para la unidad del sistema operativo. También puede usar **- tp** como una versión abreviada de este comando.|
|-tpmandstartupkey|Agrega un protector de clave de inicio y el TPM para la unidad del sistema operativo. También puede usar **-tsk** como una versión abreviada de este comando.|
|-tpmandpinandstartupkey|Agrega un protector de clave de inicio para la unidad del sistema operativo, TPM y PIN. También puede usar **- tpsk** como una versión abreviada de este comando.|
|-contraseña|Agrega un protector de clave de contraseña para la unidad de datos. También puede usar **- pw** como una versión abreviada de este comando.|
|-ADAccountOrGroup|Agrega un protector de identidad basada en SID para el volumen. El volumen se desbloqueará automáticamente si el usuario o equipo tiene las credenciales apropiadas. Anexar al especificar una cuenta de equipo, un **$** al equipo un nombre y especificar **: servicio** para indicar que debe ocurrir el desbloqueo del contenido del servidor en lugar de BitLocker de la usuario. También puede usar **-sid** como una versión abreviada de este comando.|
|-UsedSpaceOnly|Establece el modo de cifrado para el cifrado solo del espacio utilizado. Se cifrarán las secciones del volumen que contiene el espacio utilizado pero no el espacio libre. Si no se especifica esta opción, todo el espacio utilizado y el espacio libre en el volumen se cifrará... También puede usar **-usa** como una versión abreviada de este comando.|
|-encryptionMethod|Configura el tamaño de algoritmo y la clave de cifrado. También puede usar **-em** como una versión abreviada de este comando.|
|-skiphardwaretest|Inicia el cifrado sin una prueba de hardware. También puede usar **-s** como una versión abreviada de este comando.|
|-discoveryvolumetype|Especifica el sistema de archivos que se usará para la unidad de datos de detección. La unidad de datos de detección es una unidad oculta en una unidad de datos extraíbles con formato FAT, protegidas por BitLocker que contiene el lector de BitLocker To Go para que los sistemas operativos Windows Vista o Windows XP se puede usar para ver las unidades protegidas por BitLocker.|
|-ForceEncryptionType|Fuerza BitLocker al utilizar el cifrado de software o hardware. Puede especificar **Hardware** o **Software** como el tipo de cifrado. Si el **hardware** parámetro esté seleccionado, pero la unidad no admite el cifrado de hardware, ¿manage-bde devuelve un error. Si la configuración de directiva de grupo prohíbe el tipo de cifrado especificado, ¿manage-bde devuelve un error. También puede usar **-fet** como una versión abreviada de este comando.|
|-RemoveVolumeShadowCopies|Forzar deletikon de instantáneas de volumen para el volumen. No podrá restaurar este volumen mediante puntos de restauración del sistema anterior después de ejecutar este comando. También puede usar **- rvsc** como una versión abreviada de este comando.|
|\<FileSystemType>|Especifica qué sistemas de archivos se pueden usar con las unidades de datos de detección: FAT32, predeterminada o ninguno.|
|-computername|Especifica que se utiliza Manage-bde para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.|
|\<Nombre >|Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.|
|-? ¿o /?|Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h|Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se muestra cómo utilizar el **-en** comando para activar BitLocker para la unidad C y agrega una contraseña de recuperación a la unidad.
```
manage-bde –on C: -recoverypassword
```
El ejemplo siguiente se muestra cómo utilizar el **-en** comando para activar BitLocker para la unidad C, agregue una contraseña de recuperación a la unidad y guardar una clave de recuperación en la unidad E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
El ejemplo siguiente se muestra cómo utilizar el **-en** comandos para activar BitLocker para la unidad C mediante el uso de un protector de clave externo (por ejemplo, una llave USB) para desbloquear la unidad del sistema operativo. Este método es necesario si va a usar BitLocker con equipos que no tienen un TPM.
```
manage-bde -on C: -startupkey E:\
```
El ejemplo siguiente se muestra cómo utilizar el **-en** comando para activar BitLocker para unidades de datos E y agregar un protector de clave de contraseña. ¿Manage-bde le solicitará que escriba la contraseña después de escribir este comando.
```
manage-bde –on E: -pw
```
El ejemplo siguiente se muestra cómo utilizar el **-en** comando para activar BitLocker para la unidad C del sistema operativo y usar el cifrado basado en hardware.
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [¿Manage-bde](manage-bde.md)