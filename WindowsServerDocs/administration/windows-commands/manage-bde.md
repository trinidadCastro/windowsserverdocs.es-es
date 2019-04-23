---
title: manage-bde
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874506"
---
# <a name="manage-bde"></a>manage-bde



Se usa para activar o desactivar BitLocker, especificar mecanismos de desbloqueo, actualizar métodos de recuperación y desbloquear unidades de datos protegidas por BitLocker. Esta herramienta de línea de comandos puede usarse en lugar de la **cifrado de unidad BitLocker** elemento del Panel de Control. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[¿Manage-bde: estado](manage-bde-status.md)|Proporciona información sobre todas las unidades en el equipo, si están protegidas por BitLocker.|
|[¿Manage-bde: en](manage-bde-on.md)|Cifra la unidad y se activa BitLocker.|
|[¿Manage-bde: desactivado](manage-bde-off.md)|Descifra la unidad y desactiva BitLocker. Una vez completada descifrado, se quitan todos los protectores de clave.|
|[¿Manage-bde: pausar](manage-bde-pause.md)|Pausa el cifrado o descifrado.|
|[¿Manage-bde: reanudar](manage-bde-resume.md)|Reanuda el cifrado o descifrado.|
|[¿Manage-bde: bloqueo](manage-bde-lock.md)|Impide el acceso a datos protegidos por BitLocker.|
|[¿Manage-bde: desbloquear](manage-bde-unlock.md)|Permite el acceso a datos protegidos por BitLocker con una contraseña de recuperación o una clave de recuperación.|
|[¿Manage-bde: desbloqueo automático](manage-bde-autounlock.md)|Administra el desbloqueo automático de unidades de datos.|
|[¿Manage-bde: protectores](manage-bde-protectors.md)|Administra los métodos de protección de la clave de cifrado.|
|[¿Manage-bde: tpm](manage-bde-tpm.md)|Configura módulo de plataforma segura (TPM del equipo). Este comando no se admite en equipos que ejecutan Windows 8 o **win8_server_2**. Para administrar el TPM en estos equipos, use el complemento MMC de administración de TPM o los cmdlets de administración de TPM para Windows PowerShell.|
|[¿Manage-bde: setidentifier](manage-bde-setidentifier.md)|Establece el campo de identificador de unidad en la unidad para el valor especificado en el **proporcionar identificadores únicos para su organización** configuración de directiva de grupo.|
|[¿Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)|Obliga a que una unidad protegida con BitLocker en modo de recuperación de reinicio. Este comando elimina todos los protectores de clave relacionadas con el TPM de la unidad. Cuando se reinicia el equipo, sólo una contraseña o clave de recuperación puede usarse para desbloquear la unidad.|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|Modifica la contraseña de una unidad de datos.|
|[¿Manage-bde: changepin actualiza](manage-bde-changepin.md)|Modifica el código PIN para una unidad del sistema operativo.|
|[Manage-bde: changekey](manage-bde-changekey.md)|Modifica la clave de inicio para una unidad del sistema operativo.|
|[¿Manage-bde: KeyPackage](manage-bde-keypackage.md)|Genera un paquete de claves para una unidad.|
|[¿Manage-bde: actualizar](manage-bde-upgrade.md)|Actualiza la versión de BitLocker.|
|[¿Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Elimina el espacio libre en una unidad.|
|-? ¿o /?|Muestra una breve ayuda en el símbolo del sistema.|
|-help o -h|Muestra la Ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente muestra las unidades de disco en el equipo e identifica si son o no protegidas por BitLocker y el estado actual de cifrado.
```
manage-bde -status
```
El ejemplo siguiente muestra habilitar BitLocker en la unidad C con la opción de una contraseña de recuperación. La contraseña de recuperación se generada por BitLocker y se mostrará en la pantalla para que se puede registrar.
```
manage-bde –on C: -recoverypassword
```
El siguiente ejemplo ilustra desbloquear una unidad protegida con BitLocker mediante el uso de una contraseña de recuperación.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Habilitación de BitLocker mediante el uso de la línea de comandos](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
