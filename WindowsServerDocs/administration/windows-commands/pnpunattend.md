---
title: pnpunattend
description: Aprenda a auditar los controladores de dispositivos en un equipo, así como realizar instalaciones de controlador de modo silencioso.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 53b72459d497ac5d079336c2a00ba65634b2e3a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436335"
---
# <a name="pnpunattend"></a>pnpunattend

Auditorías de un equipo para los controladores de dispositivos y realizar instalaciones desatendidas de controlador, o buscar controladores sin necesidad de instalar y, opcionalmente, notificar los resultados a la línea de comandos. Use este comando para especificar la instalación de controladores específicos para dispositivos de hardware específico. Consulta Observaciones.

## <a name="syntax"></a>Sintaxis

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|auditSystem|Especifica la instalación de controladores en línea.</br>¿Obligatorio, excepto cuando **pnpunattend** se ejecuta con cualquiera el **/ayuda** o **/?** parámetros.|
|/s|Opcional. Especifica una búsqueda de controladores sin necesidad de instalar.|
|/L|Opcional. Especifica que se muestre la información de registro para este comando en el símbolo del sistema.|
|/?|Opcional. Muestra ayuda para este comando en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Preparación preliminar es necesario. Antes de usar este comando, debe completar las tareas siguientes:

1. Cree un directorio para los controladores que desea instalar. Por ejemplo, cree una carpeta en **C:\Drivers\Video** para controladores de adaptador de vídeo.
2. Descargue y extraiga el paquete de controladores para el dispositivo. Copie el contenido de la subcarpeta que contiene el archivo INF para su versión del sistema operativo y todas las subcarpetas en la carpeta de vídeo que ha creado. Por ejemplo, copie los archivos de controlador de vídeo a C:\Drivers\Video.
3. Agregar una variable de ruta de acceso de entorno del sistema a la carpeta que creó en el paso 1, por ejemplo, **C:\Drivers\Video**.
4. Cree la siguiente clave del registro y, a continuación, para la **DriverPaths** clave se crea, configura el **datos del valor** a **1**.
5. Para Windows® 7 navegar por la ruta de acceso del registro: **HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion\\** y, a continuación, cree las claves: **UnattendSettings\PnPUnattend\DriverPaths\\**
6. Para Windows Vista, vaya a la ruta de acceso del registro: **NT\CurrentVersion HK_LM\Software\Microsoft\Windows\\** y, a continuación, cree las claves = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Ejemplos

El siguiente comando de ejemplo muestra cómo utilizar el **PNPUnattend.exe** auditar un equipo para posibles actualizaciones del controlador y, a continuación, informar las conclusiones a la línea de comandos.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)