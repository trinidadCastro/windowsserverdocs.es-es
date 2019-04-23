---
title: mqbkup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842906"
---
# <a name="mqbkup"></a>mqbkup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza copias de seguridad del registro y archivos de mensajes MSMQ configuración para un dispositivo de almacenamiento y restaura la configuración y los mensajes almacenados anteriormente.   
La copia de seguridad y la operación de restauración detendrán el servicio MSMQ local. Si se inició el servicio MSMQ con antelación, la utilidad intentará reiniciar el servicio de MSMQ al final de la copia de seguridad o la operación de restauración. Si ya se detuvo el servicio antes de ejecutar la utilidad, se realiza ningún intento para reiniciar el servicio.  
Antes de usar la utilidad de copia de seguridad de mensaje de MSMQ/restauración debe cerrar todas las aplicaciones locales que usan MSMQ.  
## <a name="syntax"></a>Sintaxis  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/b|Especifica la operación de copia de seguridad|  
|/r|Especifica la operación de restauración|  
|< carpeta path_to_storage\_dispositivo >|Especifica la ruta de acceso donde se almacena los archivos de mensajes MSMQ y la configuración del registro|  
|/?|Muestra la ayuda en el símbolo del sistema.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Para todos los archivos de mensajes MSMQ y la configuración del registro de copia de seguridad y almacenarlas en el *Msmqbkup* carpeta en la unidad C:.  
```  
mqbkup /b c:\msmqbkup  
```  
Si la carpeta especificada no existe, la utilidad creará automáticamente uno. Si decide especificar una carpeta existente, esta carpeta debe estar vacía. Si especifica una carpeta de no vacío, la utilidad eliminará todos los archivos y subcarpetas incluidos dentro de él. En este caso, se le pedirá que dé permiso para eliminar subcarpetas y archivos existentes. Puede usar el **/y** parámetro para indicar que está de acuerdo con antelación para la eliminación de todos los archivos y subcarpetas existentes en la carpeta especificada.  
Para eliminar todos los archivos y subcarpetas de la *Oldbkup* carpeta en la unidad C: y almacén de archivos de mensajes MSMQ y configuración del registro en esta carpeta.  
```  
mqbkup /b /y c:\oldbkup  
```  
Para restaurar la configuración del registro y los mensajes de MSMQ:  
```  
mqbkup /r c:\msmqbkup  
```  
Las ubicaciones de carpetas utilizadas para almacenar archivos de mensajes MSMQ se almacenan en el registro. Por lo tanto, la utilidad restaurará los archivos de mensajes MSMQ a las carpetas especificadas en el registro y no a las carpetas de almacenamiento que usó antes de la operación de restauración. Si no dispone de las carpetas especificadas en el registro, la operación de restauración ellos creará automáticamente. Si los directorios de las carpetas existen y no están vacíos, la utilidad le pedirá permiso Eliminar el contenido actual de estas carpetas.  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
