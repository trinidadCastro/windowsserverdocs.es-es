---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Creación de diseño de unidad organizativa
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830406"
---
# <a name="creating-an-organizational-unit-design"></a>Creación de diseño de unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los propietarios de bosque son responsables de crear diseños de la unidad organizativa (OU) para sus dominios. Creación de un diseño de unidad organizativa implica diseñar la estructura de unidad organizativa, asignar el rol de propietario de la unidad organizativa y crear cuenta y las unidades organizativas de recursos.  
  
Inicialmente, diseñar la estructura OU para habilitar la delegación de administración. Una vez completado el diseño de la unidad organizativa, puede crear estructuras de unidad organizativa adicionales para la aplicación de directiva de grupo para los usuarios y equipos y limitar la visibilidad de objetos. Para obtener más información, vea Diseñar una infraestructura de directiva de grupo ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Rol de propietario de la unidad organizativa  
El propietario del bosque designa un propietario de la unidad organizativa para cada unidad organizativa que haya diseñado para el dominio. Los propietarios de la unidad organizativa son administradores de datos que controlan un subárbol de objetos en Active Directory Domain Services (AD DS). Los propietarios de la unidad organizativa pueden controlar cómo se delega la administración y cómo se aplica la directiva a los objetos dentro de su unidad organizativa. También puede crear nuevos subárboles y delegar la administración de unidades organizativas dentro de los subárboles.  
  
Puesto que los propietarios de la unidad organizativa no posee o controlar el funcionamiento del servicio de directorio, puede separar la propiedad y administración del servicio de directorio de propiedad y administración de objetos, lo que reduce el número de administradores de servicios que tienen altos niveles de acceso.  
  
Las unidades organizativas proporcionan autonomía administrativa y los medios para controlar la visibilidad de objetos del directorio. Las unidades organizativas proporcionan aislamiento de otros administradores de datos, pero no proporcionan aislamiento de los administradores de servicios. Aunque los propietarios de la unidad organizativa tienen control sobre un subárbol de objetos, el propietario del bosque conserva el control total sobre todos los subárboles. Esto permite al propietario del bosque para corregir los errores, como un error en una lista de control de acceso (ACL) y para recuperar subárboles delegados cuando finalizan los administradores de datos.  
  
## <a name="account-ous-and-resource-ous"></a>Las unidades organizativas de cuenta y las unidades organizativas de recursos  
Las unidades organizativas de cuenta contienen objetos de usuario, grupo y equipo. Los propietarios del bosque deben crear una estructura de unidades Organizativas para administrar estos objetos y, a continuación, delegar el control de la estructura al propietario de la unidad organizativa. Si va a implementar un nuevo dominio de AD DS, crear una cuenta de la unidad organizativa del dominio para que se puede delegar el control de cuentas del dominio.  
  
Las unidades organizativas de recursos contienen recursos y las cuentas que son responsables de administrar esos recursos. El propietario del bosque también es responsable de crear una estructura de unidades Organizativas para administrar estos recursos y para delegar el control de esa estructura al propietario de la unidad organizativa. Crear unidades organizativas de recursos según sea necesario según los requisitos de cada grupo dentro de su organización para lograr la autonomía de la administración de datos y equipos.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentar el diseño de la unidad organizativa para cada dominio  
Ensamblar un equipo para diseñar la estructura de OU que usar para delegar el control sobre los recursos del bosque. El propietario del bosque puedan estar implicado en el proceso de diseño y debe aprobar el diseño de la unidad organizativa. También puede implicar al menos un administrador de servicio para asegurarse de que el diseño es válido. Otros participantes de equipo de diseño podrían incluir los administradores de datos que funcionarán en las unidades organizativas y la unidad organizativa, los propietarios que serán responsables de su administración.  
  
Es importante documentar el diseño de la unidad organizativa. Enumerar los nombres de las unidades organizativas que va a crear. Además, para cada unidad organizativa, el tipo de unidad organizativa, el propietario de la unidad organizativa, el elemento primario de unidad organizativa (si procede) y el origen de esa unidad organizativa de documento.  
  
Para que una hoja de cálculo que le ayudarán a documentar el diseño de la unidad organizativa, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo ayudas para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abra " Identificar las unidades organizativas para cada dominio"(DSSLOGI_9.doc).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisar los conceptos de diseño de unidad organizativa](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Delegar la administración mediante el uso de objetos de unidad organizativa](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


