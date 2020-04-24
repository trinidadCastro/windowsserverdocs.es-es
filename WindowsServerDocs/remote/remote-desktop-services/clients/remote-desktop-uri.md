---
title: Esquema de URI de clientes de Escritorio remoto
description: Obtén información sobre el esquema de identificador uniforme de recursos para los clientes de Escritorio remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: 02f970cb2e793c1e342a2818a2bca3900327fa9c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80856008"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Compatibilidad del esquema de identificador universal de recursos (URI) para clientes de Escritorio remoto

>Se aplica a: Windows Server, versión 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Con un esquema de identificador uniforme de recursos (URI), los desarrolladores y profesionales de TI pueden integrar las características de los clientes de Escritorio remoto de diversas de plataformas y enriquecer la experiencia del usuario: 

- Las aplicaciones de terceros pueden iniciar el Escritorio remoto de Microsoft e iniciar sesiones remotas con configuraciones predefinidas (proporcionadas como parte de la cadena de URI).
- Los usuarios finales pueden iniciar conexiones remotas mediante direcciones URL configuradas previamente.

>[!NOTE]
> NO se admite el uso de un URI para conectarse con el cliente de Escritorio remoto para los sistemas operativos de Windows: usa el URI con dispositivos Android, iOS y MacOS.

Escritorio remoto de Microsoft usa el esquema URI rdp://query_string para almacenar la configuración de atributos preconfigurados que se usa al iniciar el cliente. Las cadenas de consulta representan un único atributo o un conjunto de atributos RDP que se proporcionan en la dirección URL. 

Los atributos RDP se separan mediante el símbolo de "y" comercial (&). Por ejemplo, al conectarse a un equipo, la cadena es:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabla proporciona una lista completa de los atributos compatibles que pueden usarse con clientes de Escritorio remoto de iOS, Mac y Android. (Una "x" en la columna de plataforma indica que el atributo es compatible. Los valores indicados por comillas angulares (<>) representan los valores que son compatibles con los clientes de Escritorio remoto).

| **Atributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 o 1&gt;                    | x       | x   | x   |
| allow font smoothing=i:<0 o 1&gt;                         | x       | x   | x   |
| alternate shell=s:&lt;cadena&gt;                              | x       | x   | x   |
| [audiomode=i:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [authentication level=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connect to console=i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disable cursor settings=i:&lt;0 o 1&gt;                      | x       | x   | x   |
| disable full window drag=i:&lt;0 o 1&gt;                     | x       | x   | x   |
| disable menu anims=i:&lt;0 o 1&gt;                           | x       | x   | x   |
| disable themes=i:&lt;0 o 1&gt;                               | x       | x   | x   |
| disable wallpaper=i:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (este es el único valor admitido) | x       | x   |     |
| [desktopheight=i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth=i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [full address=s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt for credentials on client=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;cadena&gt;         | x | x | x |
| shell working directory=s:&lt;cadena&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [screen mode id=i:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [use multimon=i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |