---
title: tracert
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837186"
---
# <a name="tracert"></a>tracert

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Determina la ruta tomada a un destino mediante el envío de mensajes ICMPv6 o solicitud de eco del protocolo de mensajes de Control de Internet (ICMP) para el destino con el aumento incremental del tiempo para los valores de campo de vida (TTL). Muestra la ruta de acceso es la lista de las interfaces de enrutador casi/lado de los enrutadores de la ruta de acceso entre un host de origen y un destino. La interfaz casi/lado es la interfaz del enrutador más cercana para el host de envío en la ruta de acceso. Tracert usa sin parámetros, muestra la Ayuda.   

## <a name="syntax"></a>Sintaxis  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/d|Evita que **tracert** intentan resolver las direcciones IP de los enrutadores intermedios en sus nombres. Esto puede acelerar la presentación de **tracert** resultados.|  
|/h \<saltosMáximos >|Especifica el número máximo de saltos en la ruta de acceso para buscar el destino (destino). El valor predeterminado es 30 saltos.|  
|/j \<Hostlist>|Especifica que un eco de los mensajes de solicitud de usan la opción de ruta de origen no estricta en el encabezado IP con el conjunto de destinos intermedios especificados en *Hostlist*. Con el enrutamiento de origen no estricta, sucesivos destinos intermedios se pueden separar con uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. El *Hostlist* es una serie de direcciones IP (en notación decimal con puntos) separada por espacios. Utilice este parámetro únicamente cuando el seguimiento de direcciones IPv4.|  
|/w \<timeout>|Especifica la cantidad de tiempo en milisegundos para esperar el tiempo ICMP superada o correspondiente a un mensaje de solicitud de eco determinado para poder recibir el mensaje de respuesta de eco. Si no se recibe en el tiempo de espera, se muestra un asterisco (*). El tiempo de espera predeterminado es 4000 (4 segundos).|  
|/R|Especifica que el encabezado de extensión de enrutamiento IPv6 se utiliza para enviar un mensaje de solicitud de eco al host local, utilizando el destino como un destino intermedio y probar el enrutamiento inverso.|  
|/S \<Srcaddr>|Especifica la dirección de origen para usar en el eco de los mensajes de solicitud. Utilice este parámetro únicamente cuando el seguimiento de direcciones IPv6.|  
|/4|Especifica que tracert.exe puede utilizar solo IPv4 para este seguimiento.|  
|/6|Especifica que tracert.exe solamente puede utilizar IPv6 para este seguimiento.|  
|\<TargetName>|Especifica el destino, identificado por el nombre de host o dirección IP.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
-   Esta herramienta de diagnóstico determina la ruta tomada a un destino enviando mensajes de solicitud con variables de tiempo de eco ICMP a valores de vida (TTL) en el destino. Cada enrutador a lo largo de la ruta de acceso debe disminuir el TTL de un paquete IP al menos 1 antes de reenviarlo. De hecho, el valor de TTL es un contador de número máximo de vínculos. Cuando el TTL de un paquete llega a 0, el enrutador debe devolver un mensaje de superado el tiempo ICMP en el equipo de origen. TRACERT determina la ruta de acceso mediante el envío el eco del primer mensaje de solicitud con un TTL de 1 e incrementando el TTL en 1 en cada transmisión posterior hasta que el destino responda o el número máximo de saltos se alcanza. El número máximo de saltos predeterminado es 30 y puede especificarse mediante la **/h** parámetro. La ruta de acceso se determina examinando los mensajes ICMP tiempo superado devueltos por los enrutadores intermedios y el eco de mensaje de respuesta devuelto por el destino. Sin embargo, algunos enrutadores no devuelven mensajes en superado el tiempo para los paquetes con valores de TTL caducados y son invisile al comando tracert. En este caso, se muestra una fila de asteriscos (*) para ese salto.  
-   Para trazar una ruta de acceso y proporcionar latencia de red y pérdida de paquetes para cada enrutador y cada vínculo de la ruta de acceso, use el **pathping** comando.  
-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.  

## <a name="BKMK_Examples"></a>Ejemplos  
Para realizar el seguimiento de la ruta de acceso al host llamado corp7.microsoft.com, escriba:  
```  
tracert corp7.microsoft.com  
```  
Para trazar la ruta de acceso al host llamado corp7.microsoft.com y evitar la resolución de cada dirección IP a su nombre, escriba:  
```  
tracert /d corp7.microsoft.com  
```  
Para rastrear la ruta de acceso al host llamado corp7.microsoft.com y usar el 10.12.0.1/10.29.3.1/10.1.44.1 ruta origen no estricta, escriba:  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
