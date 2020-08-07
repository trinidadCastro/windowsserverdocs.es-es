---
title: route_ws2008
description: Obtenga información acerca de cómo modificar y mostrar las entradas en la tabla de enrutamiento IP local.
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: be68ece7b972da06a5638cc81a487518fe27bc68
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883477"
---
# <a name="route_ws2008"></a>route_ws2008

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra y modifica las entradas de la tabla de enrutamiento IP local. Cuando se usa sin parámetros, **Route** muestra la ayuda.

## <a name="syntax"></a>Sintaxis
```
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|/f|Borra la tabla de enrutamiento de todas las entradas que no son rutas de host (rutas con una máscara de red 255.255.255.255), la ruta de red de bucle invertido (rutas con un destino de 127.0.0.0 y una máscara de red de 255.0.0.0) o una ruta de multidifusión (rutas con un destino de 224.0.0.0 y una máscara de red de 240.0.0.0). Si se usa junto con uno de los comandos (como agregar, cambiar o eliminar), la tabla se borra antes de ejecutar el comando.|
|/p|Cuando se usa con el comando Add, la ruta especificada se agrega al registro y se usa para inicializar la tabla de enrutamiento IP cada vez que se inicia el protocolo TCP/IP. De forma predeterminada, las rutas agregadas no se conservan cuando se inicia el protocolo TCP/IP. Cuando se usa con el comando Print, se muestra la lista de rutas persistentes. Este parámetro se omite para todos los demás comandos. Las rutas persistentes se almacenan en la ubicación del registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\tcpip\parameters\persistentroutes**.|
|\<Command>|Especifica el comando que se desea ejecutar. En la tabla siguiente se enumeran los comandos válidos:<p>-   **Agregar:** agrega una ruta.<br />-   **cambiar:** modifica una ruta existente.<br />-   **eliminar:** elimina una ruta o rutas.<br />-   **imprimir:** imprime una ruta o rutas.|
|\<Destination>|Especifica el destino de red de la ruta. El destino puede ser una dirección de red IP (donde los bits de host de la dirección de red están establecidos en 0), una dirección IP para una ruta de host o 0.0.0.0 para la ruta predeterminada.|
|máscara\<Netmask>|Especifica el destino de red de la ruta. El destino puede ser una dirección de red IP (donde los bits de host de la dirección de red están establecidos en 0), una dirección IP para una ruta de host o 0.0.0.0 para la ruta predeterminada.|
|\<Gateway>|Especifica la dirección IP de reenvío o del próximo salto a través de la cual se puede obtener acceso al conjunto de direcciones definido por el destino de red y la máscara de subred. En el caso de las rutas de subred conectadas localmente, la dirección de puerta de enlace es la dirección IP asignada a la interfaz que está conectada a la subred. En el caso de las rutas remotas, disponibles en uno o varios enrutadores, la dirección de puerta de enlace es una dirección IP accesible directamente que se asigna a un enrutador vecino.|
|métrica\<Metric>|Especifica una métrica de costo de enteros (entre 1 a 9999) para la ruta, que se utiliza cuando se selecciona entre varias rutas de la tabla de enrutamiento de mayor coincidencia con la dirección de destino de un paquete que se está reenviando. Se elige la ruta con la métrica mas baja. La métrica puede reflejar el número de saltos, la velocidad, confiabilidad y rendimiento de la ruta de acceso, o las propiedades administrativas.|
|Cuando\<Interface>|Especifica el índice de interfaz para la interfaz a través de la que se obtiene acceso al destino. Para obtener una lista de interfaces y los correspondientes índices de interfaz, utilice los resultados de la ejecución del comando route print. En el índice de interfaz puede usar valores decimales o hexadecimales. Para valores hexadecimales, el número hexadecimal debe estar precedido por 0x. Cuando se omite el parámetro if, la interfaz se determina mediante la dirección de la puerta de enlace.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones
- Los valores grandes de la columna **métrica** de la tabla de enrutamiento son el resultado de permitir que TCP/IP determine automáticamente la métrica de las rutas de la tabla de enrutamiento en función de la configuración de la dirección IP, la máscara de subred y la puerta de enlace predeterminada para cada interfaz LAN. La determinación automática de la métrica de la interfaz, habilitada de forma predeterminada, determina la velocidad de cada interfaz y ajusta las métricas de las rutas de cada interfaz para que la interfaz más rápida cree las rutas con la métrica más baja. Para quitar las métricas grandes, deshabilite la determinación automática de la métrica de la interfaz de las propiedades avanzadas del protocolo TCP/IP para cada conexión LAN.
- Los nombres se pueden usar para el *destino* si existe una entrada adecuada en el archivo de redes locales almacenado en la carpeta <strong>systemroot\System32\Drivers \\ </strong>etc. Los nombres se pueden usar para la *puerta de enlace* siempre que se puedan resolver en una dirección IP a través de técnicas de resolución de nombres de host estándar, como consultas del sistema de nombres de dominio (DNS), el uso del archivo de hosts local almacenado en la carpeta <strong>systemroot\system32\drivers \\ </strong>etc y la resolución de nombres NetBIOS.
- Si el comando es **Print** o **Delete**, se puede omitir el parámetro de *puerta de enlace* y se pueden usar comodines para el destino y la puerta de enlace. El valor de *destino* puede ser un valor comodín especificado por un asterisco (*). Si el destino especificado contiene un asterisco ( \* ) o un signo de interrogación (?), se trata como un carácter comodín y solo se imprimen o eliminan las rutas de destino coincidentes. El asterisco coincide con cualquier cadena y el signo de interrogación coincide con cualquier carácter individual. Por ejemplo, 10. \* . 1, 192,168. \* , 127. \* y \* 224 \* son usos válidos del comodín de asterisco.
- Si usa una combinación no válida de un valor de máscara de subred y de destino, se mostrará el mensaje de error "ruta: máscara de la dirección de puerta de enlace incorrecta". Este mensaje de error aparece cuando el destino contiene uno o varios bits establecidos en 1 en ubicaciones de bits donde el bit de máscara de subred correspondiente se establece en 0. Para probar esta condición, exprese el destino y la máscara de subred mediante la notación binaria. La máscara de subred en notación binaria consta de una serie de 1 bits, que representa la parte de la dirección de red del destino y una serie de 0 bits, que representa la parte de la dirección del host del destino. Compruebe si hay bits en el destino que estén establecidos en 1 para la parte del destino que sea la dirección del host (tal y como se define en la máscara de subred).
- El parámetro **/p** solo se admite en el comando Route para windows NT 4,0, Windows 2000, Windows Millennium Edition, Windows XP y windows Server 2003. Este parámetro no es compatible con el comando **Route** para Windows 95 o Windows 98.
- Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="examples"></a>Ejemplos
Para mostrar todo el contenido de la tabla de enrutamiento IP, escriba:
```
route print
```
Para mostrar las rutas de la tabla de enrutamiento IP que comienzan con 10, escriba:
```
route print 10.*
```
Para agregar una ruta predeterminada con la dirección de puerta de enlace predeterminada de 192.168.12.1, escriba:
```
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1
```
Para agregar una ruta al 10.41.0.0 de destino con la máscara de subred 255.255.0.0 y la dirección del próximo salto de 10.27.0.1, escriba:
```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1
```
Para agregar una ruta persistente al 10.41.0.0 de destino con la máscara de subred 255.255.0.0 y la dirección del próximo salto de 10.27.0.1, escriba:
```
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1
```
Para agregar una ruta al 10.41.0.0 de destino con la máscara de subred 255.255.0.0, la dirección del próximo salto de 10.27.0.1 y la métrica de costo 7, escriba:
```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7
```
Para agregar una ruta al 10.41.0.0 de destino con la máscara de subred 255.255.0.0, la dirección del próximo salto de 10.27.0.1 y el uso del índice de interfaz 0X3, escriba:
```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3
```
Para eliminar la ruta al 10.41.0.0 de destino con la máscara de subred 255.255.0.0, escriba:
```
route delete 10.41.0.0 mask 255.255.0.0
```
Para eliminar todas las rutas de la tabla de enrutamiento IP que comienzan por 10, escriba:
```
route delete 10.*
```
Para cambiar la dirección del próximo salto de la ruta con el destino de 10.41.0.0 y la máscara de subred de 255.255.0.0 de 10.27.0.1 a 10.27.0.25, escriba:
```
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
