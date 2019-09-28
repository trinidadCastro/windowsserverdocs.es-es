---
title: repair-bde
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 817e5fb5cf032376ddfddb3a54f73411ac175def
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384553"
---
# <a name="repair-bde"></a>repair-bde



Obtiene acceso a los datos cifrados en un disco duro gravemente dañado si la unidad se cifró mediante BitLocker. Repair-bde puede reconstruir partes críticas de la unidad y rescatar los datos recuperables, siempre que para descifrar los datos se use una clave de recuperación o una contraseña de recuperación válida. Si los metadatos de BitLocker en la unidad se han dañado, debe poder proporcionar un paquete de claves de copia de seguridad además de la contraseña de recuperación o la clave de recuperación. De este paquete de claves se realiza una copia de seguridad en los servicios de dominio de Active Directory (AD DS) si usas la configuración predeterminada para la copia de seguridad de AD DS. Con este paquete de claves y la contraseña de recuperación o la clave de recuperación puede descifrar partes de una unidad protegida con BitLocker si el disco se daña. Cada paquete de claves funcionará solo para una unidad que tenga el identificador de unidad correspondiente. Puede usar el [visor de contraseñas de recuperación de BitLocker para Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) para obtener este paquete de claves de AD DS.

> [!NOTE]
> El visor de contraseñas de recuperación de BitLocker se incluye como una de las características de administración opcionales instalables mediante el administrador de servidores en Windows Server 2012.

Existen las siguientes limitaciones para la herramienta de línea de comandos repair-BDE:
-   Repair-BDE no puede reparar una unidad en la que se produjo un error durante el proceso de cifrado o descifrado.
-   Repair-BDE supone que si la unidad tiene algún cifrado, la unidad se ha cifrado completamente.

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0InputVolume >|Identifica la letra de unidad de la unidad cifrada con BitLocker que desea reparar. La letra de unidad debe incluir un signo de dos puntos. por ejemplo: **C:** .|
|@no__t 0OutputVolumeorImage >|Identifica la unidad en la que se va a almacenar el contenido de la unidad reparada. Se sobrescribirá toda la información de la unidad de salida.|
|-RK|Identifica la ubicación de la clave de recuperación que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-RecoveryKey**.|
|-RP|Identifica la contraseña de recuperación numérica que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-RecoveryPassword**.|
|-PW|Identifica la contraseña que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **-password** .|
|-KP|Identifica el paquete de claves de recuperación que se puede usar para desbloquear el volumen. Este comando también se puede especificar como **-keypackage**.|
|-LF|Especifica la ruta de acceso al archivo que almacenará los mensajes de error, de advertencia y de información de repair-Bde. Este comando también se puede especificar como **-logfile**.|
|-f|Fuerza el desmontaje de un volumen incluso si no se puede bloquear. Este comando también se puede especificar como **Force**.|
|-? o/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Si no se especifica la ruta de acceso a un paquete de claves, **repair-BDE** buscará en la unidad un paquete de claves. Sin embargo, si el disco duro está dañado, **repair-BDE** no podrá encontrar el paquete y le pedirá que proporcione la ruta de acceso.

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se intenta reparar la unidad C y escribir el contenido de la unidad C en la unidad D mediante el archivo de clave de recuperación (RecoveryKey. Bek) almacenado en la unidad F y se escriben los resultados de este intento en el archivo de registro (log. txt) de la unidad Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
En el ejemplo siguiente se intenta reparar la unidad C y escribir el contenido de la unidad C en la unidad D mediante la contraseña de recuperación de 48 dígitos especificada. La contraseña de recuperación debe escribirse en ocho bloques de seis dígitos con un guion separando cada bloque.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
En el ejemplo siguiente se fuerza la desmontaje de la unidad C y, a continuación, se intenta reparar la unidad C y se escribe el contenido de la unidad C en la unidad D mediante el paquete de claves de recuperación y el archivo de clave de recuperación (RecoveryKey. Bek) almacenados en la unidad F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
En el ejemplo siguiente se intenta reparar la unidad C y escribir el contenido de la unidad C en la unidad D, y se debe escribir una contraseña para desbloquear la unidad C: cuando se le solicite:
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)