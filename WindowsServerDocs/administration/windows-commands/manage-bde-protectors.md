---
title: protectores de Manage-BDE
description: Tema de referencia para el comando Manage-BDE protecters, que administra los métodos de protección usados para la clave de cifrado de BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 999c92fd9f2bfedad92a9c68c1528ee66836f315
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222624"
---
# <a name="manage-bde-protectors"></a>protectores de Manage-BDE

> Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Administra los métodos de protección usados para la clave de cifrado de BitLocker.

## <a name="syntax"></a>Sintaxis

```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ----------- | ----------- |
| -obtener | Muestra todos los métodos de protección de claves habilitados en la unidad y proporciona su tipo e identificador (ID.). |
| -Agregar | Agrega métodos de protección de claves como se especifica mediante **el uso de** parámetros adicionales. |
| -delete | Elimina los métodos de protección de claves que usa BitLocker. Todos los protectores de clave se quitarán de una unidad a menos que se utilicen los parámetros opcionales **-Delete** para especificar qué protectores se deben eliminar. Cuando se elimina el último protector de una unidad, la protección de BitLocker de la unidad se deshabilita para garantizar que el acceso a los datos no se pierde accidentalmente. |
| -disable | Deshabilita la protección, lo que permitirá a los usuarios tener acceso a los datos cifrados al hacer que la clave de cifrado esté disponible sin protección en la unidad. No se quita ningún protector de clave. La protección se reanudará la próxima vez que se arranque Windows, a menos que se usen los parámetros opcionales **-Disable** para especificar el recuento de reinicios. |
| -enable | Habilita la protección quitando la clave de cifrado no segura de la unidad. Se aplicarán todos los protectores de clave configurados en la unidad. |
| -adbackup | Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada en Active Directory Domain Services (AD DS). Para realizar una copia de seguridad de una sola clave de recuperación en AD DS, Anexe el parámetro **-ID** y especifique el ID. de una clave de recuperación específica para realizar la copia de seguridad. |
| -aadbackup | Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada en Azure Active Directory (Azure AD). Para realizar una copia de seguridad de una sola clave de recuperación en Azure AD, Anexe el parámetro **-ID** y especifique el ID. de una clave de recuperación específica para realizar la copia de seguridad. |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -COMPUTERNAME | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

#### <a name="additional--add-parameters"></a>Agregar parámetros adicionales

El parámetro-Add también puede usar estos parámetros adicionales válidos.

```
manage-bde -protectors -add [<drive>] [-forceupgrade] [-recoverypassword <numericalpassword>] [-recoverykey <pathtoexternalkeydirectory>]
[-startupkey <pathtoexternalkeydirectory>] [-certificate {-cf <pathtocertificatefile>|-ct <certificatethumbprint>}] [-tpm] [-tpmandpin]
[-tpmandstartupkey <pathtoexternalkeydirectory>] [-tpmandpinandstartupkey <pathtoexternalkeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <name>]
[{-?|/?}] [{-help|-h}]
```

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -RecoveryPassword | Agrega un protector de contraseña numérica. También puede usar **-RP** como una versión abreviada de este comando. |
| `<numericalpassword>` | Representa la contraseña de recuperación. |
| -recoverykey | Agrega un protector de clave externa para la recuperación. También puede usar **-RK** como una versión abreviada de este comando. |
| `<pathtoexternalkeydirectory>` | Representa la ruta de acceso del directorio a la clave de recuperación. |
| -clave | Agrega un protector de clave externa para el inicio. También puede usar **-SK** como una versión abreviada de este comando. |
| `<pathtoexternalkeydirectory>` | Representa la ruta de acceso al directorio de la clave de inicio. |
| -certificado | Agrega un protector de clave pública para una unidad de datos. También puede usar **-CERT** como una versión abreviada de este comando. |
| -cf | Especifica que se usará un archivo de certificado para proporcionar el certificado de clave pública. |
| <pathtocertificatefile> | Representa la ruta de acceso del directorio al archivo de certificado. |
| -CT | Especifica que se usará una huella digital de certificado para identificar el certificado de clave pública. |
| `<certificatethumbprint>` | Especifica el valor de la propiedad huella digital del certificado que desea utilizar. Por ejemplo, un valor de huella digital de certificado de A9 09 50 2D D8 2A E4 14 33 E6 F8 38 86 B0 0d 42 77 a3 2A 7B debe especificarse como a909502dd82ae41433e6f83886b00d4277a32a7b. |
| -tpmandpin | Agrega un protector de Módulo de plataforma segura (TPM) y un número de identificación personal (PIN) para la unidad del sistema operativo. También puede usar **-TP** como una versión abreviada de este comando. |
| -tpmandstartupkey | Agrega un protector de clave de inicio y TPM para la unidad del sistema operativo. También puede usar **-TSK** como una versión abreviada de este comando. |
| -tpmandpinandstartupkey | Agrega un protector de TPM, PIN y clave de inicio para la unidad del sistema operativo. También puede usar **-tpsk** como una versión abreviada de este comando. |
| -password | Agrega un protector de clave de contraseña para la unidad de datos. También puede usar **-PW** como una versión abreviada de este comando. |
| -adaccountorgroup | Agrega un protector de identidad basado en el identificador de seguridad (SID) para el volumen. También puede usar **-SID** como una versión abreviada de este comando. **Importante:** De forma predeterminada, no se puede Agregar un protector de ADaccountorgroup de forma remota mediante WMI o Manage-Bde. Si la implementación requiere la capacidad de agregar este protector de forma remota, debe habilitar la delegación restringida. |
| -COMPUTERNAME | Especifica que Manage-BDE se usa para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

#### <a name="additional--delete-parameters"></a>Parámetros adicionales: Delete

```
manage-bde -protectors -delete <drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}]
[-id <keyprotectorID>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| -Type | Identifica el protector de clave que se va a eliminar. También puede usar **-t** como una versión abreviada de este comando. |
| RecoveryPassword | Especifica que se deben eliminar los protectores de clave de la contraseña de recuperación. |
| externalkey | Especifica que se debe eliminar cualquier protector de clave externa asociado a la unidad. |
| certificado | Especifica que se deben eliminar todos los protectores de clave de certificado asociados con la unidad. |
| tpm | Especifica que se deben eliminar todos los protectores de clave solo TPM asociados a la unidad. |
| tpmandstartupkey | Especifica que se deben eliminar todos los protectores de clave basados en el TPM y la clave de inicio asociados a la unidad. |
| tpmandpin | Especifica que se deben eliminar los protectores de clave basados en TPM y en el PIN asociados a la unidad. |
| tpmandpinandstartupkey | Especifica que se deben eliminar los protectores de clave basados en el TPM, el PIN y la clave de inicio asociados con la unidad. |
| password | Especifica que se deben eliminar todos los protectores de clave de contraseña asociados a la unidad. |
| identidad | Especifica que se deben eliminar todos los protectores de clave de identidad asociados a la unidad. |
| -ID | Identifica el protector de clave que se va a eliminar mediante el identificador de clave. Este parámetro es una opción alternativa al parámetro **-Type** . |
| `<keyprotectorID>` | Identifica un protector de clave individual en la unidad que se va a eliminar. Los identificadores de protector de clave se pueden mostrar con el comando **Manage-BDE-protectors-Get** . |
| -COMPUTERNAME | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

#### <a name="additional--disable-parameters"></a>Parámetros adicionales: deshabilitar

```
manage-bde -protectors -disable <drive> [-rebootcount <integer 0 - 15>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>` | Representa la letra de una unidad seguida del signo de dos puntos. |
| rebootcount | Especifica que se ha suspendido la protección del volumen del sistema operativo y se reanudará después de que se haya reiniciado Windows el número de veces especificado en el parámetro **rebootcount** . Especifique **0** para suspender la protección indefinidamente. Si no se especifica este parámetro, la protección de BitLocker se reanudará automáticamente cuando se reinicie Windows. También puede usar **-RC** como una versión abreviada de este comando. |
| -COMPUTERNAME | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
| `<name>` | Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo. |
| -? o/? | Muestra una breve ayuda en el símbolo del sistema. |
| -Help o-h | Muestra la ayuda completa en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para agregar un protector de clave de certificado, que se identifica mediante un archivo de certificado, en la unidad E, escriba:

```
manage-bde -protectors -add E: -certificate -cf c:\File Folder\Filename.cer
```

Para agregar un protector de clave **adaccountorgroup** , identificado por el dominio y el nombre de usuario, a la unidad E, escriba:

```
manage-bde -protectors -add E: -sid DOMAIN\user
```

Para deshabilitar la protección hasta que el equipo se haya reiniciado 3 veces, escriba:

```
manage-bde -protectors -disable C: -rc 3
```

Para eliminar todos los protectores de clave basados en TPM y en claves de inicio en la unidad C, escriba:

```
manage-bde -protectors -delete C: -type tpmandstartupkey
```

Para realizar una copia de seguridad de toda la información de recuperación de la unidad C en AD DS, escriba:

```
manage-bde -protectors -adbackup C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Manage-BDE](manage-bde.md)
