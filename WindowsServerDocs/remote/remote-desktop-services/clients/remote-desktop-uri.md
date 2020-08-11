---
title: Esquema de URI de Escritorio remoto
description: Obtén información sobre el esquema de identificador uniforme de recursos para los clientes de Escritorio remoto
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 082943114d00d9325e81edbd5cbeff14ba413703
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955092"
---
# <a name="remote-desktop-uri-scheme"></a>Esquema de URI de Escritorio remoto

> Se aplica a: Windows Server, versión 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

En este documento se define el formato de los identificadores uniformes de recursos (URI) para Escritorio remoto. Estos esquemas de URI permiten invocar a los clientes de Escritorio remoto con varios comandos.

## <a name="ms-rd-uri-scheme"></a>Esquema de URI ms-rd

>[!NOTE]
> El esquema de URI ms-rd solo se admite actualmente con el cliente de Escritorio de Windows (MSRDC).

El URI de ms-rd ofrece la opción de especificar un comando para el cliente y un conjunto de parámetros específicos del comando con el siguiente formato:

```
ms-rd:command?parameters
```

Los parámetros usan el formato de cadena de consulta de par clave=valor, separado por & para ofrecer información adicional para el comando dado:

```
param1=value1&param2=value2&…
```

### <a name="commands-and-parameters"></a>Comandos y parámetros

Esta es la lista de comandos admitidos actualmente y sus parámetros correspondientes.

El uso de `ms-rd:` sin ningún comando inicia el cliente.

#### <a name="subscribe"></a>Subscribe

Este comando inicia el cliente e inicia el proceso de suscripción.

**Nombre del comando:** subscribe

**Parámetros del comando:**

| Parámetro | Descripción                  | Valores |
|-----------|------------------------------|--------|
| url       | Especifica la dirección URL del área de trabajo. | Una dirección URL válida, por ejemplo: <https://contoso.com>. |

**Ejemplo:** ms-rd:subscribe?url=https://contoso.com

## <a name="legacy-rdp-uri-scheme"></a>Esquema de URI rdp heredado

>[!NOTE]
> El siguiente esquema de URI solo es compatible con los clientes de dispositivos macOS, iOS y Android. Se está reemplazando por el nuevo URI de ms-rd anterior.

Escritorio remoto de Microsoft usa el esquema de URI rdp://query_string para almacenar la configuración de atributos preconfigurados que se usa al iniciar el cliente. Las cadenas de consulta representan un único atributo o un conjunto de atributos RDP que se proporcionan en la dirección URL.

Los atributos RDP se separan mediante el símbolo de "y" comercial (&). Por ejemplo, al conectarse a un equipo, la cadena es:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabla proporciona una lista completa de los atributos compatibles que pueden usarse con clientes de Escritorio remoto de iOS, Mac y Android. (Una "x" en la columna de plataforma indica que el atributo es compatible. Los valores indicados por comillas angulares (<>) representan los valores que son compatibles con los clientes de Escritorio remoto).

| Atributo RDP                                           | Android | Mac | iOS |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 o 1&gt;              | x       | x   | x   |
| allow font smoothing=i:<0 o 1&gt;                      | x       | x   | x   |
| alternate shell=s:&lt;cadena&gt;                        | x       | x   | x   |
| [audiomode=i:&lt;0, 1 o 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393707(v=ws.10)) | x       | x   | x   |
| [authentication level=i:&lt;0 o 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393709(v=ws.10)) | x       | x   | x   |
| connect to console=i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disable cursor settings=i:&lt;0 o 1&gt;                | x       | x   | x   |
| disable full window drag=i:&lt;0 o 1&gt;               | x       | x   | x   |
| disable menu anims=i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disable themes=i:&lt;0 o 1&gt;                         | x       | x   | x   |
| disable wallpaper=i:&lt;0 o 1&gt;                      | x       | x   | x   |
| [drivestoredirect = s: *](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393728(v=ws.10)) (este es el único valor admitido) | x       | x   |     |
| [desktopheight=i:&lt;valor en píxeles&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393702(v=ws.10)) |         | x   |     |
| [desktopwidth=i:&lt;valor en píxeles&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393697(v=ws.10))  |         | x   |     |
| [domain=s:&lt;cadena&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393673(v=ws.10))                 | x | x | x |
| [full address=s:&lt;cadena&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393661(v=ws.10))           | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 o 2&gt;](/windows/win32/termserv/imsrdpclienttransportsettings-gatewayusagemethod)                | x | x | x |
| [prompt for credentials on client=i:&lt;0 o 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393660(v=ws.10)) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393684(v=ws.10))                  | x | x | x |
| [redirectprinters=i:&lt;0 o 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393671(v=ws.10))                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;cadena&gt;         | x | x | x |
| shell working directory=s:&lt;cadena&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;cadena&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393678(v=ws.10))                  | x | x | x |
| [screen mode id=i:&lt;1 o 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393692(v=ws.10))            |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 o 32&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393680(v=ws.10)) |   | x |   |
| [use multimon=i:&lt;0 o 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393695(v=ws.10))              |   | x |   |
