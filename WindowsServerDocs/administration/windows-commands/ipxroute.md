---
title: ipxroute
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d995204eea0af776a2084a82411fa95542d1d77a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889096"
---
# <a name="ipxroute"></a>ipxroute

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra y modifica la información acerca de las tablas de enrutamiento que utiliza el protocolo IPX. Se utiliza sin parámetros, **ipxroute** muestra la configuración predeterminada para los paquetes que se envían a direcciones de multidifusión, difusión y desconocidas.   
## <a name="syntax"></a>Sintaxis  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|servers[ /type=X]|Muestra la tabla de punto de acceso de servicio (SAP) para el tipo de servidor especificado.  **X** debe ser un entero. Por ejemplo, **/type = 4** muestra todos los servidores de archivos. Si no especifica **/tipo**, **ipxroute servidores** muestra todos los tipos de servidores, ordenados por nombre de servidor.|  
|ripout red|Detecta si *red* es accesible mediante la tabla de rutas de la pila IPX de consultoría y enviar una solicitud de rip si es necesario.  *Red* es el número de segmento de red IPX.|  
|resolve{ GUID&#124; name} { GUID&#124; AdapterName}|Resuelve el nombre del GUID en su nombre descriptivo o el nombre descriptivo para su GUID.|  
|panel = *N*|Especifica el adaptador de red que se va a consultar o establecer los parámetros.|  
|def|Envía paquetes a la difusión de todas las rutas. Si un paquete se transmite a una dirección única de tarjeta de acceso de medios (MAC) que no está en la tabla de enrutamiento de origen, **ipxroute** envía el paquete a las rutas solo de difusión de forma predeterminada.|  
|gbr|Envía paquetes a la difusión de todas las rutas. Si un paquete se transmite a la dirección de difusión (FFFFFFFFFFFF), **ipxroute** envía el paquete a las rutas solo de difusión de forma predeterminada.|  
|mbr|Envía paquetes a la difusión de todas las rutas. Si un paquete se transmite a una dirección de multidifusión (C000xxxxxxxx), **ipxroute** envía el paquete a las rutas solo de difusión de forma predeterminada.|  
|remove= *xxxxxxxxxxxx*|Quita la dirección del nodo de la tabla de enrutamiento de origen.|  
|config|Muestra información sobre todos los enlaces para el que está configurado IPX.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Para mostrar los segmentos de red conectado a la estación de trabajo, la dirección de nodo de la estación de trabajo y tipo de marco que se va a usar, escriba:  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
