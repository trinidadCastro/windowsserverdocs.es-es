---
title: ¿Manage-bde protectores
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78c0b338aa34684cc3c5da5ae4493f3526537ee6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437498"
---
# <a name="manage-bde-protectors"></a>¿Manage-bde: protectores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Administra los métodos de protección que se usan para la clave de cifrado de BitLocker. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxis
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                                                                                           Descripción                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -get      |                                                                                                                                            Muestra todos los métodos de protección de claves habilitados en la unidad y proporciona su tipo y el identificador (ID).                                                                                                                                             |
|     -add      |                                                                                                                                   Agrega métodos de protección de claves como se especifica mediante el uso adicionales [agregar parámetros](manage-bde-protectors.md#BKMK_addprotectors).                                                                                                                                    |
|    -delete    | Elimina los métodos de protección de claves utilizados por BitLocker. Todos los protectores de clave se quitará una unidad a menos que el elemento opcional [-eliminar parámetros](manage-bde-protectors.md#BKMK_deleteprotectors) se usan para especificar qué protectores para eliminar. Cuando se elimina el último protector en una unidad, la protección de BitLocker de la unidad está deshabilitada para asegurarse de que el acceso a datos no se pierden accidentalmente. |
|   -deshabilitar    |                      Deshabilita la protección, lo que permitirá que nadie tenga acceso a datos cifrados mediante la realización de la clave de cifrado disponibles no seguros en la unidad. No se quita ningún protector de clave. Protección se reanudará la próxima vez que se arranca Windows, a menos que el elemento opcional [-deshabilitar parámetros](manage-bde-protectors.md#BKMK_disableprot) se usan para especificar el recuento de reinicio.                       |
|    -habilitar    |                                                                                                                             Habilita la protección mediante la eliminación de la clave de cifrado no segura de la unidad. Se aplicarán todos los protectores de clave configurados en la unidad.                                                                                                                             |
|   -adbackup   |                                                                          Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada a los servicios de dominio de Active Directory (AD DS). Para copias de seguridad únicamente una clave de recuperación única en AD DS, anexe el **-id** parámetro y especifique el identificador de una clave de recuperación específico para realizar copias de seguridad.                                                                           |
|  -aadbackup   |                                                                            Realiza una copia de seguridad de toda la información de recuperación de la unidad especificada a Azure Active Directory (Azure Ad). Para la copia de seguridad únicamente una clave de recuperación única en Azure AD, anexe el **-id** parámetro y especifique el identificador de una clave de recuperación específico para realizar copias de seguridad.                                                                             |
|    <Drive>    |                                                                                                                                                                          Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                          |
| -computername |                                                                                                              Especifica que se usará ese administrar-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.                                                                                                              |
|    <Name>     |                                                                                                                 Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.                                                                                                                  |
|   -? ¿o /?    |                                                                                                                                                                            Muestra una breve ayuda en el símbolo del sistema.                                                                                                                                                                            |
|  -help o -h  |                                                                                                                                                                          Muestra Ayuda en la línea de comandos completa.                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-Agregar sintaxis y parámetros
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          Parámetro           |                                                                                                                                                                                   Descripción                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                  |
|      -recoverypassword       |                                                                                                                                    Agrega un protector de contraseña numérica. También puede usar **- rp** como una versión abreviada de este comando.                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        Representa la contraseña de recuperación.                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                Agrega un protector de clave externo para la recuperación. También puede usar **- rk** como una versión abreviada de este comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               Representa la ruta de acceso de directorio a la clave de recuperación.                                                                                                                                                                |
|         -startupkey          |                                                                                                                                 Agrega un protector de clave externo para el inicio. También puede usar **-sk** como una versión abreviada de este comando.                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                Representa la ruta de acceso de directorio a la clave de inicio.                                                                                                                                                                |
|         -certificado         |                                                                                                                               Agrega un protector de clave público para una unidad de datos. También puede usar **-cert** como una versión abreviada de este comando.                                                                                                                               |
|             -cf              |                                                                                                                                              Especifica que se utilizará un archivo de certificado para proporcionar el certificado de clave público.                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             Representa la ruta de acceso al archivo de certificado.                                                                                                                                                              |
|             -ct              |                                                                                                                                           Especifica que se utilizará una huella digital del certificado para identificar el certificado de clave público                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       Especifica el valor de la propiedad huella digital del certificado que desea usar. Por ejemplo, un valor de huella digital de certificado de "a9 09 50 2d d8 2a e4 14 33 f8 del e6 38 86 b0 0D 42 77 a3 2a 7b" debe especificarse como "a909502dd82ae41433e6f83886b00d4277a32a7b".                                                        |
|          -tpmandpin          |                                                                                           Agrega un módulo de plataforma segura (TPM) y un protector (PIN) número de identificación personal para la unidad del sistema operativo. También puede usar **- tp** como una versión abreviada de este comando.                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    Agrega un protector de clave de inicio y el TPM para la unidad del sistema operativo. También puede usar **-tsk** como una versión abreviada de este comando.                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                Agrega un protector de clave de inicio para la unidad del sistema operativo, TPM y PIN. También puede usar **- tpsk** como una versión abreviada de este comando.                                                                                                                 |
|          -contraseña           |                                                                                                                              Agrega un protector de clave de contraseña para la unidad de datos. También puede usar **- pw** como una versión abreviada de este comando.                                                                                                                              |
|      -adaccountorgroup       | Agrega un identificador de seguridad (SID): en función de protector de identidad para el volumen.  También puede usar **-sid** como una versión abreviada de este comando. **IMPORTANTE:** De forma predeterminada, no se puede agregar un protector ADAccountOrGroup forma remota mediante WMI o ¿manage-bde.  Si la implementación requiere la capacidad de agregar este protector de forma remota debe habilitar la delegación restringida. |
|        -computername         |                                                                                                       Especifica que ¿manage-bde se usa para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.                                                                                                       |
|            <Name>            |                                                                                                         Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-eliminar sintaxis y parámetros
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       Parámetro        |                                                                              Descripción                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             Representa la letra de una unidad seguida del signo de dos puntos.                                                             |
|         -tipo          |                               Identifica el protector de clave para eliminar. También puede usar **-t** como una versión abreviada de este comando.                               |
|    recoverypassword    |                                                 Especifica que deben eliminarse cualquier protectores de clave de contraseña de recuperación.                                                 |
|      ExternalKey       |                                        Especifica que deben eliminarse cualquier protectores de clave externos asociados a la unidad.                                         |
|      Certificado       |                                       Especifica que deben eliminarse cualquier protectores de clave del certificado asociados a la unidad.                                       |
|          tpm           |                                        Especifica que deben eliminarse cualquier protectores de clave solo TPM asociados a la unidad.                                         |
|    tpmandstartupkey    |                                Especifica que deben eliminarse cualquier TPM e inicio basado en claves protectores de clave asociados a la unidad.                                |
|       TPMAndPIN        |                                    Especifica que cualquier TPM y PIN basan protectores de clave asociados a la unidad debe eliminarse.                                    |
| tpmandpinandstartupkey |                             Especifica que deben eliminarse cualquier TPM, el PIN e inicio basado en claves protectores de clave asociados a la unidad.                             |
|        password        |                                        Especifica que deben eliminarse cualquier protectores de clave de contraseña asociados a la unidad.                                         |
|        aut.        |                                        Especifica que deben eliminarse cualquier protectores de clave de identidad asociados a la unidad.                                         |
|          -id           |                Identifica el protector de clave para eliminar mediante el identificador de clave. Este parámetro es una opción alternativa a la **-tipo** parámetro.                 |
|    <KeyProtectorID>    |        Identifica un protector de clave individual en la unidad a eliminar. Protector de clave de los identificadores se puede mostrar mediante el uso de la **¿manage-bde-protectors-obtener** comando.         |
|     -computername      | Especifica que se usará ese administrar-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando. |
|         <Name>         |    Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.     |
|        -? ¿o /?        |                                                               Muestra una breve ayuda en el símbolo del sistema.                                                               |
|      -help o -h       |                                                             Muestra Ayuda en la línea de comandos completa.                                                              |

### <a name="BKMK_disableprot"></a>-deshabilitar la sintaxis y parámetros
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   Parámetro   |                                                                                                                                                                                                                   Descripción                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  Representa la letra de una unidad seguida del signo de dos puntos.                                                                                                                                                                                                  |
|  RebootCount  | A partir de Windows 8, especifica que la protección del volumen del sistema operativo se ha suspendido y se reanudará una vez que Windows ha sido reiniciado el número de veces especificado en el parámetro RebootCount. Especifique 0 para suspender la protección indefinidamente. Si no se especifica este parámetro configurará automáticamente y protección de BitLocker reanudar cuando Windows se reinicia. También puede usar **-rc** como una versión abreviada de este comando. |
| -computername |                                                                                                                                      Especifica que se usará ese administrar-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando.                                                                                                                                      |
|    <Name>     |                                                                                                                                         Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.                                                                                                                                          |
|   -? ¿o /?    |                                                                                                                                                                                                    Muestra una breve ayuda en el símbolo del sistema.                                                                                                                                                                                                    |
|  -help o -h  |                                                                                                                                                                                                  Muestra Ayuda en la línea de comandos completa.                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>Ejemplos
El ejemplo siguiente se muestra cómo utilizar el **-protectores** comando para agregar un protector de clave de certificado identificado mediante un archivo de certificado en la unidad E.
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
El ejemplo siguiente se muestra cómo utilizar el **-protectores** comando para agregar un **adaccountorgroup** protector de clave identificado por el nombre de usuario y dominio a la unidad E.
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
El ejemplo siguiente se muestra cómo utilizar el **protectores** comando para deshabilitar la protección hasta que reinicie el equipo 3 veces.
```
manage-bde  protectors  disable C: -rc 3
```
El ejemplo siguiente se muestra cómo utilizar el **-protectores** comando para eliminar la clave TPM e iniciar todos los en función de los protectores de clave en la unidad C.
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
El ejemplo siguiente se muestra cómo utilizar el **-protectores** comando para la copia de seguridad de toda la información de recuperación para la unidad C: en AD DS.
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
