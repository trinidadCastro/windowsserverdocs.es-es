---
title: logman import | exportar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d68f5f340476bbb783c47f9c3fe9c060105b4e4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437716"
---
# <a name="logman-import--export"></a>logman import | exportar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importar un conjunto de recopiladores de datos desde un archivo XML, o exportar un conjunto de recopiladores de datos a un archivo XML.  

## <a name="syntax"></a>Sintaxis  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                        Descripción                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Contextual muestra la Ayuda.                                                              |
|   -s <computer name>    |                                                   Ejecutar el comando en el equipo remoto especificado.                                                   |
|     -config <value>     |                                                  Especifica el archivo de configuración que contiene las opciones de comando.                                                  |
|       [-n] <name>       |                                                                Nombre del objeto de destino.                                                                 |
|       -xml <name>       |                                                         Nombre del archivo XML para importar o exportar.                                                         |
|          -ets           |                                       Enviar comandos a sesiones de seguimiento de eventos directamente sin guardar ni programación.                                        |
| -[-]u <user [password]> | Ejecutar como usuario. Escribir un \* para la contraseña genera una solicitud para la contraseña. La contraseña no se muestra cuando se escribe en el símbolo del sistema de contraseña. |
|           -y            |                                                      Responda Sí a todas las preguntas sin preguntar.                                                       |

## <a name="BKMK_examples"></a>Ejemplos  
El siguiente comando importa el c:\windows\perf_log.xml archivo XML de la equipo servidor_1 como llamado perf_log del conjunto de un recopilador de datos.  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>Referencias adicionales  
[logman](logman.md)  
