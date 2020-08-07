---
title: Manage-BDE en
description: Artículo de referencia para el comando Manage-BDE on, que cifra la unidad y activa BitLocker.
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb1f58cb5f9683e4f9c01343d86af3332c8c3e32
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886798"
---
# <a name="manage-bde-on"></a>Manage-BDE en

Cifra la unidad y activa BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde –on <drive> {[-recoverypassword <numericalpassword>]|[-recoverykey <pathtoexternaldirectory>]|[-startupkey <pathtoexternalkeydirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <pathtoexternalkeydirectory>]|[-tpmandstartupkey <pathtoexternalkeydirectory>]|[-password]|[-ADaccountorgroup <domain\account>]}
[-usedspaceonly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <filesystemtype>] [-forceencryptiontype <type>] [-removevolumeshadowcopies][-computername <name>]
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -RecoveryPassword | Agrega un protector de contraseña numérica. También puede usar **-RP** como una versión abreviada de este comando. |
| `<numericalpassword>` | Representa la contraseña de recuperación. |
| -recoverykey | Agrega un protector de clave externa para la recuperación. También puede usar **-RK** como una versión abreviada de este comando. |
| `<pathtoexternaldirectory>` | Representa la ruta de acceso del directorio a la clave de recuperación. |
| -clave | Agrega un protector de clave externa para el inicio. También puede usar **-SK** como una versión abreviada de este comando. |
| `<pathtoexternalkeydirectory>` | Representa la ruta de acceso al directorio de la clave de inicio. |
| -certificado | Agrega un protector de clave pública para una unidad de datos. También puede usar **-CERT** como una versión abreviada de este comando. |
| -tpmandpin | Agrega un protector de Módulo de plataforma segura (TPM) y un número de identificación personal (PIN) para la unidad del sistema operativo. También puede usar **-TP** como una versión abreviada de este comando. |
| -tpmandstartupkey | Agrega un protector de clave de inicio y TPM para la unidad del sistema operativo. También puede usar **-TSK** como una versión abreviada de este comando. |
| -tpmandpinandstartupkey | Agrega un protector de TPM, PIN y clave de inicio para la unidad del sistema operativo. También puede usar **-tpsk** como una versión abreviada de este comando. |
| -password | Agrega un protector de clave de contraseña para la unidad de datos. También puede usar **-PW** como una versión abreviada de este comando. |
| -ADaccountorgroup | Agrega un protector de identidad basado en SID para el volumen. El volumen se desbloqueará automáticamente si el usuario o el equipo tienen las credenciales adecuadas. Al especificar una cuenta de equipo, anexe un **$** al nombre del equipo y especifique **– Service** para indicar que el desbloqueo debe realizarse en el contenido del servidor BitLocker en lugar de en el usuario. También puede usar **-SID** como una versión abreviada de este comando. |
| -usedspaceonly | Establece el modo de cifrado para el cifrado solo del espacio usado. Se cifrarán las secciones del volumen que contiene el espacio usado, pero no el espacio disponible. Si no se especifica esta opción, se cifrarán todos los espacios usados y el espacio disponible en el volumen. También puede usar **: se usa** como una versión abreviada de este comando. |
| -encryptionMethod | Configura el algoritmo de cifrado y el tamaño de la clave. También puede usar **-em** como una versión abreviada de este comando. |
| -skiphardwaretest | Comienza el cifrado sin una prueba de hardware. También puede usar **-s** como una versión abreviada de este comando. |
| -discoveryvolumetype | Especifica el sistema de archivos que se va a usar para la unidad de datos de detección. La unidad de datos de detección es una unidad oculta agregada a una unidad de datos extraíbles con formato FAT y protegida por BitLocker que contiene el Lector de BitLocker To Go. |
| -forceencryptiontype | Fuerza a BitLocker a usar el software o el cifrado de hardware. Puede especificar el **hardware** o el **software** como el tipo de cifrado. Si el parámetro de **hardware** está seleccionado, pero la unidad no admite el cifrado de hardware, Manage-BDE devuelve un error. Si directiva de grupo configuración prohíbe el tipo de cifrado especificado, Manage-BDE devuelve un error. También puede usar **-FET** como una versión abreviada de este comando. |
| -removevolumeshadowcopies | Forzar la eliminación de instantáneas de volumen para el volumen. No podrá restaurar este volumen mediante los puntos de restauración del sistema anteriores después de ejecutar este comando. También puede usar **-rvsc** como una versión abreviada de este comando. |
| `<filesystemtype>` | Especifica los sistemas de archivos que se pueden usar con las unidades de datos de detección: FAT32, predeterminado o ninguno. |
| -COMPUTERNAME | Especifica que Manage-BDE se usa para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para Activar BitLocker en la unidad C y agregar una contraseña de recuperación a la unidad, escriba:

```
manage-bde –on C: -recoverypassword
```

Para Activar BitLocker en la unidad C, agregue una contraseña de recuperación a la unidad y, para guardar una clave de recuperación en la unidad E, escriba:

```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```

Para Activar BitLocker para la unidad C, mediante un protector de clave externa (como una clave USB) para desbloquear la unidad del sistema operativo, escriba:

```
manage-bde -on C: -startupkey E:\
```

> [!IMPORTANT]
> Este método es necesario si usa BitLocker con equipos que no tienen un TPM.

Para Activar BitLocker para la unidad de datos E y agregar un protector de clave de contraseña, escriba:

```
manage-bde –on E: -pw
```

Para Activar BitLocker para la unidad de sistema operativo C y para usar el cifrado basado en hardware, escriba:

```
manage-bde –on C: -fet hardware
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE OFF](manage-bde-off.md)

- [comando Manage-BDE PAUSE](manage-bde-pause.md)

- [comando Manage-BDE resume](manage-bde-resume.md)

- [comando Manage-BDE](manage-bde.md)
