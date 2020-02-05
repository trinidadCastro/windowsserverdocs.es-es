---
title: protectores de Manage-BDE
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 86e170e199c7286d883f1248610c6f195add5b01
ms.sourcegitcommit: a33404f92867089bb9b0defcd50960ff231eef3f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013040"
---
# <a name="manage-bde-protectors"></a>Manage-BDE: protectores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Administra los métodos de protección usados para la clave de cifrado de BitLocker. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxis
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                                                                                           Descripción                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -obtener      |                                                                                                                                            Muestra todos los métodos de protección de claves habilitados en la unidad y proporciona su tipo e identificador (ID.).                                                                                                                                             |
|     -Agregar      |                                                                                                                                   agrega métodos de protección de claves tal y como se especifica mediante la [adición de parámetros](manage-bde-protectors.md#BKMK_addprotectors)adicionales.                                                                                                                                    |
|    -eliminar    | elimina los métodos de protección de claves que usa BitLocker. Todos los protectores de clave se quitarán de una unidad a menos que se utilicen los [parámetros opcionales-Delete](manage-bde-protectors.md#BKMK_deleteprotectors) para especificar qué protectores se deben eliminar. Cuando se elimina el último protector de una unidad, la protección de BitLocker de la unidad se deshabilita para garantizar que el acceso a los datos no se pierde accidentalmente. |
|   -Disable    |                      Deshabilita la protección, lo que permitirá a los usuarios tener acceso a los datos cifrados al hacer que la clave de cifrado esté disponible sin protección en la unidad. No se quita ningún protector de clave. La protección se reanudará la próxima vez que se arranque Windows, a menos que se usen los [parámetros opcionales-Disable](manage-bde-protectors.md#BKMK_disableprot) para especificar el recuento de reinicios.                       |
|    -habilitar    |                                                                                                                             Habilita la protección quitando la clave de cifrado no segura de la unidad. Se aplicarán todos los protectores de clave configurados en la unidad.                                                                                                                             |
|   -adbackup   |                                                                          Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada en Active Directory Domain Services (AD DS). Para realizar una copia de seguridad de una sola clave de recuperación en AD DS, Anexe el parámetro **-ID** y especifique el ID. de una clave de recuperación específica para realizar la copia de seguridad.                                                                           |
|  -aadbackup   |                                                                            Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada en Azure Active Directory (Azure ad). Para realizar una copia de seguridad de una sola clave de recuperación en Azure AD, Anexe el parámetro **-ID** y especifique el ID. de una clave de recuperación específica para realizar la copia de seguridad.                                                                             |
|    <Drive>    |                                                                                                                                                                          Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                          |
| -COMPUTERNAME |                                                                                                              Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.                                                                                                              |
|    <Name>     |                                                                                                                 Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.                                                                                                                  |
|   -? o/?    |                                                                                                                                                                            Muestra una breve ayuda en el símbolo del sistema.                                                                                                                                                                            |
|  -Help o-h  |                                                                                                                                                                          Muestra la ayuda completa en el símbolo del sistema.                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-Agregar sintaxis y parámetros
```
manage-bde  -protectors  -add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          Parámetro           |                                                                                                                                                                                   Descripción                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                  |
|      -RecoveryPassword       |                                                                                                                                    agrega un protector de contraseña numérica. También puede usar **-RP** como una versión abreviada de este comando.                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        Representa la contraseña de recuperación.                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                agrega un protector de clave externa para la recuperación. También puede usar **-RK** como una versión abreviada de este comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               Representa la ruta de acceso del directorio a la clave de recuperación.                                                                                                                                                                |
|         -clave          |                                                                                                                                 agrega un protector de clave externa para el inicio. También puede usar **-SK** como una versión abreviada de este comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                Representa la ruta de acceso al directorio de la clave de inicio.                                                                                                                                                                |
|         -certificado         |                                                                                                                               agrega un protector de clave pública para una unidad de datos. También puede usar **-CERT** como una versión abreviada de este comando.                                                                                                                               |
|             -CF              |                                                                                                                                              Especifica que se usará un archivo de certificado para proporcionar el certificado de clave pública.                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             Representa la ruta de acceso del directorio al archivo de certificado.                                                                                                                                                              |
|             -CT              |                                                                                                                                           Especifica que se usará una huella digital de certificado para identificar el certificado de clave pública.                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       Especifica el valor de la propiedad huella digital del certificado que desea utilizar. Por ejemplo, un valor de huella digital de certificado de "A9 09 50 2D D8 2A E4 14 33 E6 F8 38 86 B0 0d 42 77 a3 2A 7B" se debe especificar como "a909502dd82ae41433e6f83886b00d4277a32a7b".                                                        |
|          -tpmandpin          |                                                                                           agrega un protector de Módulo de plataforma segura (TPM) y un número de identificación personal (PIN) para la unidad del sistema operativo. También puede usar **-TP** como una versión abreviada de este comando.                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    agrega un protector de clave de inicio y TPM para la unidad del sistema operativo. También puede usar **-TSK** como una versión abreviada de este comando.                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                agrega un protector de TPM, PIN y clave de inicio para la unidad del sistema operativo. También puede usar **-tpsk** como una versión abreviada de este comando.                                                                                                                 |
|          -contraseña           |                                                                                                                              agrega un protector de clave de contraseña para la unidad de datos. También puede usar **-PW** como una versión abreviada de este comando.                                                                                                                              |
|      -adaccountorgroup       | agrega un protector de identidad basado en el identificador de seguridad (SID) para el volumen.  También puede usar **-SID** como una versión abreviada de este comando. **Importante:** De forma predeterminada, no se puede Agregar un protector de ADAccountOrGroup de forma remota mediante WMI o Manage-Bde.  Si la implementación requiere la capacidad de agregar este protector de forma remota, debe habilitar la delegación restringida. |
|        -COMPUTERNAME         |                                                                                                       Especifica que Manage-BDE se usa para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.                                                                                                       |
|            <Name>            |                                                                                                         Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-eliminar sintaxis y parámetros
```
manage-bde  -protectors  -delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       Parámetro        |                                                                              Descripción                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             Representa la letra de una unidad seguida del signo de dos puntos.                                                             |
|         -Type          |                               Identifica el protector de clave que se va a eliminar. También puede usar **-t** como una versión abreviada de este comando.                               |
|    RecoveryPassword    |                                                 Especifica que se deben eliminar los protectores de clave de la contraseña de recuperación.                                                 |
|      externalkey       |                                        Especifica que se debe eliminar cualquier protector de clave externa asociado a la unidad.                                         |
|      certificado       |                                       Especifica que se deben eliminar todos los protectores de clave de certificado asociados con la unidad.                                       |
|          TPM           |                                        Especifica que se deben eliminar todos los protectores de clave solo TPM asociados a la unidad.                                         |
|    tpmandstartupkey    |                                Especifica que se deben eliminar todos los protectores de clave basados en el TPM y la clave de inicio asociados a la unidad.                                |
|       tpmandpin        |                                    Especifica que se deben eliminar los protectores de clave basados en TPM y en el PIN asociados a la unidad.                                    |
| tpmandpinandstartupkey |                             Especifica que se deben eliminar los protectores de clave basados en el TPM, el PIN y la clave de inicio asociados con la unidad.                             |
|        contraseña        |                                        Especifica que se deben eliminar todos los protectores de clave de contraseña asociados a la unidad.                                         |
|        aut.        |                                        Especifica que se deben eliminar todos los protectores de clave de identidad asociados a la unidad.                                         |
|          identificador de           |                Identifica el protector de clave que se va a eliminar mediante el identificador de clave. Este parámetro es una opción alternativa al parámetro **-Type** .                 |
|    <KeyProtectorID>    |        Identifica un protector de clave individual en la unidad que se va a eliminar. Los identificadores de protector de clave se pueden mostrar con el comando **Manage-BDE-protectors-Get** .         |
|     -COMPUTERNAME      | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
|         <Name>         |    Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.     |
|        -? o/?        |                                                               Muestra una breve ayuda en el símbolo del sistema.                                                               |
|      -Help o-h       |                                                             Muestra la ayuda completa en el símbolo del sistema.                                                              |

### <a name="BKMK_disableprot"></a>-deshabilitar la sintaxis y los parámetros
```
manage-bde  -protectors  -disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   Parámetro   |                                                                                                                                                                                                                   Descripción                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                                                  |
|  RebootCount  | A partir de Windows 8, especifica que la protección del volumen del sistema operativo se ha suspendido y se reanudará después de que se haya reiniciado Windows el número de veces especificado en el parámetro RebootCount. Especifique 0 para suspender la protección indefinidamente. Si no se especifica este parámetro, la protección de BitLocker se reanudará automáticamente cuando se reinicie Windows. También puede usar **-RC** como una versión abreviada de este comando. |
| -COMPUTERNAME |                                                                                                                                      Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando.                                                                                                                                      |
|    <Name>     |                                                                                                                                         Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.                                                                                                                                          |
|   -? o/?    |                                                                                                                                                                                                    Muestra una breve ayuda en el símbolo del sistema.                                                                                                                                                                                                    |
|  -Help o-h  |                                                                                                                                                                                                  Muestra la ayuda completa en el símbolo del sistema.                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>Example
En el ejemplo siguiente se muestra el uso del comando **-protecters** para agregar un protector de clave de certificado identificado por un archivo de certificado a la unidad E.
```
manage-bde  -protectors  -add E: -certificate  -cf "c:\File Folder\Filename.cer"
```
En el ejemplo siguiente se muestra el uso del comando **-protecters** para agregar un protector de clave **adaccountorgroup** identificado por el dominio y el nombre de usuario a la unidad E.
```
manage-bde  -protectors  -add E: -sid DOMAIN\user
```
En el siguiente ejemplo se muestra el uso del comando **protectores** para deshabilitar la protección hasta que el equipo se haya reiniciado 3 veces.
```
manage-bde  -protectors  -disable C: -rc 3
```
En el ejemplo siguiente se muestra el uso del comando **-protecters** para eliminar todos los protectores de clave basados en el TPM y en la clave de inicio de la unidad C.
```
manage-bde  -protectors -delete C: -type tpmandstartupkey
```
En el ejemplo siguiente se muestra el uso del comando **-protecters** para realizar una copia de seguridad de toda la información de recuperación de la unidad C en AD DS.
```
manage-bde  -protectors  -adbackup C:
```
## <a name="additional-references"></a>referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
