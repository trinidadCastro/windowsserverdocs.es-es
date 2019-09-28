---
title: ntfrsutl
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372624"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vuelca la información de las tablas internas, los subprocesos y la memoria para el servicio de replicación de archivos NT \(NTFRS @ no__t-1. Se ejecuta en servidores locales y remotos. La configuración de recuperación de NTFRS en el administrador de control de servicios \(SCM @ no__t-1 puede ser fundamental para buscar y mantener eventos de registro importantes en el equipo. Esta herramienta proporciona un método práctico para revisar la configuración.   
  
## <a name="syntax"></a>Sintaxis  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|  Parámetro  |                                                                                                                                                                                                                                                                                                                                        Descripción                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Tabla de ID.                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Tabla de configuración de FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Registro de entrada                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Registro de salida                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Especifica el equipo.                                                                                                                                                                                                                                                                                                                                   |
|   memoria    |                                                                                                                                                                                                                                                                                                                                        Uso de memoria                                                                                                                                                                                                                                                                                                                                        |
|   ThreadPool   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    todo    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     directorio      |                                                                                                                                                                                                                                                                                                                         muestra la vista del servicio NTFRS del DS.                                                                                                                                                                                                                                                                                                                          |
|    Definir     |                                                                                                                                                                                                                                                                                                                             Especifica los conjuntos de réplicas activos                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Especifica las versiones del servicio API y NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    sondeo     | Especifica los intervalos de sondeo actuales.<br /><br />Parámetros:<br /><br /><ul><li>**\/quickly**\[ **\=** \[ <N> @ no__t-7 @ no__t-8 @ no__t-9Polls rápidamente @ no__t-10<br /><br /><ul><li>**rápidamente** \- sondea rápidamente hasta que la configuración estable es rectrieved</li><li>**rápidamente, @ no__t-1** \- sondea rápidamente cada minuto predeterminado.</li><li>**rápidamente, @ no__t-1**<N> \- sondea rápidamente cada *N* minutos</li></ul></li><li>**\/slowly**\[ **\=** \[ <N> @ no__t-7 @ no__t-8 \(Polls lentamente @ no__t-10<br /><br /><ul><li>los sondeos **lentamente** \- lentamente hasta que se recupera la configuración estable</li><li>**@ no__t-1** \- los sondeos lentamente cada minuto predeterminado</li><li>con **lentitud @ no__t-1**<N> \- sondea rápidamente cada *N* minutos</li></ul></li><li>**\/now** \(Polls Now @ no__t-3</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Example  
Para determinar el intervalo de sondeo para la replicación de archivos:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar la interfaz de programa de la aplicación NTFRS actual \(API @ no__t-1 versión:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
  
  

