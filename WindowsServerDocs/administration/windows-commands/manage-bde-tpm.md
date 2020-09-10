---
title: Manage-BDE TPM
description: Artículo de referencia para el comando Manage-BDE TPM, que configura el Módulo de plataforma segura del equipo (TPM).
ms.topic: reference
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d108deffb12048241704e643574698d25ac60a06
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635259"
---
# <a name="manage-bde-tpm"></a>Manage-BDE TPM

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura el Módulo de plataforma segura del equipo (TPM).

## <a name="syntax"></a>Sintaxis

```
manage-bde -tpm [-turnon] [-takeownership <ownerpassword>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -activando | Habilita y activa el TPM, lo que permite establecer la contraseña de propietario de TPM. También puede usar **-t** como una versión abreviada de este comando. |
| -takeownership | Toma posesión del TPM mediante el establecimiento de una contraseña de propietario. También puede usar **-o** como una versión abreviada de este comando. |
| `<ownerpassword>` | Representa la contraseña de propietario que se especifica para el TPM. |
| -COMPUTERNAME | Especifica que se utilizará manage-bde.exe para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para activar el TPM, escriba:

```
manage-bde  tpm -turnon
```

Para tomar posesión del TPM y establecer la contraseña del propietario en *0wnerP@ss* , escriba:

```
manage-bde  tpm  takeownership 0wnerP@ss
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de administración de TPM para Windows PowerShell](/powershell/module/trustedplatformmodule/)

- [comando Manage-BDE](manage-bde.md)
