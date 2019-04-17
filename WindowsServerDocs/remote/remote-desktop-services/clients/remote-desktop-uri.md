---
title: Esquema de URI de clientes de escritorio remoto
description: Obtén información sobre el esquema de identificador uniforme de recursos para los clientes de escritorio remoto
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
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297474"
---
# Compatibilidad con el esquema de identificador de recursos Universal (URI) del cliente de escritorio remoto

>Se aplica a: Windows Server, versión 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Habilitación de un esquema de identificador uniforme de recursos (URI) ofrece a los desarrolladores y profesionales de TI una manera integrar funciones de los clientes de escritorio remoto en plataformas y aumenta con la experiencia del usuario, ya que permite: 

- Aplicaciones de terceros a Escritorio remoto de Microsoft y se iniciará sesiones remotas con valores predefinidos (proporcionados como parte de la cadena de URI).
- Usuarios finales para iniciar las conexiones remotas con direcciones URL preconfiguradas.

>[!NOTE]
> Un URI para conectar con el cliente de escritorio remoto no se admite para los sistemas operativos de Windows: usar el URI con dispositivos Android, iOS y Mac OS.

Escritorio remoto de Microsoft usa los rdp://query_string de esquema URI para almacenar la configuración de atributo preconfigurada que se usa al iniciar al cliente. Las cadenas de consulta representan una única o un conjunto de atributos RDP proporcionados en la dirección URL. 

Los atributos RDP se separan mediante el símbolo de y comercial (&). Por ejemplo, cuando se conecta a un equipo, la cadena es:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

En esta tabla encontrarás una lista completa de atributos admitidos que puede usarse con los iOS, Mac y Android clientes de escritorio remoto. (Una "x" en la columna de la plataforma indica que el atributo es compatible. Los valores indicados comillas angulares (<>) representan los valores que son compatibles con los clientes de escritorio remoto).

| **Atributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| permitir la composición del escritorio = i:&lt;0 o 1&gt;                    | x       | x   | x   |
| permitir que el suavizado de fuentes = i:<0 o 1&gt;                         | x       | x   | x   |
| Alternar shell = s:&lt;cadena&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0, 1 o 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [nivel de autenticación = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| conectarse a la consola = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| deshabilitar la configuración de cursor = i:&lt;0 o 1&gt;                      | x       | x   | x   |
| deshabilitar el arrastre de ventana completa = i:&lt;0 o 1&gt;                     | x       | x   | x   |
| deshabilitar el menú anims = i:&lt;0 o 1&gt;                           | x       | x   | x   |
| deshabilitar temas = i:&lt;0 o 1&gt;                               | x       | x   | x   |
| deshabilitar el tapiz del escritorio = i:&lt;0 o 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (este es el único valor admitido) | x       | x   |     |
| [desktopheight = i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valor en píxeles&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [dominio = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [dirección completa = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;cadena&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 o 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [solicitar credenciales de cliente = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;cadena&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 o 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;cadena&gt;         | x | x | x |
| directorio de trabajo de shell = s:&lt;cadena&gt;          | x | x | x |
| Nombre del servidor de redirección de uso = i:&lt;0 o 1&gt;      | x | x | x |
| [nombre de usuario = s:&lt;cadena&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [Id. de modo de pantalla = i:&lt;1 o 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sesión bpp = i:&lt;8, 15, 16, 24 o 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [usar multimon = i:&lt;0 o 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |