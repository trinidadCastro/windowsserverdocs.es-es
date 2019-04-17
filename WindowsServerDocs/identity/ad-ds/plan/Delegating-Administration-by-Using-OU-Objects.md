---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: "Delegar la administración mediante el uso de objetos de la unidad organizativa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d0b4765304d8b302fc174c191af2c8e87a25304
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegar la administración mediante el uso de objetos de la unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes usar unidades organizativas (OU) para delegar la administración de objetos, como los usuarios o equipos, en la unidad organizativa a un individuo designado o grupo. Para delegar la administración mediante el uso de una unidad organizativa, coloca la persona o grupo al que se delegar derechos administrativos en un grupo, coloca el conjunto de objetos que se pueden controlar en una unidad organizativa y, a continuación, delegar tareas administrativas para la unidad organizativa a ese grupo.  
  
Los servicios de dominio de Active Directory (AD DS) te permite controlar las tareas administrativas que se pueden delegar en un nivel muy detallado. Por ejemplo, puedes asignar un grupo de tienen control total de todos los objetos en una unidad organizativa; asignar a otro grupo de los derechos solo para crear, eliminar y administrar cuentas de usuario en la unidad organizativa; y, a continuación, asigna un tercer grupo el derecho solo a restablecer las contraseñas de cuentas de usuario. Hacer que estos permisos heredables para que se aplican a las unidades organizativas que se colocan en subárboles de la unidad organizativa original.  
  
Unidades organizativas de forma predeterminada y los contenedores se crean durante la instalación de AD DS y son controlados por los administradores de servicio. Es mejor si siguen los administradores de servicio controlar estos contenedores. Si necesitas delegar control sobre los objetos en el directorio, crear otras unidades organizativas y coloca los objetos en estas unidades organizativas. Delegar el control de estas unidades organizativas a los administradores de datos apropiada. Esto hace posible delegar control sobre los objetos en el directorio sin cambiar el control predeterminado que se proporcionan a los administradores de servicio.  
  
El propietario de bosque determina el nivel de autorización que se delega a un propietario de la unidad organizativa. Esto puede oscilar entre la capacidad de crear y manipular objetos dentro de la unidad organizativa para que solo se permiten para controlar un único atributo de un solo tipo de objeto en la unidad organizativa. Conceder a un usuario la capacidad para crear un objeto en la unidad organizativa implícitamente concede a que el usuario la capacidad de manipular cualquier atributo de cualquier objeto que el usuario crea. Además, si el objeto que se crea es un contenedor, el usuario implícitamente tiene la capacidad de crear y manipular los objetos que se colocan en el contenedor.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Delegar la administración de unidades organizativas y contenedores predeterminados](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delegar la administración de unidades organizativas de la cuenta y unidades organizativas de recursos](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


