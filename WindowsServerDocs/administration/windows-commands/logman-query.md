---
title: logman query
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79e23e15d85e7ab50d651baf7a556cc76c64fc26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876666"
---
# <a name="logman-query"></a>logman query

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recopilador de datos de consulta o recopilador de datos de conjunto de propiedades.  
  
## <a name="syntax"></a>Sintaxis  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/?|Contextual muestra la Ayuda.|  
|-s <computer name>|Ejecutar el comando en el equipo remoto especificado.|  
|-config <value>|Especifica el archivo de configuración que contiene las opciones de comando.|  
|[-n] <name>|Nombre del objeto de destino.|  
|-ets|Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación.|  
## <a name="BKMK_examples"></a>Ejemplos  
El siguiente comando enumera todos los conjuntos de recopiladores de datos configurada en el sistema de destino.  
```  
logman query  
```  
El comando siguiente enumera los recopiladores de datos contenidos en el conjunto de recopiladores de datos llamado perf_log.  
```  
logman query "perf_log"  
```  
El siguiente comando enumera todos los proveedores disponibles de recopiladores de datos en el sistema de destino.  
```  
logman query providers  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
