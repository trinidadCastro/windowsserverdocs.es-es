---
title: 'administrar: desbloqueo de BDE'
description: Artículo de referencia para el comando Manage-BDE Unlock, que desbloquea una unidad protegida con BitLocker mediante una contraseña de recuperación o una clave de recuperación.
ms.topic: reference
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 23b7a593b450265b2547acd6fea0bbe8241e993b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635240"
---
# <a name="manage-bde-unlock"></a>administrar: desbloqueo de BDE

Desbloquea una unidad protegida con BitLocker mediante una contraseña de recuperación o una clave de recuperación.

## <a name="syntax"></a>Sintaxis

```
manage-bde -unlock {-recoverypassword <password>|-recoverykey <pathtoexternalkeyfile>} <drive> [-certificate {-cf pathtocertificatefile | -ct certificatethumbprint} {-pin}] [-password] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -RecoveryPassword | Especifica que se usará una contraseña de recuperación para desbloquear la unidad. También puede usar **-RP** como una versión abreviada de este comando. |
| `<password>` | Representa la contraseña de recuperación que se puede usar para desbloquear la unidad. |
| -recoverykey | Especifica que se usará un archivo de clave de recuperación externa para desbloquear la unidad. También puede usar **-RK** como una versión abreviada de este comando. |
| `<pathtoexternalkeyfile>` | Representa el archivo de clave de recuperación externa que se puede usar para desbloquear la unidad. |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -certificado | El certificado de usuario local de un certificado de BitLocker para desbloquear el volumen se encuentra en el almacén de certificados del usuario local. También puede usar **-CERT** como una versión abreviada de este comando. |
| -CF `<pathtocertificatefile>` | Ruta de acceso al archivo de certificado. |
| -CT `<certificatethumbprint>` | Huella digital del certificado que puede incluir opcionalmente el PIN (-PIN). |
| -password | Presenta un símbolo del sistema para la contraseña para desbloquear el volumen. También puede usar **-PW** como una versión abreviada de este comando. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para desbloquear la unidad E con un archivo de clave de recuperación que se ha guardado en una carpeta de copia de seguridad de otra unidad, escriba:

```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
