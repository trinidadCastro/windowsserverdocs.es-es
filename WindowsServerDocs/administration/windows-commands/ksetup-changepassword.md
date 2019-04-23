---
title: ksetup:changepassword
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f629c6c7930777583df38f5af900ed380ec60f9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878536"
---
# <a name="ksetupchangepassword"></a>ksetup:changepassword



Usa el valor de contraseña (kpasswd) del centro de distribución de claves (KDC) para cambiar la contraseña del usuario que ha iniciado sesión. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /changepassword <OldPasswd> <NewPasswd>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<OldPasswd>|Indica la contraseña existente del usuario que ha iniciado sesión.|
|\<NewPasswd>|Indica el inicio de sesión en la nueva contraseña del usuario.|

## <a name="remarks"></a>Comentarios

Este comando usa el valor de contraseña (kpasswd) de KDC para cambiar la contraseña del usuario que ha iniciado sesión. El kpasswd, si establece, se muestra en la salida mediante la ejecución de la **ksetup /dumpstate** comando.

Nueva contraseña del usuario debe cumplir todos los requisitos de contraseña que se establecen en este equipo.

Si no se encuentra la cuenta de usuario en el dominio actual, el sistema le pedirá que proporcione el nombre de dominio donde reside la cuenta de usuario.

Si desea forzar un cambio de contraseña en próximo inicio de sesión, este comando permite el uso del asterisco (*), por lo que se pedirá al usuario una contraseña nueva.

La salida del comando informa del estado de éxito o error.

## <a name="BKMK_Examples"></a>Ejemplos

Cambiar la contraseña de un usuario que ha iniciado sesión actualmente en este equipo en este dominio:
```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```
Cambiar la contraseña de un usuario que actualmente ha iniciado sesión en el dominio Contoso:
```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```
Hacer que el usuario ha iniciado sesión actualmente para cambiar la contraseña en el siguiente inicio de sesión:
```
ksetup /changepassword Pas$w0rd *
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)