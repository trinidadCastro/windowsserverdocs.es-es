---
title: tpmtool
description: 'Temas de comandos de Windows para tpmtool: obtiene información sobre el Módulo de plataforma segura.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: 3967136bc64d1e06425a019466dea15ddce3a563
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385717"
---
# <a name="tpmtool"></a>tpmtool

Esta utilidad se puede usar para obtener información sobre el [módulo de plataforma segura (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxis

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|getdeviceinformation|Muestra la información básica del TPM. El significado de los valores de marca de información se puede encontrar [aquí](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [ruta de acceso del directorio de salida]|Recopila registros de TPM y los coloca en el directorio especificado. Si el directorio no existe, se crea. De forma predeterminada, se colocan en el directorio actual. Los archivos posibles generados son: </br>-TpmEvents. evtx</br>-TpmInformation. txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [iniciar/detener]|Iniciar o detener la recopilación de seguimientos del controlador de TPM. El registro de seguimiento, TPMTRACE. ETL, se generará y se colocará en el directorio actual.|
|parsetcglogs [-Validate (-v)]|Muestra el registro de TCG analizado, también conocido como registro de configuración de arranque de Windows (WBCL). Las descripciones de eventos más actualizadas se pueden encontrar en el [sitio web de TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), en **descripciones de eventos**. Si se establece la marca `-validate`, valida que los valores del registro de configuración de la plataforma (PCR) en el TPM coincidan con los valores del registro.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tpmtool_examples"></a>Example

Para mostrar la información básica del TPM, escriba:
```
tpmtool getdeviceinformation
```
Para recopilar los registros de TPM y colocarlos en el directorio actual, escriba:
```
tpmtool gatherlogs
```
Para recopilar los registros de TPM y colocarlos en `C:\Users\Public`, escriba:
```
tpmtool gatherlogs C:\Users\Public
```
Para recopilar seguimientos de controladores de TPM, escriba:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Para analizar el registro de TCG:
```
tpmtool parsetcglogs
```
Para analizar el registro de TCG y validar las PCR:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Descodificación de códigos de error

Los códigos de error específicos de TPM se documentan [aquí](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
