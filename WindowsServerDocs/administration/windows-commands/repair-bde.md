---
title: repair-bde
description: Artículo de referencia para el comando repair-BDE, que puede intentar reconstruir partes críticas de una unidad gravemente dañada y salvar los datos recuperables si la unidad se cifró mediante BitLocker.
ms.topic: reference
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7eb1128a34f366d5a096558118f43473252c8d4
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904090"
---
# <a name="repair-bde"></a>repair-bde

Intenta reconstruir partes críticas de una unidad gravemente dañada y salvar los datos recuperables si la unidad se cifró mediante BitLocker y si tiene una contraseña de recuperación o una clave de recuperación válidas para el descifrado.

> [!IMPORTANT]
> Si los datos de los metadatos de BitLocker en la unidad están dañados, debe ser capaz de proporcionar un paquete de claves de copia de seguridad además de la contraseña de recuperación o la clave de recuperación. Si usó la configuración de copia de seguridad de clave predeterminada para Active Directory Domain Services, se realizará una copia de seguridad del paquete de claves. Puede usar [BitLocker: usar el visor de contraseñas de recuperación de BitLocker](/windows/security/information-protection/bitlocker/bitlocker-use-bitlocker-recovery-password-viewer) para obtener el paquete de claves de AD DS.
>
> Con el paquete de claves y la contraseña de recuperación o la clave de recuperación, puede descifrar partes de una unidad protegida con BitLocker, incluso si el disco está dañado. Cada paquete de claves solo funciona para una unidad con el identificador de unidad correspondiente.

## <a name="syntax"></a>Sintaxis

```
repair-bde <inputvolume> <outputvolumeorimage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

> [!WARNING]
> El contenido del volumen de salida se **eliminará por completo y se sobrescribirá** con el contenido descifrado de la unidad BitLocker dañada. Si desea guardar los datos existentes en la unidad de destino seleccionada, mueva primero los datos existentes a otros medios de copia de seguridad de confianza antes de ejecutar el `repair-bde` comando.

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
| --- | --- |
| `<inputvolume>` | Identifica la letra de unidad de la unidad cifrada con BitLocker que desea reparar. La letra de unidad debe incluir un signo de dos puntos. por ejemplo: **C:**. Si no se especifica la ruta de acceso a un paquete de claves, este comando busca un paquete de claves en la unidad. En caso de que la unidad de disco duro esté dañada, es posible que este comando no pueda encontrar el paquete y le pedirá que proporcione la ruta de acceso. |
| `<outputvolumeorimage>` | Identifica la unidad en la que se va a almacenar el contenido de la unidad reparada. Se sobrescribirá toda la información de la unidad de salida. |
| -RK | Identifica la ubicación de la clave de recuperación que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-RecoveryKey**. |
| -RP | Identifica la contraseña de recuperación numérica que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-RecoveryPassword**. |
| -PW | Identifica la contraseña que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-password** . |
| -KP | Identifica el paquete de claves de recuperación que se puede usar para desbloquear el volumen. Este comando también se puede especificar como **-keypackage**. |
| -lf | Especifica la ruta de acceso al archivo que almacenará los mensajes de error, de advertencia y de información de repair-Bde. Este comando también se puede especificar como **-logfile**. |
| -f | Fuerza el desmontaje de un volumen incluso si no se puede bloquear. Este comando también se puede especificar como **Force**. |
| -? o/? | Muestra la Ayuda en el símbolo del sistema. |

### <a name="limitations"></a>Limitaciones

Existen las siguientes limitaciones para este comando:

- Este comando no puede reparar una unidad en la que se produjo un error durante el proceso de cifrado o descifrado.

- Este comando presupone que, si la unidad tiene cifrado, la unidad se ha cifrado completamente.

## <a name="examples"></a>Ejemplos

Para intentar reparar la unidad C:, escriba el contenido de la unidad C: en la unidad D: mediante el archivo de clave de recuperación (RecoveryKey. Bek) almacenado en la unidad F:, y para escribir los resultados de este intento en el archivo de registro (log.txt) en la unidad Z:, escriba:

```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```

Para intentar reparar la unidad C: y escribir el contenido de la unidad C: en la unidad D: con la contraseña de recuperación 48-digit especificada, escriba:

```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```

> [!NOTE]
> La contraseña de recuperación debe escribirse en ocho bloques de seis dígitos con un guion separando cada bloque.

Para forzar la unidad C: a desmontar, intente reparar la unidad C: y, a continuación, escriba el contenido de la unidad C: en la unidad D: con el paquete de claves de recuperación y el archivo de clave de recuperación (RecoveryKey. Bek) almacenados en la unidad F:, escriba:

```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```

Para intentar reparar la unidad C: y escribir el contenido de la unidad C: en la unidad D:, donde debe escribir una contraseña para desbloquear la unidad C: (cuando se le solicite), escriba:

```
repair-bde C: D: -pw
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
