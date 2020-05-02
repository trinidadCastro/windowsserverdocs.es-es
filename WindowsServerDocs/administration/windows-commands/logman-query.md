---
title: logman query
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05448a4f129a59145813dd0da7199d4adf845c5c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724357"
---
# <a name="logman-query"></a>logman query

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

propiedades del recopilador de datos de consulta o conjunto de recopiladores de datos.  

## <a name="syntax"></a>Sintaxis  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Parámetros  

|     Parámetro      |                                 Descripción                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Muestra la ayuda contextual.                       |
| -s<computer name> |            Ejecute el comando en el equipo remoto especificado.             |
|  -config <value>   |           Especifica el archivo de configuración que contiene opciones de comando.            |
|    [-n]<name>     |                          Nombre del objeto de destino.                          |
|        -ETS        | Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar. |

## <a name="examples"></a>Ejemplos  
El comando siguiente muestra todos los conjuntos de recopiladores de datos configurados en el sistema de destino.  
```  
logman query  
```  
El siguiente comando muestra los recopiladores de datos contenidos en el conjunto de recopiladores de datos denominado perf_log.  
```  
logman query perf_log  
```  
El siguiente comando muestra todos los proveedores disponibles de recopiladores de datos en el sistema de destino.  
```  
logman query providers  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
