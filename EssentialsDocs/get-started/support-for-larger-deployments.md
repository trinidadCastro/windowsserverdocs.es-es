---
title: Compatibilidad con implementaciones de gran tamaño
description: Obtenga información acerca de la compatibilidad con varios dominios, varios controladores de dominio, 500 usuarios y dispositivos 500 y la capacidad de especificar un controlador de dominio designado.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ec8cea711b679c41562b1e0580430cdb264ace0f
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696945"
---
# <a name="support-for-larger-deployments"></a>Compatibilidad con implementaciones de gran tamaño

>Se aplica a: Windows Server 2016 Essentials

> [!IMPORTANT]
> Las características descritas en este tema solo funcionan en Windows Server 2016 con el rol de experiencia de Essentials habilitado y no con la SKU de Windows Server 2016 Essentials.


Windows Server Essentials ahora admite implementaciones de mayor tamaño con:

- varios dominios
- varios controladores de dominio
- capacidad para especificar un controlador de dominio designado
- compatibilidad con hasta 500 usuarios y dispositivos 500

## <a name="support-for-multiple-domains"></a>Compatibilidad con varios dominios

Windows Server 2012 R2 Essentials solo admite un dominio por servidor, que es necesario, y el servidor de Essentials debe ser la raíz del bosque. Aunque todavía se requiere un dominio y un bosque, el rol experiencia con Windows Server 2016 Essentials ahora se puede implementar en Windows Server 2016 Standard o Datacenter para admitir varios dominios.

## <a name="support-for-multiple-domain-controllers"></a>Compatibilidad con varios controladores de dominio

 Windows Server Essentials 2012 R2 bloquea todos los servicios que aprovechan Azure Active Directory, como Microsoft 365, donde se implementa más de un controlador de dominio. La razón es que la sincronización de cuentas y contraseñas entre los controladores de dominio locales y Azure Active Directory puede conducir a las credenciales de cuenta con contraseñas que no están sincronizadas. Esta limitación se ha quitado en Windows Server 2016 Essentials.

## <a name="ability-to-specify-a-designated-domain-controller"></a>Capacidad para especificar un controlador de dominio designado

Ahora puede elegir un controlador de dominio designado para mejorar los tiempos de recuperación de Active Directory objetos de dominio, así como coordinar la sincronización de cambios de cuenta en otros controladores de dominio del dominio.

El controlador de dominio designado predeterminado será el mismo servidor que ejecuta el rol de servidor experiencia con Windows Server Essentials. Si ese servidor es un servidor miembro, lo que significa que no es un controlador de dominio, el controlador de dominio designado predeterminado se determinará automáticamente en función de la prueba de qué controlador de dominio del dominio tiene la latencia de red más baja para el servidor que ejecuta el rol de servidor de experiencia con Windows Server. Si desea cambiar manualmente el servidor que es el controlador de dominio designado, puede hacerlo en la **configuración** del panel de **Windows Server Essentials** , como se muestra a continuación.

![Captura de pantalla que muestra el panel de control de configuración en primer plano y en el panel de Windows Server Essentials en segundo plano. La página controlador de dominio designado del panel de control configuración está seleccionada actualmente.](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>Compatibilidad con 500 usuarios y dispositivos 500
-------------------------------------

El número máximo de usuarios y dispositivos admitidos en Windows Server 2012 R2 Essentials es 25 y 50, respectivamente. Con la introducción de la función de servidor de experiencia con Windows Server Essentials, se aumentó el límite a 100 usuarios y 200 dispositivos.

Windows Server 2016 Essentials admite 500 usuarios y dispositivos 500. Hacer esto posible es una actualización de los controles de marco de trabajo del proveedor y de la lista de objetos, de modo que almacenen en memoria caché y representen rápidamente listas de objetos de dispositivos y usuarios. Además, se ha agregado una característica de búsqueda y filtro para encontrar rápidamente el usuario o el dispositivo que puede buscar (vea la figura 5-2). Encontrará la funcionalidad de búsqueda y filtrado en el **Panel de Windows Server Essentials**, el **acceso Web remoto** y las aplicaciones de Windows y Windows Phone almacenar **My Server** Applications.

![Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c". Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.](media/larger-deployments-2.PNG)

Captura de pantalla que muestra el uso de la característica de búsqueda del panel de Windows Server Essentials para buscar la cadena "d5c". Los resultados de esta búsqueda incluyen dos archivos y carpetas y dos usuarios.

> [!NOTE]
> Aunque el límite de usuarios y dispositivos admitidos ha aumentado para el rol de servidor de Windows Server Essentials, el límite admitido para la copia de seguridad del cliente se mantiene en 75.

<a name="see-also"></a>Consulte también
--------
[Introducción a Windows Server Essentials](get-started.md)