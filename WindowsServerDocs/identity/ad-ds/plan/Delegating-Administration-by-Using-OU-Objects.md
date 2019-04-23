---
ms.assetid: d8e61aa4-8e4b-4097-83ca-70cf61366b75
title: Delegar la administración mediante objetos de unidad organizativa
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39c4278270e4ab4fba9ff1062d2aa043d203a74b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886946"
---
# <a name="delegating-administration-by-using-ou-objects"></a>Delegar la administración mediante objetos de unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar unidades organizativas (OU) para delegar la administración de objetos, como usuarios o equipos, en la unidad organizativa a un individuo designado o un grupo. Para delegar la administración mediante el uso de una unidad organizativa, coloque el individuo o grupo al que va a delegar derechos administrativos en un grupo, coloque el conjunto de objetos que estén controladas en una unidad organizativa y, a continuación, delegar tareas administrativas para la unidad organizativa a ese grupo.  
  
Los servicios de dominio de Active Directory (AD DS) le permite controlar las tareas administrativas que se pueden delegar en un nivel muy detallado. Por ejemplo, puede asignar un grupo para tener control total de todos los objetos en una unidad organizativa; asignar a otro grupo los derechos solo para crear, eliminar y administrar cuentas de usuario en la unidad organizativa; y, a continuación, asignar un tercer grupo el derecho solo para restablecer las contraseñas de cuentas de usuario. Puede hacer que estos permisos heredables para que se apliquen a todas las unidades organizativas que se colocan en los subárboles de la unidad organizativa original.  
  
De forma predeterminada las unidades organizativas y contenedores se crean durante la instalación de AD DS y se controlan los administradores de servicios. Es mejor si continúan los administradores de servicios controlar estos contenedores. Si desea delegar el control sobre los objetos en el directorio, crear otras unidades organizativas y colocar los objetos en estas unidades organizativas. Delegar el control a través de estas unidades organizativas a los administradores de datos adecuado. Esto permite delegar el control sobre los objetos en el directorio sin cambiar el control predeterminado que se asigna a los administradores de servicios.  
  
El propietario del bosque determina el nivel de entidad que se delega a un propietario de la unidad organizativa. Esto puede abarcar desde la capacidad de crear y manipular objetos en la unidad organizativa a sólo se les permite controlar un único atributo de un solo tipo de objeto en la unidad organizativa. Conceder a un usuario la capacidad para crear un objeto en la unidad organizativa implícitamente concede a ese usuario la capacidad de manipular cualquier atributo de cualquier objeto que crea el usuario. Además, si el objeto que se crea es un contenedor, el usuario tiene implícitamente la capacidad de crear y manipular los objetos que se colocan en el contenedor.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Delegar la administración de unidades organizativas y contenedores predeterminados](../../ad-ds/plan/Delegating-Administration-of-Default-Containers-and-OUs.md)  
  
-   [Delegar la administración de unidades organizativas de cuenta y las unidades organizativas de recursos](../../ad-ds/plan/Delegating-Administration-of-Account-OUs-and-Resource-OUs.md)  
  


