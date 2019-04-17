---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: "Crear un diseño de la unidad organizativa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>Crear un diseño de la unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los propietarios de bosque son responsables de crear diseños de la unidad organizativa (OU) para sus dominios. Crear un diseño de la unidad organizativa implica diseñar la estructura de unidad organizativa, asignar la función de propietario de la unidad organizativa y crear cuenta y unidades organizativas de recursos.  
  
Inicialmente, diseñar la estructura de unidad organizativa para habilitar la delegación de administración. Una vez completado el diseño de la unidad organizativa, puedes crear estructuras adicionales de unidad organizativa de la aplicación de la directiva de grupo para los usuarios y equipos y limitar la visibilidad de objetos. Para obtener más información, consulta el diseño de una infraestructura de directiva de grupo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Rol de propietario de la unidad organizativa  
El propietario del bosque designa un propietario de la unidad organizativa para cada unidad organizativa que hayas diseñado para el dominio. Los propietarios de unidad organizativa son administradores de datos que controlan un subárbol de objetos en los servicios de dominio de Active Directory (AD DS). Los propietarios de unidad organizativa pueden controlar cómo se delega la administración y cómo se aplica la directiva a los objetos de su unidad organizativa. También puedes crear nuevos subárboles y delegar la administración de unidades organizativas dentro de los subárboles.  
  
Dado que los propietarios de unidad organizativa no posees o controlar el funcionamiento del servicio de directorio, puedes separar posesión y administración del servicio de directorio de propiedad y administración de objetos, reducir el número de administradores de servicios que tienen altos niveles de acceso.  
  
Unidades organizativas proporcionan autonomía administrativa y los medios para controlar la visibilidad de los objetos en el directorio. Las unidades organizativas proporcionan aislamiento desde otros administradores de datos, pero no proporcionan el aislamiento de los administradores de servicio. Aunque los propietarios de unidad organizativa tienen control sobre un subárbol de objetos, el propietario de bosque conserva el control total sobre todos los subárboles. Esto permite que el propietario de bosque para corregir errores, como un error en una lista de control de acceso (ACL) y recuperar subárboles delegadas cuando finalizan los administradores de datos.  
  
## <a name="account-ous-and-resource-ous"></a>Unidades organizativas y unidades organizativas de recursos de la cuenta  
Unidades organizativas de la cuenta contienen los objetos de grupo, usuario y del equipo. Los propietarios del bosque deben crear una estructura de unidad organizativa para administrar estos objetos y, a continuación, delegar control de la estructura del propietario de la unidad organizativa. Si vas a implementar un nuevo dominio de AD DS, crea una cuenta de la unidad organizativa para el dominio de modo que puede delegar el control de las cuentas del dominio.  
  
Unidades organizativas de recursos contienen recursos y las cuentas que se encarga de administrar esos recursos. El propietario del bosque también es responsable de crear una estructura de unidad organizativa para administrar estos recursos y de delegación del control de dicha estructura al propietario de la unidad organizativa. Crear recurso de unidades organizativas según sea necesario en función de los requisitos de cada grupo dentro de la organización para autonomía en la administración de datos y equipos.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentación del diseño de la unidad organizativa para cada dominio  
Crear un equipo para diseñar la estructura de unidad organizativa que usas para delegar control sobre los recursos en el bosque. El propietario del bosque puede participar en el proceso de diseño y debe aprobar el diseño de la unidad organizativa. También puede implicar al menos un administrador de servicio para asegurarse de que el diseño es válido. Otros participantes del equipo de diseño pueden incluir los administradores de los datos que trabajan en las unidades organizativas y la unidad organizativa propietarios quién serán responsables de administrarlas.  
  
Es importante para el diseño de la unidad organizativa del documento. Enumera los nombres de las unidades organizativas que deseas crear. Y para cada unidad organizativa, el tipo de unidad organizativa, el propietario de la unidad organizativa, la unidad organizativa (si procede) principal y el origen de esa unidad organizativa del documento.  
  
Para que una hoja de cálculo que le ayudarán a documentar el diseño de la unidad organizativa, descarga Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisar los conceptos de diseño de la unidad organizativa](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Delegar la administración mediante el uso de objetos de la unidad organizativa](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


