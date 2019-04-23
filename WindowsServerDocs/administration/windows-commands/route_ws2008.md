---
title: route_ws2008
description: Obtenga información sobre cómo modificar y mostrar las entradas de la tabla de enrutamiento IP local.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1164767a80b4d7dd24152bc34eda5d88834c1bdb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854826"
---
# <a name="routews2008"></a>route_ws2008

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra y modifica las entradas de la tabla de enrutamiento IP local. Se utiliza sin parámetros, **ruta** muestra la Ayuda.   

## <a name="syntax"></a>Sintaxis  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

### <a name="parameters"></a>Parámetros  

|Parámetro|Descripción|  
|-------|--------|  
|/f|Borra la tabla de enrutamiento de todas las entradas que no son las rutas de host (rutas con una máscara de red 255.255.255.255), la ruta de red de bucle invertido (rutas con un destino de 127.0.0.0 y una máscara de red de 255.0.0.0) o una ruta de multidifusión (rutas con un destino de 224.0.0.0 y una máscara de red de 240.0.0.0). Si se usa junto con uno de los comandos (como agregar, cambiar o eliminar), se borra la tabla antes de ejecutar el comando.|  
|/p|Cuando se usa con el comando add, la ruta especificada se agrega al registro y se usa para inicializar la tabla de enrutamiento IP siempre que se inicia el protocolo TCP/IP. De forma predeterminada, las rutas agregadas no se conservan cuando se inicia el protocolo TCP/IP. Cuando se usa con el comando de impresión, se muestra la lista de rutas persistentes. Este parámetro se omite para todos los demás comandos. Las rutas persistentes se almacenan en la ubicación del registro **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\PersistentRoutes**.|  
|\<Command>|Especifica el comando que desea ejecutar. En la tabla siguiente se enumera los comandos válidos:<br /><br />-   **agregar:** agrega una ruta.<br />-   **cambiar:** modifica una ruta existente.<br />-   **eliminar:** elimina una o varias rutas.<br />-   **imprimir:** imprime una o varias rutas.|  
|\<Destino >|Especifica el destino de la ruta de red. El destino puede ser una dirección de red (donde los bits de host de la dirección de red se establecen en 0), una dirección IP para una ruta de host, o 0.0.0.0 para la ruta predeterminada.|  
|máscara \<máscara de red >|Especifica el destino de la ruta de red. El destino puede ser una dirección de red (donde los bits de host de la dirección de red se establecen en 0), una dirección IP para una ruta de host, o 0.0.0.0 para la ruta predeterminada.|  
|\<Gateway>|Especifica la dirección IP de salto siguiente o de reenvío en la que el conjunto de direcciones definido por la máscara de subred y de destino de la red son accesibles. Para las rutas de subred conectadas localmente, la dirección de puerta de enlace es la dirección IP asignada a la interfaz que está conectada a la subred. Para rutas remotas, disponibles a través de uno o más enrutadores, la dirección de puerta de enlace es una dirección IP accesible directamente que se asigna a un enrutador vecino.|  
|métrica \<métrica >|Especifica una métrica de costo de enteros (entre 1 y 9999) para la ruta, que se usa al elegir entre varias rutas en la tabla de enrutamiento que mejor coincide con la dirección de destino de un paquete que se va a reenviar. Se elige la ruta con la métrica más baja. La métrica puede reflejar el número de saltos, la velocidad de la ruta de acceso, confiabilidad, rendimiento de la ruta de acceso o propiedades administrativas.|  
|Si \<interfaz >|Especifica el índice de la interfaz en la que el destino es accesible. Para obtener una lista de interfaces y sus índices correspondientes, use la visualización de lo del comando route print. Puede usar valores decimales o hexadecimales para el índice de interfaz. Para valores hexadecimales, preceda el número hexadecimal 0 x. Cuando la si se omite el parámetro, la interfaz se determina a partir de la dirección de puerta de enlace.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Los valores grandes en el **métrica** columna de la tabla de enrutamiento son el resultado de permitir que TCP/IP determinar automáticamente la métrica de rutas en la tabla de enrutamiento basado en la configuración de dirección IP, máscara de subred y puerta de enlace predeterminada para cada interfaz LAN. Determinación automática de la métrica de interfaz habilitada de forma predeterminada, determina la velocidad de cada interfaz y ajusta las métricas de rutas para cada interfaz para que las rutas crea la interfaz más rápida con la métrica más baja. Para quitar las métricas de gran tamaño, deshabilite la determinación automática de la métrica de interfaz de las propiedades avanzadas del protocolo TCP/IP para cada conexión LAN.  
-   Los nombres pueden usarse para *destino* si existe una entrada correspondiente en el archivo de las redes local almacenado en el **systemroot\System32\Drivers\\**carpeta etcetera. Los nombres pueden usarse para la *puerta de enlace* , siempre se pueden resolver como una dirección IP a través de técnicas de resolución de nombre de host estándar como las consultas del sistema de nombres de dominio (DNS), usar el archivo Hosts local almacenado en el  **systemroot\system32\drivers\\**etcetera carpeta y resolución de nombres NetBIOS.  
-   Si el comando es **imprimir** o **eliminar**, *puerta de enlace* se puede omitir el parámetro y se pueden usar caracteres comodín para el destino y la puerta de enlace. El *destino* valor puede ser un valor de carácter comodín especificado por un asterisco (*). Si el destino especificado contiene un asterisco (\*) o un signo de interrogación (?), se trata como un carácter comodín y solo rutas de destino coincidente se imprimen o se eliminan. El asterisco coincide con cualquier cadena, y el signo de interrogación devuelve cualquier carácter individual. Por ejemplo, 10. \*.1, 192.168. \*, 127. \*, y \*224\* son usos válidos del comodín de asterisco.  
-   Utiliza una combinación no válida de un valor de destino y subred mask (máscara de red), aparecerá una "ruta: máscara de red de dirección de puerta de enlace incorrecta" mensaje de error. Este mensaje de error aparece cuando el destino contiene uno o más bits establecidos en 1 en las ubicaciones donde el bit correspondiente de la máscara de subred se establece en 0. Para probar esta condición, exprese el destino y máscara de subred mediante notación binaria. La máscara de subred en notación binaria consta de una serie de 1 bit, que representa la parte de la dirección de red de destino y una serie de bits 0, que representa la parte de la dirección de host de destino. Compruebe si hay bits en el destino que se establecen en 1 para la parte del destino que es la dirección de host (como se define por la máscara de subred).  
-   El **/p** parámetro solo se admite en el comando route para Windows NT 4.0, Windows 2000, Windows Millennium edition, Windows XP y Windows Server 2003. Este parámetro no es compatible con la **ruta** comando de Windows 95 o Windows 98.  
-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.  

