---
title: tpmtool
description: Artículo de referencia de tpmtool, que obtiene información sobre el Módulo de plataforma segura.
ms.topic: reference
author: ashleytqy
ms.author: asteoh
manager: raigner
ms.date: 05/07/2019
ms.openlocfilehash: f9d1a7c0bc1516feea3afaf00d750e54871ad818
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077592"
---
# <a name="tpmtool"></a>tpmtool

Esta utilidad se puede usar para obtener información sobre el [módulo de plataforma segura (TPM)](/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no proporciona ninguna garantía, expresa o implícita, con respecto a la información proporcionada aquí.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#tpmtool_examples).

## <a name="syntax"></a>Sintaxis

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|getdeviceinformation|Muestra la información básica del TPM. El significado de los valores de marca de información se puede encontrar [aquí](/windows/desktop/secprov/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [ruta de acceso del directorio de salida]|Recopila registros de TPM y los coloca en el directorio especificado. Si el directorio no existe, se crea. De forma predeterminada, se colocan en el directorio actual. Los archivos posibles generados son: </br>-TpmEvents. evtx</br>-TpmInformation.txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [iniciar/detener]|Iniciar o detener la recopilación de seguimientos del controlador de TPM. El registro de seguimiento, TPMTRACE. ETL, se generará y se colocará en el directorio actual.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=tpmtool_examples></a>Ejemplos

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

## <a name="decoding-error-codes"></a>Descodificación de códigos de error

Los códigos de error específicos de TPM se documentan [aquí](/windows/desktop/com/com-error-codes-6).
