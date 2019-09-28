---
title: logman delete
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374429"
---
# <a name="logman-delete"></a>logman delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

eliminar un recopilador de datos existente.  

## <a name="syntax"></a>Sintaxis  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                               Descripción                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Muestra la ayuda contextual.                                                                     |
|   -s <computer name>    |                                                          Ejecute el comando en el equipo remoto especificado.                                                          |
|     -config <value>     |                                                         Especifica el archivo de configuración que contiene opciones de comando.                                                         |
|       [-n] <name>       |                                                                   Nombre del recopilador de datos de destino.                                                                    |
|          -ETS           |                                              Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar.                                               |
| -[-] u < usuario [contraseña] > | Especifica el usuario que se va a ejecutar como. Al escribir un \* para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |

## <a name="BKMK_examples"></a>Example  
El siguiente comando elimina el recopilador de datos perf_log.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
