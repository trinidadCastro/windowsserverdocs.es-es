---
title: ntfrsutl
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e94d2b0a9ca764a6e8a25a087817598e3f158581
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436404"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Volcados de memoria de las tablas internas, subproceso e información de memoria para el servicio de replicación de archivos NT \(NTFRS\). Ejecuta en servidores locales y remotos. La configuración de recuperación de NTFRS Service Control Manager \(SCM\) puede resultar fundamental para localizar y mantener los eventos de registro importantes en el equipo. Esta herramienta proporciona una manera cómoda de revisión de esas configuraciones.   
  
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
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Tabla de Id.                                                                                                                                                                                                                                                                                                                                          |
| configtable |                                                                                                                                                                                                                                                                                                                                  Tabla de configuración de FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Entrada de registro                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Registro de salida                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Especifica el equipo.                                                                                                                                                                                                                                                                                                                                   |
|   memoria    |                                                                                                                                                                                                                                                                                                                                        Uso de memoria                                                                                                                                                                                                                                                                                                                                        |
|   Subprocesos   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    todo    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         Muestra la vista del servicio NTFRS de DS.                                                                                                                                                                                                                                                                                                                          |
|    Conjuntos     |                                                                                                                                                                                                                                                                                                                             Especifica los conjuntos de réplica activo                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Especifica las versiones del servicio NTFRS y API.                                                                                                                                                                                                                                                                                                                        |
|    Sondeo     | Especifica los intervalos de sondeo actual.<br /><br />Parámetros:<br /><br /><ul><li>**\/rápidamente** \[ **\=** \[ <N> \] \] \(sondea rápidamente\)<br /><br /><ul><li>**rápidamente** \- sondea rápidamente hasta configuración estable es rectrieved</li><li>**rápidamente\=**  \- rápidamente sondea cada minutos de forma predeterminada.</li><li>**rápidamente\=**  <N> \- sondea rápidamente cada *N* minutos</li></ul></li><li>**\/lentamente** \[ **\=** \[ <N> \] \] \(sondea lentamente\)<br /><br /><ul><li>**lentamente** \- lentamente sondea hasta que se recupere la configuración estable</li><li>**lentamente\=**  \- lentamente sondea cada minutos de forma predeterminada</li><li>**lentamente\=**  <N> \- sondea rápidamente cada *N* minutos</li></ul></li><li>**\/Ahora** \(sondea ahora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para determinar el intervalo de sondeo para la replicación de archivos:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar la interfaz actual del programa de aplicación de NTFRS \(API\) versión:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
  
  

