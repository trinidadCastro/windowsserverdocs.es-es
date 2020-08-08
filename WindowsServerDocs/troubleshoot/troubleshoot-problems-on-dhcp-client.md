---
title: Solución de problemas en el cliente DHCP
description: Este artilce presenta cómo solucionar problemas en el cliente DHCP y recopilar datos.
ms.service: na
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 650b3f83ebd0467df2a747d865db2d0a346bcddc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954582"
---
# <a name="troubleshoot-problems-on-the-dhcp-client"></a>Solución de problemas en el cliente DHCP

En este artículo se describe cómo solucionar problemas que se producen en clientes DHCP.

## <a name="troubleshooting-checklist"></a>Lista de comprobación de solución de problemas

Compruebe los siguientes dispositivos y configuraciones:

  - Los cables están conectados y funcionan.

  - El filtrado de MAC está habilitado en los conmutadores a los que está conectado el cliente.

  - El adaptador de red está habilitado.

  - El controlador de adaptador de red correcto está instalado y actualizado.

  - El servicio cliente DHCP se ha iniciado y está en ejecución. Para comprobarlo, ejecute el comando **net start** y busque **cliente DHCP**.

  - No hay puertos de bloqueo de Firewall 67 y 68 UDP en el equipo cliente.

## <a name="event-logs"></a>Registros de eventos

Examine los registros de eventos de eventos de administrador, eventos de cliente de Microsoft-Windows-DHCP/operativos y Microsoft-Windows-DHCP. Todos los eventos relacionados con el servicio de cliente DHCP se envían a estos registros de eventos.
Los eventos de cliente de Microsoft-Windows-DHCP se encuentran en el Visor de eventos en **registros de aplicaciones y servicios**.

El comando de PowerShell "Get-NetAdapter-IncludeHidden" proporciona la información necesaria para interpretar los eventos que se enumeran en los registros. Por ejemplo, identificador de interfaz, dirección MAC, etc.

## <a name="data-collection"></a>Recopilación de datos

Se recomienda que recopile datos simultáneamente en el lado cliente y en el servidor DHCP cuando se produzca el problema. Sin embargo, en función del problema real, también puede iniciar la investigación mediante el uso de un solo conjunto de datos en el cliente DHCP o en el servidor DHCP.

Para recopilar datos del servidor y del cliente afectado, use [Wireshark](https://www.wireshark.org/download.html). Comience a recopilar al mismo tiempo en el cliente DHCP y en los equipos del servidor DHCP.

Ejecute los siguientes comandos en el cliente que está experimentando el problema:

```console
ipconfig /release
ipconfig /renew
```

A continuación, detenga Wireshark en el cliente y el servidor. Compruebe los seguimientos generados. Estos deberían, al menos, indicar en qué fase se detiene la comunicación.