---
title: logman delete
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0ab38eab988770de4fbcef8af2c7be6a6137b16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840758"
---
# <a name="logman-delete"></a>logman delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

eliminar un recopilador de datos existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                               Descripción                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Muestra la ayuda contextual.                                                                     |
|   -s <computer name>    |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|       [-n] <name>       |                                                                   Nombre del recopilador de datos de destino.                                                                    |
|          -ETS           |                                              Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar.                                               |
| -[-] u < usuario [contraseña] > | Especifica el usuario que se va a ejecutar como. Al escribir una \* para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |

## <a name="examples"></a><a name=BKMK_examples></a>Example  
El siguiente comando elimina el perf_log del recopilador de datos.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
