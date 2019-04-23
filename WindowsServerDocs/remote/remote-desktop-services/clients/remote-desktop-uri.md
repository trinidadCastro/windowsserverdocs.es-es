---
title: Esquema de URI de clientes de escritorio remoto
description: Obtenga información sobre el esquema de identificador uniforme de recursos para los clientes de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: 86de6468e2fa45c976711aef43a1a274e04498d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870506"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Compatibilidad con el esquema del identificador de recursos Universal (URI) del cliente de escritorio remoto

>Se aplica a: Windows Server, versión 1803, Windows Server 2016, Windows Server 2012 R2

Con un esquema de identificador uniforme de recursos (URI), los desarrolladores y profesionales de TI pueden integrar las características de los clientes de Escritorio remoto de diversas de plataformas y enriquecer la experiencia del usuario: 

- Las aplicaciones de terceros pueden iniciar el Escritorio remoto de Microsoft e iniciar sesiones remotas con configuraciones predefinidas (proporcionadas como parte de la cadena de URI).
- Los usuarios finales pueden iniciar conexiones remotas mediante direcciones URL configuradas previamente.

>[!NOTE]
> NO se admite el uso de un URI para conectar con el cliente de escritorio remoto para los sistemas operativos de Windows: use el URI con dispositivos Android, iOS y MacOS.

Escritorio remoto de Microsoft usa el rdp://query_string del esquema URI para almacenar la configuración de atributos preconfigurados que se utiliza al iniciar al cliente. Las cadenas de consulta representan un único atributo o un conjunto de atributos RDP que se proporcionan en la dirección URL. 

Los atributos RDP se separan mediante el símbolo de "y" comercial (&). Por ejemplo, al conectarse a un equipo, la cadena es:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabla proporciona una lista completa de los atributos compatibles que pueden usarse con clientes de Escritorio remoto de iOS, Mac y Android. (Una “x” en la columna de plataforma indica que el atributo es compatible. Los valores indicados por comillas angulares (<>) representan los valores que son compatibles con los clientes de Escritorio remoto).

| **Atributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Permitir composición del escritorio = i:&lt;0 o 1&gt;                    | x       | x   | x   |
| Permitir suavizado de fuentes = i: < 0 o 1&gt;                         | x       | x   | x   |
| Alternar shell = s:&lt;cadena&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [nivel de autenticación = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| conectarse a la consola = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| deshabilitar la configuración de cursor = i:&lt;0 o 1&gt;                      | x       | x   | x   |
| deshabilitar arrastrar ventana completa = i:&lt;0 o 1&gt;                     | x       | x   | x   |
| deshabilitar menús anims = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| deshabilitar themes = i:&lt;0 o 1&gt;                               | x       | x   | x   |
| deshabilitar wallpaper = i:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (este es el único valor admitido) | x       | x   |     |
| [desktopheight = i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [dominio = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [dirección completa = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [Pedir credenciales en el cliente = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| directorio de trabajo de shell = s:&lt;cadena&gt;          | x | x | x |
| Usar el nombre de servidor redirección = i:&lt;0 o 1&gt;      | x | x | x |
| [username=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [identificador del modo de pantalla = i:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sesión bpp = i:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [Use multimon = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |