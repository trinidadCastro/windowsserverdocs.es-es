---
title: bootcfg rmsw
description: 'Tema de los comandos de Windows para **rmsw bootcfg** : quita operativo opciones de carga para una entrada de sistema operativo especificado.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65a8ba452911eb1b46a748d4e9d639ed102421b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878826"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificado.

## <a name="syntax"></a>Sintaxis
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/s <computer>|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u <Domain>\\<User>|Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.|
|/p <Password>|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/mm|Quita la opción/maxmem y su valor de memoria máxima asociado especificado <OSEntryLineNum>. La opción/maxmem especifica la cantidad máxima de RAM que puede utilizar el sistema operativo.|
|/bv|Quita la opción basevideo especificado <OSEntryLineNum>. La opción basevideo dirige el sistema operativo que se usará el modo VGA estándar para el controlador de vídeo instalado.|
|/so|Quita la opción/SOS especificado <OSEntryLineNum>. La opción/SOS dirige el sistema operativo para mostrar los nombres de controlador de dispositivo mientras se cargan.|
|/ng|Quita la opción noguiboot especificado <OSEntryLineNum>. La opción noguiboot deshabilita la barra de progreso que aparece antes de la CTRL + ALT + SUPR inicio de sesión.|
|/ Id <OSEntryLineNum>|Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini del que se quitan las opciones de carga del sistema operativo. La primera línea después de encabezado de sección de la sección [operating systems] es 1.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /rmsw**comando:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
