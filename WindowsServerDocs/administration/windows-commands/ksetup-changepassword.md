---
title: 'ksetup: ChangePassword'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51be9e71c2b290e6346d23144543e0eec29f9d07
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375179"
---
# <a name="ksetupchangepassword"></a>ksetup: ChangePassword



Usa el valor de la contraseña del Centro de distribución de claves (KDC) (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<OldPasswd >|Indica la contraseña existente del usuario que ha iniciado sesión.|
|\<NewPasswd >|Indica la nueva contraseña del usuario que ha iniciado sesión.|

## <a name="remarks"></a>Observaciones

Este comando usa el valor de la contraseña de KDC (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión. El Kpasswd, si se establece, se muestra en la salida ejecutando el comando **ksetup/dumpstate** .

La nueva contraseña del usuario debe cumplir todos los requisitos de contraseña que se establezcan en este equipo.

Si la cuenta de usuario no se encuentra en el dominio actual, el sistema le pedirá que proporcione el nombre de dominio en el que reside la cuenta de usuario.

Si desea forzar un cambio de contraseña en el siguiente inicio de sesión, este comando permite el uso del asterisco (*), por lo que se solicitará al usuario una nueva contraseña.

La salida del comando le informa del estado de éxito o de error.

## <a name="BKMK_Examples"></a>Example

Cambiar la contraseña de un usuario que ha iniciado sesión actualmente en este equipo en este dominio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Cambiar la contraseña de un usuario que ha iniciado sesión actualmente en el dominio contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Forzar al usuario que ha iniciado sesión actualmente a cambiar la contraseña en el siguiente inicio de sesión:
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)