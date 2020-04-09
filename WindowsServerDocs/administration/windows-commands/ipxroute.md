---
title: ipxroute
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1e011835dbdbcf7be1daca2cdfbd47c39f9355c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842069"
---
# <a name="ipxroute"></a>ipxroute

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra y modifica información acerca de las tablas de enrutamiento utilizadas por el protocolo IPX. Si se usa sin parámetros, **ipxroute** muestra la configuración predeterminada de los paquetes que se envían a direcciones desconocidas, de difusión y de multidifusión.   
## <a name="syntax"></a>Sintaxis  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|servidores [/Type = X]|Muestra la tabla de punto de acceso de servicio (SAP) para el tipo de servidor especificado.  **X** debe ser un entero. Por ejemplo, **/Type = 4** muestra todos los servidores de archivos. Si no especifica **/Type**, **los servidores ipxroute** muestran todos los tipos de servidores y los enumeran por nombre de servidor.|  
|Red ripout|Detecta si la *red* es accesible mediante la consulta de la tabla de rutas de la pila IPX y el envío de una solicitud RIP si es necesario.  *Network* es el número de segmento de red IPX.|  
|resolver {GUID&#124; Name} {GUID&#124; AdapterName}|Resuelve el nombre del GUID como su nombre descriptivo o el nombre descriptivo de su GUID.|  
|panel = *N*|Especifica el adaptador de red para el que se van a consultar o establecer parámetros.|  
|Def|Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a una dirección de tarjeta de acceso a medios (MAC) única que no se encuentra en la tabla de enrutamiento de origen, **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada.|  
|GbR|Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a la dirección de difusión (FFFFFFFFFFFF), **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada.|  
|Maestro|Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a una dirección de multidifusión (C000xxxxxxxx), **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada.|  
|Remove = *XXXXXXXXXXXX*|quita la dirección de nodo especificada de la tabla de enrutamiento de origen.|  
|config|Muestra información acerca de todos los enlaces para los que se ha configurado IPX.|  
|/?|Muestra la Ayuda en el símbolo del sistema.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Para mostrar los segmentos de red a los que está conectada la estación de trabajo, la dirección del nodo de estación de trabajo y el tipo de marco que se está usando, escriba:  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
