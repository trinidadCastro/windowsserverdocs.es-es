---
title: repair-bde
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9e2bce0c0a0f12a3a171c161a669c903044e8b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813546"
---
# <a name="repair-bde"></a>repair-bde



Tiene acceso a datos cifrados en un disco duro dañado seriamente si la unidad se cifró mediante el uso de BitLocker. Repair-bde puede reconstruir partes críticas de la unidad y rescatar los datos recuperables, siempre que para descifrar los datos se use una clave de recuperación o una contraseña de recuperación válida. Si los metadatos de BitLocker en la unidad se han dañado, debe poder proporcionar un paquete de claves de copia de seguridad además de la contraseña de recuperación o la clave de recuperación. De este paquete de claves se realiza una copia de seguridad en los servicios de dominio de Active Directory (AD DS) si usas la configuración predeterminada para la copia de seguridad de AD DS. Con este paquete de claves y la contraseña de recuperación o la clave de recuperación puede descifrar partes de una unidad protegida con BitLocker si el disco se daña. Cada paquete de claves funcionará solo para una unidad que tenga el identificador de unidad correspondiente. Puede usar el [Visor de contraseñas de recuperación de BitLocker para Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) para obtener este paquete de claves de AD DS.

> [!NOTE]
> El Visor de contraseñas de recuperación de BitLocker se incluye como una de las características de administración opcional instalables con el servidor de administración en Windows Server 2012.

Para la herramienta de línea de comandos Repair-bde, existen las siguientes limitaciones:
-   Repair-bde no puede reparar una unidad que no se pudo durante el proceso de cifrado o descifrado.
-   Repair-bde supone que si la unidad tiene una, a continuación, la unidad se ha totalmente cifrada.

Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<InputVolume>|Identifica la letra de unidad de la unidad cifrada con BitLocker que desea reparar. La letra de unidad debe incluir un signo de dos puntos; Por ejemplo: **C:**.|
|\<OutputVolumeorImage>|Identifica la unidad en la que se va a almacenar el contenido de la unidad reparada. Se sobrescribirá toda la información en la unidad de salida.|
|-rk|Identifica la ubicación de la clave de recuperación que debe usarse para desbloquear el volumen. Este comando también se puede especificar como **- recoverykey**.|
|-rp|Identifica la contraseña de recuperación numérica que se debe usar para desbloquear el volumen. Este comando también se puede especificar como **- recoverypassword**.|
|-pw|Identifica la contraseña que debe usarse para desbloquear el volumen. Este comando también se puede especificar como **-contraseña**|
|-kp|Identifica el paquete de claves de recuperación que se puede usar para desbloquear el volumen. Este comando también se puede especificar como **- keypackage**.|
|-lf|Especifica la ruta de acceso al archivo donde se almacenará los mensajes de información, advertencia y error Repair-bde. Este comando también se puede especificar como **- logfile**.|
|-f|Fuerza un volumen desmontado incluso si no se puede bloquear. Este comando también se puede especificar como **-forzar**.|
|-? ¿o /?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Si no se especifica la ruta de acceso a un paquete de claves, **repair-bde** buscará en la unidad para un paquete de claves. Sin embargo, si se ha dañado el disco duro, **repair-bde** puede no ser capaz de encontrar el paquete y le solicitará que proporcione la ruta de acceso.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente se intenta reparar la unidad C y escribe el contenido de la unidad C en la unidad D con el archivo de clave de recuperación (RecoveryKey.bek) almacenado en la unidad F y escribe los resultados de este intento en el archivo de registro (log.txt) en la unidad Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
El ejemplo siguiente se intenta reparar la unidad C y escribe el contenido en la unidad C en la unidad D con la contraseña de recuperación de 48 dígitos especificada. La contraseña de recuperación debe escribirse en ocho bloques de seis dígitos con un guión separa cada bloque.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
El ejemplo siguiente fuerza la unidad C para desmontar y, a continuación, intenta reparar la unidad C y escribe el contenido en la unidad C en la unidad D con el paquete de claves de recuperación y archivo de clave de recuperación (RecoveryKey.bek) almacenado en la unidad f el.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
El ejemplo siguiente se intenta reparar la unidad C y escribe el contenido de la unidad C en la unidad D y debe escribir una contraseña para desbloquear la unidad C:, cuando se le solicite:
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)