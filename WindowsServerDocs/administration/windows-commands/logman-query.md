---
title: logman query
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7b7cc202266a568108c7cbf0eac89260721014a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840668"
---
# <a name="logman-query"></a>logman query

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

propiedades del recopilador de datos de consulta o conjunto de recopiladores de datos.  

## <a name="syntax"></a>Sintaxis  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Parámetros  

|     Parámetro      |                                 Descripción                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Muestra la ayuda contextual.                       |
| -s <computer name> |            Ejecute el comando en el equipo remoto especificado.             |
|  -config <value>   |           Especifica el archivo de configuración que contiene opciones de comando.            |
|    [-n] <name>     |                          Nombre del objeto de destino.                          |
|        -ETS        | Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar. |

## <a name="examples"></a><a name=BKMK_examples></a>Example  
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
