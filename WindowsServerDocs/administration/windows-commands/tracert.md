---
title: tracert
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f08fd3276f3377fed06d7b9a2cc3399fa1071f39
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385649"
---
# <a name="tracert"></a>tracert

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Determina la ruta de acceso tomada a un destino mediante el envío de solicitudes de eco del Protocolo de mensajes de control de Internet (ICMP) o mensajes ICMPv6 al destino con valores de campo de período de vida (TTL) que aumentan incrementalmente. La ruta de acceso que se muestra es la lista de interfaces de enrutador Near/Side de los enrutadores de la ruta de acceso entre un host de origen y un destino. La interfaz en paralelo es la interfaz del enrutador que está más cerca del host de envío en la ruta de acceso. Si se usa sin parámetros, tracert muestra la ayuda.   

## <a name="syntax"></a>Sintaxis  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/d|Impide que **tracert** intente resolver las direcciones IP de los enrutadores intermedios en sus nombres. Esto puede acelerar la presentación de los resultados de **tracert** .|  
|/h \<MaximumHops >|Especifica el número máximo de saltos en la ruta de acceso para buscar el destino (destino). El valor predeterminado es 30 saltos.|  
|/j \<Hostlist >|Especifica que los mensajes de solicitud de eco usan la opción de ruta de origen flexible del encabezado IP con el conjunto de destinos intermedios especificados en *hostlist*. Con el enrutamiento de origen suelto, los destinos intermedios sucesivos pueden estar separados por uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. *Hostlist* es una serie de direcciones IP (en notación decimal con puntos) separadas por espacios. Use este parámetro solo cuando se realice un seguimiento de las direcciones IPv4.|  
|/w \<timeout >|Especifica la cantidad de tiempo en milisegundos que se debe esperar a que se reciba el mensaje de tiempo de ICMP superado o de respuesta de eco correspondiente a un mensaje de solicitud de eco determinado. Si no se recibe en el tiempo de espera, se muestra un asterisco (*). El tiempo de espera predeterminado es 4000 (4 segundos).|  
|/R|Especifica que el encabezado de extensión de enrutamiento IPv6 se utiliza para enviar un mensaje de solicitud de eco al host local, utilizando el destino como un destino intermedio y probando la ruta inversa.|  
|/S \<Srcaddr >|Especifica la dirección de origen que se va a usar en los mensajes de solicitud de eco. Use este parámetro solo cuando se realice el seguimiento de direcciones IPv6.|  
|/4|Especifica que Tracert. exe solo puede usar IPv4 para este seguimiento.|  
|/6|Especifica que Tracert. exe solo puede utilizar IPv6 para este seguimiento.|  
|@no__t 0TargetName >|Especifica el destino, identificado por la dirección IP o el nombre de host.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Esta herramienta de diagnóstico determina la ruta de acceso tomada a un destino mediante el envío de mensajes de solicitud de eco ICMP con valores de período de vida (TTL) variables al destino. Cada enrutador a lo largo de la ruta de acceso es necesario para reducir el TTL de un paquete IP por al menos 1 antes de reenviarlo. De hecho, el TTL es un contador de vínculos máximo. Cuando el TTL de un paquete llega a 0, se espera que el enrutador devuelva un mensaje ICMP Time exceeded al equipo de origen. tracert determina la ruta de acceso enviando el primer mensaje de solicitud de eco con un TTL de 1 e incrementando el TTL en 1 en cada transmisión posterior hasta que el destino responde o hasta que se alcanza el número máximo de saltos. De forma predeterminada, el número máximo de saltos es 30 y se puede especificar mediante el parámetro **/h** . La ruta de acceso se determina examinando los mensajes de tiempo de ICMP superados que devuelven los enrutadores intermedios y el mensaje de respuesta de eco devuelto por el destino. Sin embargo, algunos enrutadores no devuelven los mensajes de tiempo excedidos para los paquetes con valores de TTL caducados y se invisilen al comando tracert. En este caso, se muestra una fila de asteriscos (*) para ese salto.  
-   Para realizar un seguimiento de una ruta de acceso y proporcionar la latencia de red y la pérdida de paquetes para cada enrutador y vínculo en la ruta de acceso, use el comando **pathping** .  
-   Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.  

## <a name="BKMK_Examples"></a>Example  
Para realizar un seguimiento de la ruta de acceso al host denominado corp7.microsoft.com, escriba:  
```  
tracert corp7.microsoft.com  
```  
Para realizar un seguimiento de la ruta de acceso al host denominado corp7.microsoft.com y evitar la resolución de cada dirección IP en su nombre, escriba:  
```  
tracert /d corp7.microsoft.com  
```  
Para realizar un seguimiento de la ruta de acceso al host denominado corp7.microsoft.com y usar la ruta de origen flexible 10.12.0.1/10.29.3.1/10.1.44.1, escriba:  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
