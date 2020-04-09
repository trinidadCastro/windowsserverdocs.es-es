---
title: Usar supervisión y cuentas de acceso remoto
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 92519b49-0df4-43c1-9717-f13570644212
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 03d367f2f981ca3ed649f1ca4d5eca23967c9cfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860518"
---
# <a name="use-remote-access-monitoring-and-accounting"></a>Usar supervisión y cuentas de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La supervisión de acceso remoto notifica la actividad y el estado de los usuarios remotos para las conexiones de DirectAccess y VPN. Controla el número y la duración de las conexiones de cliente (entre otras estadísticas) y supervisa las operaciones de estado del servidor. Una consola de supervisión fácil de usar proporciona una vista de toda la infraestructura de acceso remoto. Las vistas de supervisión están disponibles para configuraciones de un solo servidor, clúster y multisitio.  
  
**Nota:** Windows Server 2012 combina DirectAccess y el Servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas sobre la supervisión del acceso remoto.  
>   
> -   [Supervisar la carga existente en el servidor de acceso remoto](Monitor-the-existing-load-on-the-Remote-Access-server.md)  
> -   [Supervisar el estado de distribución de la configuración del servidor de acceso remoto](Monitor-the-configuration-distribution-status-of-the-Remote-Access-server.md)  
> -   [Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)  
> -   [Identificar y resolver los problemas de operaciones del servidor de acceso remoto](Identify-and-resolve-Remote-Access-server-operations-problems.md)  
> -   [Supervisar la actividad y el estado de los clientes remotos conectados](Monitor-connected-remote-clients-for-activity-and-status.md)  
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
  
-   **Conexiones de tráfico de túnel de equipo**: este túnel lo establece el equipo, en el contexto del sistema, para obtener acceso a los servidores necesarios para la resolución de nombres, la autenticación, la actualización de correcciones, etc.  
  
-   **Conexiones de tráfico de túnel de usuario**: este túnel lo establece la cuenta de usuario en el equipo, en un contexto de usuario, cuando el usuario intenta obtener acceso a un recurso de la red corporativa. En función de los requisitos de implementación, un usuario podría tener que proporcionar credenciales seguras (por ejemplo, mediante una tarjeta inteligente o con una contraseña de un solo uso) para acceder a los recursos de la red corporativa.  
  
En el caso de DirectAccess, una conexión se identifica de forma única por la dirección IP del cliente remoto. Por ejemplo, si un túnel de equipo está abierto para un equipo cliente y un usuario está conectado desde ese equipo, se utilizará la misma conexión. Si el usuario se desconecta y se vuelve a conectar mientras el túnel de equipo está activo, es una sola conexión.  
  