## <a name="BKMK_Examples"></a>Ejemplos  
Para mostrar todo el contenido de la tabla de enrutamiento IP, escriba:  
```  
route print  
```  
Para mostrar las rutas en la tabla de enrutamiento IP que comienzan por 10, escriba:  
```  
route print 10.*  
```  
Para agregar una ruta predeterminada con la dirección de puerta de enlace predeterminada de 192.168.12.1, escriba:  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Para agregar una ruta al destino 10.41.0.0 con la máscara de subred 255.255.0.0 y la dirección del próximo salto de 10.27.0.1, escriba:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Para agregar una ruta persistente al destino 10.41.0.0 con la máscara de subred 255.255.0.0 y la dirección del próximo salto de 10.27.0.1, escriba:  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Para agregar una ruta al destino 10.41.0.0 con la máscara de subred 255.255.0.0, la dirección del próximo salto de 10.27.0.1 y la métrica de costo de 7, escriba:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Para agregar una ruta al destino 10.41.0.0 con la máscara de subred 255.255.0.0, la dirección del próximo salto de 10.27.0.1 y utilizando el índice de interfaz 0 x 3, escriba:  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Para eliminar la ruta al destino 10.41.0.0 con la máscara de subred 255.255.0.0, escriba:  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Para eliminar todas las rutas de la tabla de enrutamiento IP que comienzan con 10, escriba:  
```  
route delete 10.*  
```  
Para cambiar la dirección del próximo salto de la ruta con el destino de 10.41.0.0 y la máscara de subred 255.255.0.0 de 10.27.0.1 a 10.27.0.25, escriba:  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
