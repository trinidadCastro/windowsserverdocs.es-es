---
title: tpmtool
description: Artículo de referencia para el comando tpmtool, que obtiene información sobre el Módulo de plataforma segura.
ms.topic: reference
author: ashleytqy
ms.author: asteoh
manager: raigner
ms.date: 05/07/2019
ms.openlocfilehash: bc0aaa4bc4bd9c23e0cf22b0315335287867d8cd
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156437"
---
# <a name="tpmtool"></a>tpmtool

Esta utilidad se puede usar para obtener información sobre el [módulo de plataforma segura (TPM)](/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Algunos datos pueden estar relacionados con el producto publicado previamente, que puede modificarse sustancialmente antes de que se publique comercialmente. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

## <a name="syntax"></a>Sintaxis

```
tpmtool /parameter [<arguments>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| getdeviceinformation | Muestra la información básica del TPM. Consulte el artículo [Win32_Tpm:: IsReadyInformation Method Parameters](/windows/win32/secprov/win32-tpm-isreadyinformation#parameters) para obtener más información sobre los valores de marca de información. |
| GatherLogs [ruta de acceso del directorio de salida] | Recopila registros de TPM y los coloca en el directorio especificado. Si el directorio no existe, se crea. De forma predeterminada, los archivos de registro se colocan en el directorio actual. Los archivos posibles generados son:<ul><li>TpmEvents. evtx</li><li>TpmInformation.txt</li><li>SRTMBoot. dat</li><li>SRTMResume. dat</li><li>DRTMBoot. dat</li><li>DRTMResume. dat</li></ul> |
| drivertracing `[start | stop]` | Inicia o detiene la recopilación de seguimientos del controlador de TPM. El registro de seguimiento, *TPMTRACE. ETL*, se crea y se coloca en el directorio actual. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para mostrar la información básica del TPM, escriba:

```
tpmtool getdeviceinformation
```

Para recopilar los registros de TPM y colocarlos en el directorio actual, escriba:

```
tpmtool gatherlogs
```

Para recopilar los registros de TPM y colocarlos en `C:\Users\Public` , escriba:

```
tpmtool gatherlogs C:\Users\Public
```

Para recopilar seguimientos de controladores de TPM, escriba:

```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Códigos de error COM (TPM, PLA, FVE)](/windows/win32/com/com-error-codes-6)
