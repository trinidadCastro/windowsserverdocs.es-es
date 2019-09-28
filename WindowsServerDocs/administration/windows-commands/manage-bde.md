---
title: manage-bde
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7ca23e5f4499672f1e4bfcca6b9ad27f4e84039b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373767"
---
# <a name="manage-bde"></a>manage-bde



Se usa para activar o desactivar BitLocker, especificar mecanismos de desbloqueo, actualizar métodos de recuperación y desbloquear unidades de datos protegidas por BitLocker. Esta herramienta de línea de comandos se puede usar en lugar de la **cifrado de unidad BitLocker** elemento del panel de control. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|Proporciona información acerca de todas las unidades del equipo, tanto si están protegidas por BitLocker como si no.|
|[Manage-bde: on](manage-bde-on.md)|Cifra la unidad y activa BitLocker.|
|[Manage-bde: off](manage-bde-off.md)|Descifra la unidad y desactiva BitLocker. Todos los protectores de clave se quitan cuando se completa el descifrado.|
|[Manage-bde: pause](manage-bde-pause.md)|Pausa el cifrado o el descifrado.|
|[Manage-bde: resume](manage-bde-resume.md)|Reanuda el cifrado o el descifrado.|
|[Manage-bde: lock](manage-bde-lock.md)|Impide el acceso a los datos protegidos con BitLocker.|
|[Manage-bde: unlock](manage-bde-unlock.md)|Permite el acceso a los datos protegidos con BitLocker con una contraseña de recuperación o una clave de recuperación.|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|Administra el desbloqueo automático de las unidades de datos.|
|[Manage-bde: protectors](manage-bde-protectors.md)|Administra métodos de protección para la clave de cifrado.|
|[Manage-bde: tpm](manage-bde-tpm.md)|Configura el Módulo de plataforma segura del equipo (TPM). Este comando no se admite en equipos que ejecutan Windows 8 o **win8_server_2**. Para administrar el TPM en estos equipos, use el complemento MMC administración de TPM o los cmdlets de administración de TPM para Windows PowerShell.|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|Establece el campo Identificador de unidad de la unidad en el valor especificado en la configuración **proporcionar los identificadores únicos de la organización** Directiva de grupo.|
|[Manage-bde: ForceRecovery](manage-bde-forcerecovery.md)|Fuerza una unidad protegida por BitLocker en modo de recuperación al reiniciarse. Este comando elimina de la unidad todos los protectores de clave relacionados con TPM. Cuando se reinicia el equipo, solo se puede usar una contraseña de recuperación o una clave de recuperación para desbloquear la unidad.|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|Modifica la contraseña de una unidad de datos.|
|[Manage-bde: changepin](manage-bde-changepin.md)|Modifica el PIN de una unidad del sistema operativo.|
|[Manage-bde: changekey](manage-bde-changekey.md)|Modifica la clave de inicio de una unidad del sistema operativo.|
|[Manage-bde: KeyPackage](manage-bde-keypackage.md)|Genera un paquete de claves para una unidad.|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|Actualiza la versión de BitLocker.|
|[Manage-bde: WipeFreeSpace](manage-bde-wipefreespace.md)|Borra el espacio libre de una unidad.|
|-? o/?|Muestra una breve ayuda en el símbolo del sistema.|
|-Help o-h|Muestra la ayuda completa en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestran las unidades del equipo y se identifica si están protegidas por BitLocker y el estado de cifrado actual.
```
manage-bde -status
```
En el ejemplo siguiente se muestra cómo habilitar BitLocker en la unidad C con la opción de una contraseña de recuperación. BitLocker generará la contraseña de recuperación y se mostrará en la pantalla para que pueda grabarla.
```
manage-bde –on C: -recoverypassword
```
En el ejemplo siguiente se muestra cómo desbloquear una unidad protegida con BitLocker mediante una contraseña de recuperación.
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Habilitar BitLocker mediante la línea de comandos](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
