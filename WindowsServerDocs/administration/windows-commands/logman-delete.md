---
title: logman delete
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b30fd6eb7915d3d0296988a98968dcde58bdbc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724372"
---
# <a name="logman-delete"></a>logman delete

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

eliminar un recopilador de datos existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                               Descripción                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Muestra la ayuda contextual.                                                                     |
|   -s<computer name>    |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|       [-n]<name>       |                                                                   Nombre del recopilador de datos de destino.                                                                    |
|          -ETS           |                                              Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar.                                               |
| -[-] u <usuario [contraseña] > | Especifica el usuario que se va a ejecutar como. Al escribir \* un para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |

## <a name="examples"></a>Ejemplos  
El siguiente comando elimina el perf_log del recopilador de datos.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
