---
title: Comandos de proxy de puerto de la interfaz de netsh
description: Usa los comandos de proxy de puerto de la interfaz de netsh para que funcionen como servidores proxy entre las redes y aplicaciones IPv4 e IPv6.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 6244213ea689f07230ce53288e52959972112037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405503"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandos de proxy de puerto de la interfaz de netsh

Usa los comandos de **proxy de puerto de la interfaz de netsh** para que funcionen como servidores proxy entre las redes y aplicaciones IPv4 e IPv6. Puedes usar estos comandos para establecer el servicio de proxy de las siguientes maneras:

-   Mensajes de equipos y aplicaciones configurados con IPv4 enviados a otros equipos y aplicaciones configurados con IPv4.

-   Mensajes de equipos y aplicaciones configurados con IPv4 enviados a equipos y aplicaciones configurados con IPv6.

-   Mensajes de equipos y aplicaciones configurados con IPv6 enviados a equipos y aplicaciones configurados con IPv4.

-   Mensajes de equipos y aplicaciones configurados con IPv6 enviados a otros equipos y aplicaciones configurados con IPv6.

Al escribir archivos por lotes o scripts con estos comandos, cada comando debe comenzar con **netsh interface portproxy**. Por ejemplo, al usar el comando **delete v4tov6** para especificar que el servidor proxy de puerto elimina un puerto y una dirección IPv4 de la lista de direcciones IPv4 en las que el servidor escucha, el archivo por lotes o script deben usar la siguiente sintaxis:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Los comandos disponibles de proxy de puerto de la interfaz de netsh son:

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [show all](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

El servidor proxy de puerto escucha los mensajes enviados a un puerto y una dirección IPv4 específicos y asigna un puerto y una dirección IPv4 para enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv4, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv4 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv4, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv4 a la cual escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="add-v4tov6"></a>add v4tov6

El servidor proxy de puerto escucha los mensajes enviados a un puerto y una dirección IPv4 específicos y asigna un puerto y una dirección IPv6 para enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv4, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv6 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv6, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv4 en la cual escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="add-v6tov4"></a>add v6tov4

El servidor proxy de puerto escucha los mensajes enviados a un puerto y una dirección IPv6 específicos y asigna un puerto y una dirección IPv4 a la cual enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv6, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv4 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv4, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv6 a la que se va a escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="add-v6tov6"></a>add v6tov6

El servidor proxy de puerto escucha los mensajes enviados a un puerto y una dirección IPv6 específicos y asigna un puerto y una dirección IPv6 a la cual enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv6, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv6 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv6, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv6 a la que se va a escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="delete-v4tov4"></a>delete v4tov4

El servidor proxy de puerto elimina una dirección IPv4 de la lista de puertos y direcciones IPv4 en los que el servidor escucha.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica el puerto IPv4 que se va a eliminar.                                    |
| **listenaddress** | Especifica la dirección IPv4 que se va a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|   **protocol**    |                                      Especifica el protocolo que se va a usar.                                      |

## <a name="delete-v4tov6"></a>delete v4tov6

El servidor proxy de puerto elimina un puerto y una dirección IPv4 de la lista de direcciones IPv4 en las que el servidor escucha.

### <a name="syntax"></a>Sintaxis

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica el puerto IPv4 que se va a eliminar.                                    |
| **listenaddress** | Especifica la dirección IPv4 que se va a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|   **protocol**    |                                      Especifica el protocolo que se va a usar.                                      |

## <a name="delete-v6tov4"></a>delete v6tov4

El servidor proxy de puerto elimina un puerto y una dirección IPv6 de la lista de direcciones IPv6 en las que el servidor escucha.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica el puerto IPv6 que se va a eliminar.                                    |
| **listenaddress** | Especifica la dirección IPv6 que se va a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|   **protocol**    |                                      Especifica el protocolo que se va a usar.                                      |

## <a name="delete-v6tov6"></a>delete v6tov6

El servidor proxy de puerto elimina una dirección IPv6 de la lista de direcciones IPv6 en las que el servidor escucha.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica el puerto IPv6 que se va a eliminar.                                    |
| **listenaddress** | Especifica la dirección IPv6 que se va a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|   **protocol**    |                                      Especifica el protocolo que se va a usar.                                      |

## <a name="reset"></a>reset

Restablece el estado de configuración de IPv6.

### <a name="syntax"></a>Sintaxis

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifica los valores de parámetro de una entrada existente en el servidor proxy de puerto creada con el comando **add v4tov4**, o agrega una nueva entrada a la lista, que asigna pares puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv4, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv4 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv4, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv4 a la cual escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

Modifica los valores de parámetro de una entrada existente en el servidor proxy de puerto creada con el comando **add v4tov6**, o agrega una nueva entrada a la lista, que asigna pares puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv4, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv6 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv6, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv4 en la cual escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

Modifica los valores de parámetro de una entrada existente en el servidor proxy de puerto creada con el comando **add v6tov4**, o agrega una nueva entrada a la lista, que asigna pares puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica el puerto IPv6, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv4 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |
|  **connectport**   |       Especifica el puerto IPv4, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv6 a la que se va a escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|    **protocol**    |                                                                                  Especifica el protocolo que se va a usar.                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

Modifica los valores de parámetro de una entrada existente en el servidor proxy de puerto creada con el comando **add v6tov6**, o agrega una nueva entrada a la lista, que asigna pares puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Especifica el puerto IPv6, por número de puerto o nombre de servicio, en el que se va a escuchar.                                                            |
| **connectaddress** | Especifica la dirección IPv6 a la que se va a conectar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |
|  **connectport**   |        Especifica el puerto IPv6, por número de puerto o nombre de servicio, al cual conectarse. Si no se especifica **connectport**, el valor predeterminado es el valor de **listenport** en el equipo local.        |
| **listenaddress**  | Especifica la dirección IPv6 a la que se va a escuchar. Los valores aceptables son la dirección IP, el nombre de NetBIOS del equipo o el nombre DNS del equipo. Si no especificas una dirección, el valor predeterminado es el equipo local. |
|    **protocol**    |                                                                                   Especifica el protocolo que se va a usar.                                                                                   |

## <a name="show-all"></a>mostrar todas

Muestra todos los parámetros de proxy de puerto, incluidos los pares de puerto/dirección para v4tov4, v4tov6, v6tov4 y v6tov6.

### <a name="syntax"></a>Sintaxis

`show all`


## <a name="show-v4tov4"></a>show v4tov4

Muestra los parámetros de proxy de puerto de v4tov4.

### <a name="syntax"></a>Sintaxis

`show v4tov4`

## <a name="show-v4tov6"></a>show v4tov6

Muestra los parámetros de proxy de puerto de v4tov6.

### <a name="syntax"></a>Sintaxis

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

Muestra los parámetros de proxy de puerto de v6tov4.

### <a name="syntax"></a>Sintaxis

`show v6tov4`


## <a name="show-v6tov6"></a>show v6tov6

Muestra los parámetros de proxy de puerto de v6tov6.

### <a name="syntax"></a>Sintaxis

`show v6tov6`

---
