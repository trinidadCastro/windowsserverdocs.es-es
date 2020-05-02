---
title: ejecutar Logman | detener
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f570d99d4b3eaa818c9fbdcce76c42d1cb12d4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724344"
---
# <a name="logman-start--stop"></a>ejecutar Logman | detener

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicie un recopilador de datos y establezca la hora de inicio en manual, o bien detenga un conjunto de recopiladores de datos y establezca la hora de finalización en manual.  

## <a name="syntax"></a>Sintaxis  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|     Parámetro      |                                 Descripción                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Muestra la ayuda contextual.                       |
| -s<computer name> |            Ejecute el comando en el equipo remoto especificado.             |
|  -config <value>   |           Especifica el archivo de configuración que contiene opciones de comando.            |
|    [-n]<name>     |                          Nombre del objeto de destino.                          |
|        -ETS        | Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar. |
|        -as         |               Realizar la operación solicitada de forma asincrónica.                |

## <a name="examples"></a>Ejemplos  
El comando siguiente inicia el perf_log del recopilador de datos en el server_1 del equipo remoto.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
