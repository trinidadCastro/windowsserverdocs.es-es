---
title: Compatibilidad con implementaciones de gran tamaño
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860486"
---
#<a name="support-for-larger-deployments"></a>Compatibilidad con implementaciones de gran tamaño

>Se aplica a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Las características descritas en este tema solo funcionan en Windows Server 2016 con el rol experiencia con Essentials habilitado y no con la SKU de Windows Server 2016 Essentials.


Windows Server Essentials ahora admite implementaciones más grandes con:

- varios dominios
- varios controladores de dominio
- capacidad de especificar un controlador de dominio designado
- compatibilidad con hasta 500 usuarios y 500 dispositivos

##<a name="support-for-multiple-domains"></a>Compatibilidad con varios dominios

Servidor Windows 2012 R2 Essentials admite solo un dominio por servidor, que es necesario, y el servidor de Essentials debe ser la raíz del bosque. Mientras que un dominio y bosque siguen siendo necesarias, ahora puede implementarse el rol experiencia con Windows Server 2016 Essentials en Windows Server 2016 Standard o Datacenter para admitir varios dominios.

## <a name="support-for-multiple-domain-controllers"></a>Compatibilidad con varios controladores de dominio

 Windows Server Essentials 2012 R2 bloquea todos los servicios que utilizan Azure Active Directory, como Office 365, donde se implementa más de un controlador de dominio. El motivo es que las credenciales de cuenta puede provocar cuenta y contraseña la sincronización entre los controladores de dominio local y Azure Active Directory con contraseñas que no están sincronizadas. Esta limitación se ha quitado en Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Capacidad de especificar un controlador de dominio designado

Ahora puede elegir un controlador de dominio designado que se mejorar los tiempos de recuperación de objetos de dominio de Active Directory, así como coordinar la sincronización de cambio de la cuenta a través de otros controladores de dominio en el dominio.

Designada el controlador de dominio predeterminado será el mismo servidor que ejecuta el rol de servidor experiencia con Windows Server Essentials. Si ese servidor es un servidor miembro, lo que significa que no es un controlador de dominio y, después, el valor predeterminado se determinará automáticamente el controlador de dominio designado en función de pruebas qué controlador de dominio en el dominio tiene la menor latencia de red al servidor que ejecuta el Rol de servidor de experiencia de Windows Server. Si desea cambiar manualmente el servidor que es el controlador de dominio designado, puede hacer en **configuración** en el **panel de Windows Server Essentials** tal como se muestra a continuación.

![Una captura de pantalla que muestra los valores de control panel en primer plano y el panel de Windows Server Essentials en segundo plano. La página de controlador de dominio designado de la configuración del panel de control está seleccionada actualmente.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Compatibilidad con 500 usuarios y 500 dispositivos
-------------------------------------

El número máximo de usuarios admitidos y los dispositivos de Windows Server 2012 R2 Essentials es 25 y 50, respectivamente. Con la introducción del rol de servidor Windows Server Essentials Experience, ese límite se ha aumentado a 100 usuarios y 200 dispositivos.

Es compatible con Windows Server 2016 Essentials 500 dispositivos y 500 usuarios. Por lo que es posible es una actualización para el marco de proveedores y los controles de lista de objetos para poder almacenar en caché y presentar rápidamente grandes listas de objetos de usuario y dispositivo. Además, se agregó una característica de búsqueda y filtrado para encontrar rápidamente el usuario o dispositivo que esté buscando (Véase la figura 5-2). Encontrará la funcionalidad de búsqueda y filtrado en el **panel de Windows Server Essentials**, **acceso Web remoto**y la tienda Windows y Windows Phone **My Server** aplicaciones.

![Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c." Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.](media/larger-deployments-2.PNG)

Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c". Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.

> [!NOTE]  
> Mientras ha aumentado el límite de usuarios y dispositivos compatible para el rol de servidor Windows Server Essentials, el límite admitido para sigue siendo de copia de seguridad de cliente en 75.

<a name="see-also"></a>Vea también
--------
[Introducción a Windows Server Essentials](get-started.md)