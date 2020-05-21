---
title: pnpunattend
description: Obtenga información acerca de cómo auditar los controladores de dispositivos en un equipo, así como realizar instalaciones de controladores silenciosos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d62edde425a00db23f150246df65166234604cef
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436470"
---
# <a name="pnpunattend"></a>pnpunattend

Audita un equipo para los controladores de dispositivos y realiza instalaciones de controladores desatendidas, o busca controladores sin instalar y, opcionalmente, notifica los resultados a la línea de comandos. Use este comando para especificar la instalación de controladores específicos para dispositivos de hardware específicos. Vea la sección Comentarios.

## <a name="syntax"></a>Sintaxis

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|auditSystem|Especifica la instalación del controlador en línea.</br>Obligatorio, excepto cuando se ejecuta **pnpunattend** con las opciones **/Help** o **/?** parámetros.|
|/s|Opcional. Especifica que se busquen controladores sin instalar.|
|/L|Opcional. Especifica que se muestre la información de registro de este comando en el símbolo del sistema.|
|/?|Opcional. Muestra la ayuda de este comando en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Se requiere la preparación preliminar. Antes de utilizar este comando, debe completar las siguientes tareas:

1. Cree un directorio para los controladores que desea instalar. Por ejemplo, cree una carpeta en **C:\Drivers\Video** para los controladores del adaptador de vídeo.
2. Descargue y extraiga el paquete de controladores del dispositivo. Copie el contenido de la subcarpeta que contiene el archivo INF de la versión del sistema operativo y de las subcarpetas que haya creado en la carpeta de vídeo. Por ejemplo, copie los archivos del controlador de vídeo en C:\Drivers\Video.
3. Agregue una variable de ruta de acceso de entorno del sistema a la carpeta que creó en el paso 1. por ejemplo, **C:\Drivers\Video**.
4. Cree la siguiente clave del registro y, a continuación, para la clave **DriverPaths** que cree, establezca los **datos del valor** en **1**.
5. En Windows® 7, navegue por la ruta de acceso del registro: **HKEY_LOCAL_Machine \Software\microsoft\windows NT\CurrentVersion \\ **y, a continuación, cree las claves: **UnattendSettings\PnPUnattend\DriverPaths \\ **
6. En Windows Vista, vaya a la ruta de acceso del registro: **HK_LM \Software\microsoft\windows NT\CurrentVersion \\ **y, a continuación, cree las claves = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Ejemplos

El comando to muestra cómo usar **PNPUnattend. exe** para auditar un equipo en busca de posibles actualizaciones de controladores y, a continuación, notificar los hallazgos en el símbolo del sistema.

```
pnpunattend auditsystem /s /l
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)