---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Usar el modelo de bosque organizativa de dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 22d871d9157622375619dd90336e597d4bfb3d68
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="using-the-organizational-domain-forest-model"></a>Usar el modelo de bosque organizativa de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En el modelo de bosque organizativa de dominio, varios grupos autónomos cada propietario de un dominio dentro de un bosque. Cada grupo de controles de administración del servicio de nivel de dominio, lo que permite administrar ciertos aspectos de administración de servicios de forma autónoma mientras el propietario de bosque controla la administración de servicios de nivel de bosque.  
  
La siguiente ilustración muestra un modelo de bosque de dominio de organización.  
  
![con el modelo de bosque de dominio de organización](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  
  
## <a name="domain-level-service-autonomy"></a>Autonomía del servicio de nivel de dominio  
El modelo de bosque de dominio de organización permite la delegación de autoridad de administración de servicios de nivel de dominio. La siguiente tabla enumera los tipos de administración de servicios que pueden controlarse en el nivel de dominio.  
  
|Tipo de administración de servicios|Tareas asociadas|  
|------------------------------|--------------------|  
|Administración de las operaciones de controlador de dominio|-Crear y quitar controladores de dominio<br />-Supervisión el funcionamiento de los controladores de dominio<br />-Administrar servicios que se ejecutan en los controladores de dominio<br />-Copias de seguridad y restaurar el directorio|  
|Configuración de todo el dominio|-Crear dominio y usuario de dominio directivas de cuenta, como contraseñas, Kerberos y las directivas de bloqueo de cuenta<br />-Crear y aplicar la directiva de grupo de todo el dominio|  
|Delegación de administración de nivel de datos|-Crear unidades organizativas (OU) y delegar la administración<br />-Solucionar problemas en la estructura de unidad organizativa que no tienen derechos de acceso para corregir los propietarios de unidad organizativa|  
|Administración de confianzas externas|-Establecer relaciones de confianza con los dominios fuera del bosque|  
  
Otros tipos de administración de servicios, como el esquema o administración de la topología de replicación, son la responsabilidad del propietario del bosque.  
  
## <a name="domain-owner"></a>Propietario del dominio  
En un modelo de bosque organizativa de dominio, los propietarios de dominio son responsables de tareas de administración del servicio de nivel de dominio. Los propietarios de dominio tienen autoridad sobre el dominio completo, así como acceso a todos los otros dominios del bosque. Por este motivo, los propietarios de dominio deben ser individuos seleccionados por el propietario de bosque de confianza.  
  
Delegar la administración de servicios de dominio de nivel en el propietario de un dominio, si se cumplen las condiciones siguientes:  
  
-   Todos los grupos que participan en el bosque de confianza al propietario del dominio nuevo y las prácticas de administración de servicio del nuevo dominio.  
  
-   El propietario del dominio nuevo confía en el propietario de bosque y el otros de los propietarios de dominios.  
  
-   Todos los propietarios de dominio del bosque acepta que el propietario del dominio nuevo tiene administración del Administrador de servicios y las directivas de selección y procedimientos que son iguales o más estricto que sus propios.  
  
-   Todos los propietarios de dominio del bosque acepta que administrados por el propietario del dominio nuevo en el nuevo dominio los controladores de dominio son físicamente seguros.  
  
Ten en cuenta que si una bosque propietario delegados nivel de dominio administración de servicios al propietario de un dominio, es posible que elegir otros grupos no quieres unirte a ese bosque si no son de confianza que propietario del dominio.  
  
Todos los propietarios de dominio deben tener en cuenta que si alguna de estas condiciones cambiar en el futuro, podría ser necesario mover los dominios organizativos a una implementación de bosque de varios.  
  
> [!NOTE]  
> Otra forma de minimizar los riesgos de seguridad a un dominio de Active Directory de Windows Server 2008 es emplear separación de roles de administrador, que requiere la implementación de un controlador de dominio de solo lectura (RODC) en tu infraestructura de Active Directory. Un RODC es un nuevo tipo de controlador de dominio en el sistema operativo Windows Server 2008 que hospeda las particiones de solo lectura de la base de datos de Active Directory. Antes del lanzamiento de Windows Server 2008, los trabajos de mantenimiento de servidor en un controlador de dominio tenía que debe realizar un administrador de dominio. En Windows Server 2008, puede delegar permisos administrativos locales para un RODC a cualquier usuario de dominio sin conceder los derechos administrativos para el dominio o a otros controladores de dominio de ese usuario. Esto permite que el usuario delegado para iniciar sesión en un RODC y realizar tareas de mantenimiento, como actualizar un controlador, en el servidor. Sin embargo, este usuario delegada no se puede iniciar sesión en cualquier otro controlador de dominio o realizar cualquier otra tarea administrativa en el dominio. De este modo, cualquier de confianza puede estar delegar la capacidad para administrar eficazmente el RODC sin comprometer la seguridad del resto del dominio. Para obtener más información sobre los RODC, consulta el tema de AD DS: Read-Only controladores de dominio ([https://go.microsoft.com/fwlink/?LinkId=106616](https://go.microsoft.com/fwlink/?LinkId=106616)).  
  


