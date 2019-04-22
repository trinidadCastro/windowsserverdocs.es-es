---
title: Usar supervisión y cuentas de acceso remoto
description: Este tema forma parte de la Guía de supervisión de acceso remoto y las cuentas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92519b49-0df4-43c1-9717-f13570644212
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 454bc8dc5a9cbf8dc4e759196a13e7920de2eaf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823526"
---
# <a name="use-remote-access-monitoring-and-accounting"></a>Usar supervisión y cuentas de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La supervisión de acceso remoto notifica la actividad y el estado de los usuarios remotos para las conexiones de DirectAccess y VPN. Controla el número y la duración de las conexiones de cliente (entre otras estadísticas) y supervisa las operaciones de estado del servidor. Una consola de supervisión fácil de usar proporciona una vista de toda la infraestructura de acceso remoto. Las vistas de supervisión están disponibles para configuraciones de un solo servidor, clúster y multisitio.  
  
**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas sobre la supervisión del acceso remoto.  
>   
> -   [Supervisar la carga existente en el servidor de acceso remoto](Monitor-the-existing-load-on-the-Remote-Access-server.md)  
> -   [Supervisar el estado de distribución de la configuración del servidor de acceso remoto](Monitor-the-configuration-distribution-status-of-the-Remote-Access-server.md)  
> -   [Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)  
> -   [Identificar y resolver problemas de las operaciones del servidor de acceso remoto](Identify-and-resolve-Remote-Access-server-operations-problems.md)  
> -   [Supervisar clientes remotos conectados para la actividad y el estado](Monitor-connected-remote-clients-for-activity-and-status.md)  
> -   [Generar un informe de uso para clientes remotos mediante datos históricos](Generate-a-usage-report-for-remote-clients-using-historical-data.md)  

## <a name="in-this-guide"></a>En esta guía  
Este documento contiene instrucciones para aprovechar las funcionalidades de supervisión de acceso remoto mediante la consola de administración de DirectAccess y los correspondientes cmdlets de Windows PowerShell, que se proporcionan como parte del rol de servidor de acceso remoto.  
  
Se explican los siguientes escenarios de supervisión y contabilización:  
  
1.  Supervisar la carga existente en el servidor de acceso remoto  
  
2.  Supervisar el estado de distribución de la configuración del servidor de acceso remoto  
  
3.  Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes  
  
4.  Identificar y resolver los problemas de operaciones del servidor de acceso remoto  
  
5.  Supervisar la actividad y el estado de los clientes remotos conectados  
  
6.  Generar un informe de uso para clientes remotos mediante datos históricos  
  
### <a name="understand-monitoring-and-accounting"></a>Descripción de la supervisión y administración de cuentas  
Antes de comenzar a supervisar y contabilizar las tareas de clientes remotos, debe comprender la diferencia entre los dos.  
  
-   La **supervisión** muestra los usuarios conectados activamente en un momento del tiempo determinado.  
  
-   La **contabilización** guarda un historial de los usuarios que se han conectado a la red corporativa y sus detalles de uso (con fines de cumplimiento normativo y auditoría).  
  
La supervisión de clientes remotos se basa en las conexiones. Hay dos tipos de conexiones de túnel que los clientes de DirectAccess establecen:  
  
-   **Conexiones de tráfico de túnel de equipo**: este túnel lo establece el equipo, en el contexto del sistema, para acceder a los servidores que se necesitan para la resolución de nombres, autenticación, actualización de remedios, etc.  
  
-   **Conexiones de tráfico de túnel de usuario**: este túnel lo establece la cuenta de usuario en el equipo, en un contexto de usuario, cuando este intenta acceder a un recurso en la red corporativa. En función de los requisitos de implementación, un usuario podría tener que proporcionar credenciales seguras (por ejemplo, mediante una tarjeta inteligente o con una contraseña de un solo uso) para acceder a los recursos de la red corporativa.  
  
En el caso de DirectAccess, una conexión se identifica de forma única por la dirección IP del cliente remoto. Por ejemplo, si un túnel de equipo está abierto para un equipo cliente y un usuario se conecta desde ese equipo, estos sería utilizar la misma conexión. Si el usuario se desconecta y se vuelve a conectar mientras el túnel de equipo está activo, es una sola conexión.  
  


