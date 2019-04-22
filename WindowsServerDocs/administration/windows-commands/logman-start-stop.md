---
title: logman start | detener
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47d5bda20c79ac069c8c51e51535de49b5ca380d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821896"
---
# <a name="logman-start--stop"></a>logman start | detener

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

iniciar un recopilador de datos y establecer la hora de inicio a manual o detener un recopilador de datos establece y la hora de finalización en manual.  
  
## <a name="syntax"></a>Sintaxis  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|-?|Contextual muestra la Ayuda.|  
|-s <computer name>|Ejecutar el comando en el equipo remoto especificado.|  
|-config <value>|Especifica el archivo de configuración que contiene las opciones de comando.|  
|[-n] <name>|Nombre del objeto de destino.|  
|-ets|Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación.|  
|-as|Realizar la operación solicitada de forma asincrónica.|  
## <a name="BKMK_examples"></a>Ejemplos  
El comando siguiente inicia el perf_log del recopilador de datos en el servidor_1 del equipo remoto.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
