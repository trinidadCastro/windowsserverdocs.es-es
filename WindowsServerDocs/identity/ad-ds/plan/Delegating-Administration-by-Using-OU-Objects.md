---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delegar la administración mediante objetos de unidad organizativa
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af13896c07c10710be6e087be5d31dbd4aec698f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822718"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegar la administración mediante objetos de unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar unidades organizativas (OU) para delegar la administración de objetos, como usuarios o equipos, dentro de la unidad organizativa a un individuo o grupo designado. Para delegar la administración mediante el uso de una unidad organizativa, coloque la persona o el grupo al que está delegando los derechos administrativos en un grupo, coloque el conjunto de objetos que se van a controlar en una unidad organizativa y, a continuación, delegue las tareas administrativas de la unidad organizativa en ese grupo.  
  
Active Directory Domain Services (AD DS) permite controlar las tareas administrativas que se pueden delegar en un nivel muy detallado. Por ejemplo, puede asignar un grupo para tener el control total de todos los objetos de una unidad organizativa. asignar a otro grupo los derechos solo para crear, eliminar y administrar cuentas de usuario en la unidad organizativa; y, a continuación, asigne a un tercer grupo el derecho solo para restablecer las contraseñas de las cuentas de usuario. Puede hacer que estos permisos sean heredables para que se apliquen a las unidades organizativas que se colocan en los subárboles de la unidad organizativa original.  
  
Los contenedores y unidades organizativas predeterminados se crean durante la instalación de AD DS y se controlan mediante los administradores de servicios. Es mejor que los administradores de servicios sigan controlando estos contenedores. Si necesita delegar el control sobre objetos en el directorio, cree unidades organizativas adicionales y coloque los objetos en estas unidades organizativas. Delegue el control sobre estas unidades organizativas en los administradores de datos adecuados. Esto permite delegar el control sobre objetos en el directorio sin cambiar el control predeterminado dado a los administradores de servicios.  
  
El propietario del bosque determina el nivel de autoridad que se delega a un propietario de la unidad organizativa. Esto puede abarcar desde la capacidad de crear y manipular objetos dentro de la unidad organizativa para que solo se permita el control de un solo atributo de un tipo de objeto en la unidad organizativa. Al conceder a un usuario la capacidad de crear un objeto en la unidad organizativa, se concede implícitamente a ese usuario la capacidad de manipular cualquier atributo de cualquier objeto que el usuario crea. Además, si el objeto que se crea es un contenedor, el usuario tiene implícitamente la capacidad de crear y manipular cualquier objeto que se coloque en el contenedor.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Delegar la administración de unidades organizativas y contenedores predeterminados](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delegar la administración de unidades organizativas de cuentas y unidades organizativas de recursos](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


