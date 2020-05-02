---
title: ntfrsutl
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 698237380d02fb1ceb4e738c6fb4f083dd31aef3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723465"
---
# <a name="ntfrsutl"></a>ntfrsutl

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Vuelca la información de las tablas internas, los subprocesos y la memoria para el \(servicio\)de replicación de archivos de NT NTFRS. Se ejecuta en servidores locales y remotos. La configuración de recuperación de NTFRS en el administrador \(de\) control de servicios SCM puede ser fundamental para buscar y mantener eventos de registro importantes en el equipo. Esta herramienta proporciona un método práctico para revisar la configuración.   
  
## <a name="syntax"></a>Sintaxis  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
#### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                                                                                                                                                                                                                                        Descripción                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Tabla de ID.                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Tabla de configuración de FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Registro de entrada                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Registro de salida                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Especifica el equipo.                                                                                                                                                                                                                                                                                                                                   |
|   memoria    |                                                                                                                                                                                                                                                                                                                                        Uso de la memoria                                                                                                                                                                                                                                                                                                                                        |
|   subprocesos   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    fase    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         muestra la vista del servicio NTFRS del DS.                                                                                                                                                                                                                                                                                                                          |
|    conjuntos     |                                                                                                                                                                                                                                                                                                                             Especifica los conjuntos de réplicas activos                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Especifica las versiones del servicio API y NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    poll     | Especifica los intervalos de sondeo actuales.<p>Parámetros:<p><ul><li>**\/sondea rápidamente rápidamente** \[ **\=** \[ <N> \] \] \(  \)<p><ul><li>sondea rápidamente hasta que la configuración estable es rectrieved **quickly** \-</li><li>**sondea\= rápidamente** \- cada minuto predeterminado.</li><li>**sondea\= ** cada *N* minutos<N> \-</li></ul></li><li>**\/**\[ **\=** \[ <N> sondeos lentos \] lentamente \] \(\)<p><ul><li>**sondea lentamente lentamente hasta** que se recupera la configuración estable \-</li><li>sondeos **lentos\= ** \- lentamente cada minuto predeterminado</li><li>**sondea\= lentamente** <N> cada N minutos rápidamente *N* \-</li></ul></li><li>** \/ahora** \(sondea ahora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="examples"></a>Ejemplos  
Para determinar el intervalo de sondeo para la replicación de archivos:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar la versión actual de API \(\) de la interfaz de programa de la aplicación NTFRS:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
  
  

