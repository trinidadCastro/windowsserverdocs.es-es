---
title: Manage-BDE TPM
description: Tema de referencia sobre el comando Manage-BDE TPM, que configura el Módulo de plataforma segura del equipo (TPM).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3aa597dbd871c64efc7e718ef70ed0c69b256ab9
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222141"
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
| -COMPUTERNAME | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
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

- [Cmdlets de administración de TPM para Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/)

- [comando Manage-BDE](manage-bde.md)
