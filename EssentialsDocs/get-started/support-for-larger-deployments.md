---
title: "Soporte técnico para realizar implementaciones grandes"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>Soporte técnico para realizar implementaciones grandes

>Se aplica a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Las características descritas en este tema solo funcionan en Windows Server 2016 con el rol de experiencia de Essentials habilitado y no con la SKU de Windows Server 2016 Essentials.


Windows Server Essentials ahora admite implementaciones de mayor tamaño con:

- varios dominios
- varios controladores de dominio
- capacidad para especificar un controlador de dominio designado
- soporte técnico para usuarios de hasta 500 y 500 dispositivos

##<a name="support-for-multiple-domains"></a>Soporte para varios dominios

Windows server 2012 R2 Essentials admite un solo dominio por servidor, lo que es necesario, y el servidor de Essentials debe ser la raíz del bosque. Mientras que un dominio y bosque sigue siendo necesarios, el rol de Windows Server 2016 Essentials Experience puede implementarse ahora en Windows Server 2016 Standard o Datacenter para admitir varios dominios.

## <a name="support-for-multiple-domain-controllers"></a>Compatibilidad con varios controladores de dominio

 Windows Server Essentials 2012 R2 bloquea los servicios que sacan partido de Azure Active Directory, como Office 365, donde se implementa más de un controlador de dominio. El motivo es que puede provocar cuenta y contraseña de la sincronización entre los controladores de dominio local y Azure Active Directory a las credenciales de cuenta con las contraseñas que están sincronizadas. Esta limitación se ha quitado en Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Capacidad para especificar un controlador de dominio designado

Ahora puedes elegir un controlador de dominio designado que se mejorar los tiempos de recuperación para objetos de dominio de Active Directory, así como la sincronización de cambio en la cuenta de coordenadas a través de otros controladores de dominio del dominio.

Tu controlador de dominio predeterminada será el mismo servidor que ejecuta el rol de servidor de Windows Server Essentials Experience. Si ese servidor es un servidor miembro, lo que significa que no es un controlador de dominio, a continuación, el valor predeterminado controlador de dominio designado se determinará automáticamente en función de pruebas qué controlador de dominio en el dominio es la menor latencia de red al servidor que ejecuta el rol de servidor de experiencia de Windows Server. Si quieres cambiar manualmente qué servidor es el controlador de dominio designado, puede hacerlo en **configuración** en la **panel de Windows Server Essentials** como se muestra a continuación.

![Captura de pantalla que muestra la configuración de control panel en primer plano y el panel de Windows Server Essentials en segundo plano. La página de controlador de dominio designado la configuración del panel de control está seleccionada actualmente.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Soporte técnico para usuarios de 500 y 500 dispositivos
-------------------------------------

El número máximo de usuarios admitidos y dispositivos de Windows Server 2012 R2 Essentials es 25 y 50, respectivamente. Con la introducción del rol de servidor de Windows Server Essentials Experience, ese límite se ha aumentado a 100 usuarios y dispositivos de 200.

Windows Server 2016 Essentials admite 500 usuarios y dispositivos de 500. Convertir este posibles es una actualización para el marco de proveedor y controles de lista de objetos para que se almacena en caché y representan rápidamente largas listas de objeto de usuario y del dispositivo. Además, se ha agregado una característica de búsqueda y filtrado para encontrar rápidamente el usuario o dispositivo que puede buscando (consulta la figura 5-2). Encontrarás la funcionalidad de búsqueda y filtrado en la **panel de Windows Server Essentials**, **acceso Web remoto**y la tienda Windows y Windows Phone **mi servidor** aplicaciones.

![Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c". Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.](media/larger-deployments-2.PNG)

Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c". Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.

> [!NOTE]  
> Mientras se ha aumentado el límite de usuarios y dispositivos compatible para el rol de servidor de Windows Server Essentials, el límite admitido para permanece de hacer copias de seguridad de cliente en 75.

<a name="see-also"></a>Consulta también
--------
[Introducción a Windows Server Essentials](get-started.md)