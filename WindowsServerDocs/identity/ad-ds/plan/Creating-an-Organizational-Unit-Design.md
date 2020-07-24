---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Creación de diseño de unidad organizativa
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 186ad8d63a1d30ce56b1f6a2780893cfb744463d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962257"
---
# <a name="creating-an-organizational-unit-design"></a>Creación de diseño de unidad organizativa

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los propietarios de bosques son responsables de crear diseños de unidades organizativas (OU) para sus dominios. La creación de un diseño de Uo implica diseñar la estructura de la unidad organizativa, asignar el rol de propietario de la unidad organizativa y crear unidades organizativas de cuentas y recursos.

Inicialmente, diseñe la estructura de la unidad organizativa para habilitar la delegación de la administración. Una vez completado el diseño de la unidad organizativa, puede crear estructuras de unidades organizativas adicionales para la aplicación de directiva de grupo a los usuarios y equipos, así como para limitar la visibilidad de los objetos. Para obtener más información, vea [diseñar una infraestructura de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10)).

## <a name="ou-owner-role"></a>Rol de propietario de Uo
El propietario del bosque designa a un propietario de la unidad organizativa que diseña para el dominio. Los propietarios de las unidades organizativas son administradores de datos que controlan un subárbol de objetos en Active Directory Domain Services (AD DS). Los propietarios de las unidades organizativas pueden controlar cómo se delega la administración y cómo se aplica la Directiva a los objetos de la unidad organizativa. También pueden crear nuevos subárboles y delegar la administración de las unidades organizativas dentro de esos subárboles.

Dado que los propietarios de las unidades organizativas no poseen ni controlan el funcionamiento del servicio de directorio, puede separar la propiedad y la administración del servicio de directorio de la propiedad y la administración de objetos, lo que reduce el número de administradores de servicios que tienen altos niveles de acceso.

Las unidades organizativas proporcionan autonomía administrativa y los medios para controlar la visibilidad de los objetos en el directorio. Las unidades organizativas proporcionan aislamiento de otros administradores de datos, pero no proporcionan aislamiento de los administradores de servicios. Aunque los propietarios de las unidades organizativas tienen control sobre un subárbol de objetos, el propietario del bosque conserva el control total sobre todos los subárboles. Esto permite que el propietario del bosque corrija los errores, como un error en una lista de control de acceso (ACL), y para reclamar los subárboles delegados cuando finalicen los administradores de datos.

## <a name="account-ous-and-resource-ous"></a>Unidades organizativas de cuentas y unidades organizativas de recursos
Las unidades organizativas de cuentas contienen objetos de usuario, grupo y equipo. Los propietarios de bosques deben crear una estructura de unidad organizativa para administrar estos objetos y delegar el control de la estructura en el propietario de la unidad organizativa. Si va a implementar un nuevo dominio de AD DS, cree una unidad organizativa de cuenta para el dominio de modo que pueda delegar el control de las cuentas en el dominio.

Las unidades organizativas de recursos contienen recursos y las cuentas que son responsables de administrar esos recursos. El propietario del bosque también es responsable de crear una estructura de unidad organizativa para administrar estos recursos y para delegar el control de esa estructura en el propietario de la unidad organizativa. Cree unidades organizativas de recursos según sea necesario en función de los requisitos de cada grupo dentro de su organización para obtener autonomía en la administración de datos y equipos.

## <a name="documenting-the-ou-design-for-each-domain"></a>Documentar el diseño de la unidad organizativa para cada dominio
Ensamble un equipo para diseñar la estructura de la unidad organizativa que se usa para delegar el control sobre los recursos en el bosque. El propietario del bosque podría estar implicado en el proceso de diseño y debe aprobar el diseño de la unidad organizativa. También puede incluir al menos un administrador de servicios para asegurarse de que el diseño es válido. Otros participantes del equipo de diseño podrían incluir los administradores de datos que trabajarán en las unidades organizativas y los propietarios de las unidades organizativas que serán responsables de administrarlos.

Es importante documentar el diseño de la unidad organizativa. Enumere los nombres de las unidades organizativas que va a crear. Y, para cada unidad organizativa, documente el tipo de unidad organizativa, el propietario de la unidad organizativa, la unidad organizativa principal (si es aplicable) y el origen de esa unidad organizativa.

Para obtener una hoja de cálculo que le ayude a documentar el diseño de la unidad organizativa, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) y abra "identificación de las unidades organizativas para cada dominio" (DSSLOGI_9.doc).

## <a name="in-this-section"></a>En esta sección

- [Revisar los conceptos de diseño de unidad organizativa](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [Delegar la administración mediante objetos de unidad organizativa](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
