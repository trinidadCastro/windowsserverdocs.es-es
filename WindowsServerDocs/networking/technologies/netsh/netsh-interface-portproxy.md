---
title: Comandos Netsh para la interfaz portproxy
description: Use los comandos de netsh interface portproxy para actuar como un proxy entre redes IPv4 e IPv6 y las aplicaciones.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 194a418fe6b33e312a3f2529e82d50d76cd15f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842486"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandos de proxy de puerto de interfaz de netsh

Use la **netsh interfaz portproxy** comandos para que actúe como proxy entre redes IPv4 e IPv6 y las aplicaciones. Puede usar estos comandos para establecer el servicio de proxy de las maneras siguientes:

-   Equipo configurado para IPv4 y la aplicación de los mensajes enviados a otras aplicaciones y los equipos configurados para IPv4.

-   Equipo configurado para IPv4 y aplicación mensajes enviados a configurado para IPv6 de equipos y aplicaciones.

-   Equipo configurado para IPv6 y aplicación mensajes enviados a la configurada de IPv4 de equipos y aplicaciones.

-   Equipo configurado para IPv6 y la aplicación de los mensajes enviados a otras aplicaciones y los equipos configurados para IPv6.

Al escribir archivos por lotes o secuencias de comandos mediante estos comandos, cada uno de ellos debe comenzar con **netsh interfaz portproxy**. Por ejemplo, cuando se usa el **eliminar v4tov6** comando para especificar que el servidor portproxy elimina un puerto y dirección IPv4 de la lista de direcciones IPv4 para el que escucha el servidor, el archivo por lotes o el script debe usar la sintaxis siguiente:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

La interfaz portproxy comandos de netsh disponibles son:

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

