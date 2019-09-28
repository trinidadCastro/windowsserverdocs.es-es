---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Usar el modelo de bosque de dominio de la organización
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af5fd2da396ecc27db68d3be8d1c0eda82314d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402451"
---
# <a name="using-the-organizational-domain-forest-model"></a>Usar el modelo de bosque de dominio de la organización

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En el modelo de bosque de dominio de la organización, varios grupos autónomos poseen cada uno un dominio dentro de un bosque. Cada grupo controla la administración del servicio de nivel de dominio, lo que les permite administrar determinados aspectos de la administración de servicios de forma autónoma mientras el propietario del bosque controla la administración de servicios de nivel de bosque.  

En la ilustración siguiente se muestra un modelo de bosque de dominio organizativo.  

![usar el modelo de bosque de dominio de la organización](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomía de servicio de nivel de dominio

El modelo de bosque de dominio de la organización permite la delegación de autoridad para la administración de servicios en el nivel de dominio. En la tabla siguiente se enumeran los tipos de administración de servicios que se pueden controlar en el nivel de dominio.  

|Tipo de administración de servicios|Tareas asociadas|  
|------------------------------|--------------------|  
|Administración de operaciones de controlador de dominio|-Creación y eliminación de controladores de dominio<br />-Supervisar el funcionamiento de los controladores de dominio<br />-Administrar servicios que se ejecutan en controladores de dominio<br />-Realizar copias de seguridad y restaurar el directorio|  
|Configuración de la configuración de todo el dominio|-Creación de directivas de cuenta de usuario de dominio y dominio, como contraseñas, Kerberos y directivas de bloqueo de cuenta<br />-Crear y aplicar directiva de grupo de todo el dominio|  
|Delegación de la administración de nivel de datos|-Crear unidades organizativas (OU) y delegar la administración<br />-Reparación de problemas en la estructura de unidades organizativas que los propietarios de unidades organizativas no tienen derechos de acceso suficientes para solucionar|  
|Administración de confianzas externas|-Establecimiento de relaciones de confianza con dominios fuera del bosque|  

Otros tipos de administración de servicios, como el esquema o la administración de la topología de replicación, son responsabilidad del propietario del bosque.  

## <a name="domain-owner"></a>Propietario del dominio

En un modelo de bosque de dominio organizativo, los propietarios de dominio son responsables de las tareas de administración de servicios de nivel de dominio. Los propietarios de dominio tienen autoridad sobre todo el dominio, así como acceso a todos los demás dominios del bosque. Por esta razón, los propietarios de dominio deben ser usuarios de confianza seleccionados por el propietario del bosque.  

Delegue la administración de servicios de nivel de dominio en un propietario de dominio si se cumplen las condiciones siguientes:  

- Todos los grupos que participan en el bosque confían en el nuevo propietario de dominio y las prácticas de administración de servicios del nuevo dominio.  

- El nuevo propietario de dominio confía en el propietario del bosque y en todos los demás propietarios del dominio.  

- Todos los propietarios de dominios del bosque acuerdan que el nuevo propietario del dominio tiene directivas de selección y administración de administradores de servicios y prácticas que son iguales o más estrictas que las suyas propias.  

- Todos los propietarios del dominio del bosque acuerdan que los controladores de dominio administrados por el nuevo propietario del dominio en el nuevo dominio son físicamente seguros.  

Tenga en cuenta que si un propietario del bosque delega la administración de servicios de nivel de dominio en un propietario de dominio, es posible que otros grupos decidan no unirse a ese bosque si no confían en el propietario del dominio.  

Todos los propietarios de dominios deben tener en cuenta que, si alguna de estas condiciones cambia en el futuro, podría ser necesario trasladar los dominios de la organización a una implementación de varios bosques.  

> [!NOTE]  
> Otra manera de minimizar los riesgos de seguridad en un dominio de Active Directory de Windows Server 2008 es emplear la separación de roles de administrador, que requiere la implementación de un controlador de dominio de solo lectura (RODC) en la infraestructura de Active Directory. Un RODC es un nuevo tipo de controlador de dominio en el sistema operativo Windows Server 2008 que hospeda particiones de solo lectura de la base de datos de Active Directory. Antes del lanzamiento de Windows Server 2008, cualquier trabajo de mantenimiento del servidor en un controlador de dominio debía ser realizado por un administrador de dominio. En Windows Server 2008, puede delegar permisos administrativos locales para un RODC a cualquier usuario del dominio sin conceder a ese usuario ningún derecho administrativo para el dominio u otros controladores de dominio. Esto permite que el usuario delegado inicie sesión en un RODC y realice el trabajo de mantenimiento, como la actualización de un controlador, en el servidor. Sin embargo, este usuario delegado no puede iniciar sesión en ningún otro controlador de dominio ni realizar ninguna otra tarea administrativa en el dominio. De esta manera, se puede delegar a cualquier usuario de confianza la capacidad de administrar de forma eficaz el RODC sin poner en peligro la seguridad del resto del dominio. Para obtener más información acerca de los RODC, vea [AD DS: Controladores de dominio de solo lectura @ no__t-0.  
