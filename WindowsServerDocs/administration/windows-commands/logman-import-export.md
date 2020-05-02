---
title: Logman Import | Portada
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffc2e42f353352f69cf61dfb1f108a7d53cb7c4b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724362"
---
# <a name="logman-import--export"></a>Logman Import | Portada

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

importar un conjunto de recopiladores de datos desde un archivo XML o exportar un conjunto de recopiladores de datos a un archivo XML.  

## <a name="syntax"></a>Sintaxis  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
### <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                        Descripción                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Muestra la ayuda contextual.                                                              |
|   -s<computer name>    |                                                   Ejecute el comando en el equipo remoto especificado.                                                   |
|     -config <value>     |                                                  Especifica el archivo de configuración que contiene opciones de comando.                                                  |
|       [-n]<name>       |                                                                Nombre del objeto de destino.                                                                 |
|       -XML<name>       |                                                         Nombre del archivo XML que se va a importar o exportar.                                                         |
|          -ETS           |                                       Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programar.                                        |
| -[-] u <usuario [contraseña] > | Usuario que se ejecuta como. Al escribir \* un para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
|           -y            |                                                      Responda sí a todas las preguntas sin preguntar.                                                       |

## <a name="examples"></a>Ejemplos  
El comando siguiente importa el archivo XML c:\windows\ perf_log. xml del equipo server_1 como un conjunto de recopiladores de datos denominado perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