-   [Mostrar todo](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>Agregar v4tov4

El servidor portproxy escucha los mensajes enviados a un puerto específico y la dirección IPv4 y se asigna un puerto y IPv4 de direcciones para enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros


| | |
|-----|--------|----------|
| **listenport**     | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, en el que se va a escuchar.                                                                                                                      | Requerido |
| **connectaddress** | Especifica la dirección IPv4 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv4 que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="add-v4tov6"></a>Agregar v4tov6

El servidor portproxy escucha los mensajes enviados a un puerto específico y dirección IPv4 y se asigna un puerto e IPv6 de direcciones para enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-----------|-------------|----------|
| **listenport**     | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, en el que se va a escuchar.       | Requerido |
| **connectaddress** | Especifica la dirección IPv6 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv4 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="add-v6tov4"></a>Agregar v6tov4

El servidor portproxy escucha los mensajes enviados a un puerto específico y la dirección de IPv6 y se asigna un puerto y IPv4 de direcciones que se va a enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|------------|-------------|----------|
| **listenport**     | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, en el que se va a escuchar.              | Requerido |
| **connectaddress** | Especifica la dirección IPv4 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv6 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **protocol**       | Especifica el protocolo que se usará.      |          |

## <a name="add-v6tov6"></a>Agregar v6tov6

El servidor portproxy escucha los mensajes enviados a un puerto específico y la dirección de IPv6 y se asigna un puerto e IPv6 de direcciones que se va a enviar los mensajes recibidos después de establecer una conexión TCP independiente.

### <a name="syntax"></a>Sintaxis

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-------------|------------------|----------|
| **listenport**     | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, en el que se va a escuchar.       | Requerido |
| **connectaddress** | Especifica la dirección IPv6 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv6 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="delete-v4tov4"></a>eliminar v4tov4

El servidor portproxy elimina una dirección IPv4 de la lista de puertos y para el que escucha el servidor de direcciones IPv4.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica el puerto IPv4 a eliminar.                                                                       | Requerido |
| **listenaddress** | Especifica la dirección IPv4 para eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**      | Especifica el protocolo que se usará.                                                                           |          |

## <a name="delete-v4tov6"></a>delete v4tov6

El servidor portproxy elimina un puerto y dirección IPv4 de la lista de direcciones IPv4 para el que escucha el servidor.

### <a name="syntax"></a>Sintaxis

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica el puerto IPv4 a eliminar.                                                                       | Requerido |
| **listenaddress** | Especifica la dirección IPv4 para eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**      | Especifica el protocolo que se usará.                                                                           |          |

## <a name="delete-v6tov4"></a>delete v6tov4

El servidor portproxy elimina un puerto IPv6 y la dirección de la lista de direcciones IPv6 para el que escucha el servidor.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica el puerto IPv6 va a eliminar.                                                                       | Requerido |
| **listenaddress** | Especifica la dirección IPv6 a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**      | Especifica el protocolo que se usará.                                                                           |          |

## <a name="delete-v6tov6"></a>delete v6tov6

El servidor portproxy elimina una dirección IPv6 de la lista de direcciones IPv6 para el que escucha el servidor.

### <a name="syntax"></a>Sintaxis

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica el puerto IPv6 va a eliminar.                                                                       | Requerido |
| **listenaddress** | Especifica la dirección IPv6 a eliminar. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**      | Especifica el protocolo que se usará.                                                                           |          |

## <a name="reset"></a>Restablecer

Restablece el estado de configuración de IPv6.

### <a name="syntax"></a>Sintaxis

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifica los valores de parámetro de una entrada existente en el servidor portproxy creada con el **agregar v4tov4** comando o agrega una nueva entrada a la lista que asigna pares de puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|--------------------|---------------------------|----------|
| **listenport**     | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, en el que se va a escuchar.     | Requerido |
| **connectaddress** | Especifica la dirección IPv4 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv4 que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="set-v4tov6"></a>set v4tov6

Modifica los valores de parámetro de una entrada existente en el servidor portproxy creada con el **agregar v4tov6** comando o agrega una nueva entrada a la lista que asigna pares de puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|--------------------|---------------------|----------|
| **listenport**     | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, en el que se va a escuchar.     | Requerido |
| **connectaddress** | Especifica la dirección IPv6 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv4 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="set-v6tov4"></a>set v6tov4

Modifica los valores de parámetro de una entrada existente en el servidor portproxy creada con el **agregar v6tov4** comando o agrega una nueva entrada a la lista que asigna pares de puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|--------------------|----------------------|----------|
| **listenport**     | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, en el que se va a escuchar.      | Requerido |
| **connectaddress** | Especifica la dirección IPv4 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local. |          |
| **connectport**    | Especifica el puerto IPv4, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.              |          |
| **listenaddress**  | Especifica la dirección IPv6 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                    |          |

## <a name="set-v6tov6"></a>set v6tov6

Modifica los valores de parámetro de una entrada existente en el servidor portproxy creada con el **agregar v6tov6** comando o agrega una nueva entrada a la lista que asigna pares de puerto/dirección.

### <a name="syntax"></a>Sintaxis

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parámetros

|   |   |
|--------------------|-------------------------|----------|
| **listenport**     | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, en el que se va a escuchar.   | Requerido |
| **connectaddress** | Especifica la dirección IPv6 que se va a conectar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no se especifica una dirección, el valor predeterminado es el equipo local.  |          |
| **connectport**    | Especifica el puerto IPv6, por número de puerto o el servicio de nombre, que se va a conectar. Si **connectport** no se especifica, el valor predeterminado es el valor de **listenport** en el equipo local.               |          |
| **listenaddress**  | Especifica la dirección IPv6 en la que se va a escuchar. Los valores aceptables son direcciones IP, nombre NetBIOS del equipo o nombre DNS del equipo. Si no especifica una dirección, el valor predeterminado es el equipo local. |          |
| **protocol**       | Especifica el protocolo que se usará.                                                                                                                                                                     |          |

## <a name="show-all"></a>mostrar todas

Muestra todos los parámetros de portproxy, incluidos los pares de puerto/dirección para v4tov4, v4tov6, v6tov4 y v6tov6.

### <a name="syntax"></a>Sintaxis

`show all`


## <a name="show-v4tov4"></a>Mostrar v4tov4

Muestra los parámetros de portproxy v4tov4.

### <a name="syntax"></a>Sintaxis

`show v4tov4`

## <a name="show-v4tov6"></a>Mostrar v4tov6

Muestra los parámetros de portproxy v4tov6.

### <a name="syntax"></a>Sintaxis

`show v4tov6`

## <a name="show-v6tov4"></a>Mostrar v6tov4

Muestra los parámetros de portproxy v6tov4.

### <a name="syntax"></a>Sintaxis

`show v6tov4`


## <a name="show-v6tov6"></a>Mostrar v6tov6

Muestra los parámetros de portproxy v6tov6.

### <a name="syntax"></a>Sintaxis

`show v6tov6`

---
