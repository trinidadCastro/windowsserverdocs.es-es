---
title: 'ksetup: ChangePassword'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68b14388ff3c33458873b494c8d5a770b44f7545
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841788"
---
# <a name="ksetupchangepassword"></a>ksetup: ChangePassword



Usa el valor de la contraseña del Centro de distribución de claves (KDC) (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<OldPasswd >|Indica la contraseña existente del usuario que ha iniciado sesión.|
|\<NewPasswd >|Indica la nueva contraseña del usuario que ha iniciado sesión.|

## <a name="remarks"></a>Comentarios

Este comando usa el valor de la contraseña de KDC (Kpasswd) para cambiar la contraseña del usuario que ha iniciado sesión. El Kpasswd, si se establece, se muestra en la salida ejecutando el comando **ksetup/dumpstate** .

La nueva contraseña del usuario debe cumplir todos los requisitos de contraseña que se establezcan en este equipo.

Si la cuenta de usuario no se encuentra en el dominio actual, el sistema le pedirá que proporcione el nombre de dominio en el que reside la cuenta de usuario.

Si desea forzar un cambio de contraseña en el siguiente inicio de sesión, este comando permite el uso del asterisco (*), por lo que se solicitará al usuario una nueva contraseña.

La salida del comando le informa del estado de éxito o de error.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

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

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)