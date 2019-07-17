---
title: tpmtool
description: Tema de los comandos de Windows para tpmtool - obtiene información sobre el módulo de plataforma segura.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230865"
---
# <a name="tpmtool"></a>tpmtool

Esta utilidad se puede usar para obtener información sobre la [módulo de plataforma segura (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

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
|getdeviceinformation|Muestra la información básica del TPM. El significado de los valores de marca de la información puede encontrarse [aquí](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [ruta de acceso de directorio de salida]|Recopila registros TPM y los coloca en el directorio especificado. Si no existe ese directorio, se crea. De forma predeterminada, se colocan en el directorio actual. Los posibles archivos generados son: </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [iniciar / detener]|Iniciar o detener la recopilación de seguimientos de controlador TPM. El registro de seguimiento, TPMTRACE.etl, se generarán y se coloca en el directorio actual.|
|parsetcglogs [-validar (-v)]|Muestra el registro TCG analizado, también conocido como el Windows arranque configuración del registro (WBCL). Las descripciones de evento actualizadas pueden encontrarse en el [sitio Web TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), en **las descripciones de evento**. Si el `-validate` marcador establecido, valida que los valores de registro de configuración de plataforma (PCR) del TPM coinciden con los valores en el registro.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="tpmtool_examples"></a>Ejemplos

Para mostrar la información básica del TPM, escriba:
```
tpmtool getdeviceinformation
```
Para recopilar registros TPM y colocarlos en el directorio actual, escriba:
```
tpmtool gatherlogs
```
Para recopilar registros TPM y colocarlos en `C:\Users\Public`, tipo:
```
tpmtool gatherlogs C:\Users\Public
```
Para recopilar seguimientos de controlador TPM, escriba:
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Para analizar el registro TCG:
```
tpmtool parsetcglogs
```
Para analizar el registro TCG y validar PCRs:
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Descodificación de códigos de Error

Códigos de error específicos de TPM se documentan [aquí](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
