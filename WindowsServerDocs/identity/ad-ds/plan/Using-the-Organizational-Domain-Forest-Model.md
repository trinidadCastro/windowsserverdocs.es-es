---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Mediante el modelo de bosque de dominio organizativo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875916"
---
# <a name="using-the-organizational-domain-forest-model"></a>Mediante el modelo de bosque de dominio organizativo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En el modelo de bosque de dominio de la organización, varios grupos autónomos cada poseen un dominio dentro de un bosque. Cada grupo controla la administración de servicios de nivel de dominio, lo que les permite administrar determinados aspectos de administración de servicios de forma autónoma mientras que el propietario del bosque controla la administración de servicios de nivel de bosque.  

La siguiente ilustración muestra un modelo de bosque de dominio de la organización.  

![utilizando el modelo de bosque de dominio de organización](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomía del servicio de nivel de dominio

El modelo de bosque de dominio de la organización permite la delegación de autoridad de administración de servicios de nivel de dominio. En la tabla siguiente se enumera los tipos de administración del servicio que se puede controlar en el nivel de dominio.  

|Tipo de administración de servicios|Tareas asociadas|  
|------------------------------|--------------------|  
|Administración de las operaciones de controlador de dominio|-Crear y quitar controladores de dominio<br />-Supervisión el funcionamiento de los controladores de dominio<br />-Administración de servicios que se ejecutan en controladores de dominio<br />-Copia de seguridad y restaurar el directorio|  
|Configuración de todo el dominio|-Crear dominio y usuario de dominio directivas de cuenta, como la contraseña, Kerberos y directivas de bloqueo de cuenta<br />-Crear y aplicar la directiva de grupo de todo el dominio|  
|Delegación de administración a nivel de datos|-Crear unidades organizativas (OU) y delegar la administración<br />-Reparar problemas en la estructura de OU que los propietarios de la unidad organizativa no tiene suficientes derechos de acceso para corregir|  
|Administración de confianzas externas|-Establecer relaciones de confianza con dominios fuera del bosque|  

Otros tipos de administración de servicios, por ejemplo, esquema o administración de la topología de replicación, son responsabilidad del propietario del bosque.  

## <a name="domain-owner"></a>Propietario del dominio

En un modelo de bosque de dominio de la organización, los propietarios son responsables de las tareas de administración de servicio de nivel de dominio. Los propietarios del dominio tienen autoridad sobre todo el dominio, así como acceso a todos los demás dominios del bosque. Por este motivo, los propietarios de dominio deben ser las personas de confianza seleccionadas por el propietario del bosque.  

Delegar la administración de servicio de nivel de dominio en el propietario de un dominio, si se cumplen las condiciones siguientes:  

- Todos los grupos que participan en el bosque de confianza al nuevo propietario del dominio y las prácticas de administración de servicio del nuevo dominio.  

- El nuevo propietario del dominio confía en el propietario del bosque y todos los otros propietarios de dominio.  

- Todos los propietarios de dominios del bosque acepta que el nuevo propietario del dominio tenga administración del Administrador de servicios y las directivas de selección y prácticas que son iguales o más estricta que sus propios.  

- Todos los propietarios de dominios del bosque de acuerdo en que los controladores de dominio administrados por el propietario del dominio nuevo en el nuevo dominio son físicamente seguros.  

Tenga en cuenta que si una bosque propietario delega el servicio de nivel de dominio en administración de propietario de un dominio, otros grupos podrían decidir no unirse a ese bosque si no confía en ese propietario del dominio.  

Todos los propietarios del dominio deben ser conscientes de que si alguna de estas condiciones se cambia en el futuro, podría ser necesario mover los dominios de la organización a una implementación de varios bosques.  

> [!NOTE]  
> Otra manera de minimizar los riesgos de seguridad a un dominio de Active Directory de Windows Server 2008 es emplear la separación de roles de administrador, lo que requiere la implementación de un controlador de dominio de solo lectura (RODC) en su infraestructura de Active Directory. Un RODC es un nuevo tipo de controlador de dominio en el sistema operativo Windows Server 2008 que hospeda particiones de sólo lectura de la base de datos de Active Directory. Antes de la versión de Windows Server 2008, cualquier trabajo de mantenimiento del servidor en un controlador de dominio debía realizarse por un administrador de dominio. En Windows Server 2008, puede delegar permisos administrativos locales para un RODC para cualquier usuario del dominio sin conceder derechos administrativos para el dominio o en otros controladores de dominio de ese usuario. Esto permite que el usuario delegado para iniciar sesión en un RODC y realizar el trabajo de mantenimiento, como actualizar un controlador, en el servidor. Sin embargo, este usuario delegado no puede iniciar sesión en cualquier otro controlador de dominio o realizar cualquier otra tarea administrativa en el dominio. De esta manera, cualquier confianza puede ser el usuario delegado la capacidad para administrar eficazmente el RODC sin poner en peligro la seguridad del resto del dominio. Para obtener más información acerca de los RODC, vea [AD DS: Los controladores de dominio de solo lectura](https://go.microsoft.com/fwlink/?LinkId=106616).  
