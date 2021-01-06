---
title: Administrar clientes de DirectAccess de forma remota
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 36255d80-a13e-4af7-a5c0-ab4c8f302622
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 86824aaba3e249aa5cc4f0743d28ce0371481c21
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947741"
---
# <a name="manage-directaccess-clients-remotely"></a>Administrar clientes de DirectAccess de forma remota

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La supervisión de acceso remoto notifica la actividad y el estado de los usuarios remotos para las conexiones de DirectAccess y VPN. Controla el número y la duración de las conexiones de cliente (entre otras estadísticas) y supervisa las operaciones de estado del servidor. Una consola de supervisión fácil de usar proporciona una vista de toda la infraestructura de acceso remoto. Las vistas de supervisión están disponibles para configuraciones de un solo servidor, clúster y multisitio.

**Nota:** Windows Server 2016 combina DirectAccess y el servicio de acceso remoto (RAS) en un solo rol de acceso remoto.

## <a name="in-this-guide"></a>En esta guía
Este documento contiene instrucciones para aprovechar las funcionalidades de supervisión de acceso remoto mediante la consola de administración de DirectAccess y los correspondientes cmdlets de Windows PowerShell, que se proporcionan como parte del rol de servidor de acceso remoto.

Se explican los siguientes escenarios de supervisión y contabilización:

1.  Supervisar la carga existente en el servidor de acceso remoto

2.  Supervisar el estado de distribución de la configuración del servidor de acceso remoto

3.  Supervisar el estado de las operaciones de acceso remoto y de sus componentes

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



