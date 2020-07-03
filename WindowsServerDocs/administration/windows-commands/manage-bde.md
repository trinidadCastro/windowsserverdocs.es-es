---
title: manage-bde
description: Artículo de referencia para el comando Manage-BDE, que activa o desactiva BitLocker, especifica los mecanismos de desbloqueo, actualiza los métodos de recuperación y desbloquea las unidades de datos protegidas por BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 480f44c6467587918ad347413315b208c874f8cd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922129"
---
# <a name="manage-bde"></a>manage-bde

Activa o desactiva BitLocker, especifica los mecanismos de desbloqueo, actualiza los métodos de recuperación y desbloquea las unidades de datos protegidas por BitLocker.

> [!NOTE]
> Esta herramienta de línea de comandos se puede usar en lugar de la **cifrado de unidad BitLocker** elemento del panel de control.

## <a name="syntax"></a>Sintaxis

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm]
[–setidentifier] [-forcerecovery] [–changepassword] [–changepin] [–changekey] [-keypackage] [–upgrade] [-wipefreespace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| [administrar: estado de BDE](manage-bde-status.md) | Proporciona información acerca de todas las unidades del equipo, tanto si están protegidas por BitLocker como si no. |
| [Manage-BDE en](manage-bde-on.md) | Cifra la unidad y activa BitLocker. |
| [Manage-BDE desactivado](manage-bde-off.md) | Descifra la unidad y desactiva BitLocker. Todos los protectores de clave se quitan cuando se completa el descifrado. |
| [la pausa de Manage-BDE](manage-bde-pause.md) | Pausa el cifrado o el descifrado. |
| [resume de Manage-BDE](manage-bde-resume.md) | Reanuda el cifrado o el descifrado. |
| [Manage-BDE (bloqueo)](manage-bde-lock.md) | Impide el acceso a los datos protegidos con BitLocker. |
| [administrar: desbloqueo de BDE](manage-bde-unlock.md) | Permite el acceso a los datos protegidos con BitLocker con una contraseña de recuperación o una clave de recuperación. |
| [Manage-BDE AUTOLOCK](manage-bde-autounlock.md) | Administra el desbloqueo automático de las unidades de datos. |
| [protectores de Manage-BDE](manage-bde-protectors.md) | Administra métodos de protección para la clave de cifrado. |
| [Manage-BDE TPM](manage-bde-tpm.md) | Configura el Módulo de plataforma segura del equipo (TPM). Este comando no se admite en equipos que ejecutan Windows 8 o **win8_server_2**. Para administrar el TPM en estos equipos, use el complemento MMC administración de TPM o los cmdlets de administración de TPM para Windows PowerShell. |
| [Manage-BDE setidentifier](manage-bde-setidentifier.md)   | Establece el campo Identificador de unidad de la unidad en el valor especificado en la configuración **proporcionar los identificadores únicos de la organización** Directiva de grupo. |
| [Manage-BDE ForceRecovery](manage-bde-forcerecovery.md) | Fuerza una unidad protegida por BitLocker en modo de recuperación al reiniciarse. Este comando elimina de la unidad todos los protectores de clave relacionados con TPM. Cuando se reinicia el equipo, solo se puede usar una contraseña de recuperación o una clave de recuperación para desbloquear la unidad. |
| [Manage-BDE ChangePassword](manage-bde-changepassword.md) | Modifica la contraseña de una unidad de datos. |
| [Manage-BDE changepin](manage-bde-changepin.md) | Modifica el PIN de una unidad del sistema operativo. |
| [Manage-BDE changekey](manage-bde-changekey.md) | Modifica la clave de inicio de una unidad del sistema operativo. |
| [Manage-BDE KeyPackage](manage-bde-keypackage.md) | Genera un paquete de claves para una unidad. |
| [actualización de Manage-BDE](manage-bde-upgrade.md) | Actualiza la versión de BitLocker. |
| [Manage-BDE WipeFreeSpace](manage-bde-wipefreespace.md) | Borra el espacio libre de una unidad. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Habilitar BitLocker mediante la línea de comandos](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)
