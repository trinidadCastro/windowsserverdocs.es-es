---
description: 'Más información acerca de: identificación de los requisitos de diseño de bosque'
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificar los requisitos de diseño de bosque
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 5e1f5d6a5cee5f082a2c941912f141054887be50
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049673"
---
# <a name="identifying-forest-design-requirements"></a>Identificar los requisitos de diseño de bosque

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para crear un diseño de bosque para su organización, debe identificar los requisitos empresariales que la estructura de directorios necesita acomodar. Esto implica determinar cuánta autonomía necesitan los grupos de su organización para administrar sus recursos de red y si cada grupo necesita aislar los recursos de la red de otros grupos.

Active Directory Domain Services (AD DS) permite diseñar una infraestructura de directorios que incluye varios grupos de una organización que tienen requisitos de administración únicos y para lograr independencia estructural y operativa entre grupos según sea necesario.

Los grupos de su organización pueden tener algunos de los siguientes tipos de requisitos:

- **Requisitos de la estructura organizativa**. Las partes de una organización pueden participar en una infraestructura compartida para ahorrar costos, pero requieren la capacidad de operar de forma independiente del resto de la organización. Por ejemplo, un grupo de investigación dentro de una organización grande podría necesitar mantener el control sobre todos sus datos de investigación.

- **Requisitos operativos**. Una parte de una organización puede realizar restricciones únicas en la configuración del servicio de directorio, la disponibilidad o la seguridad, o bien usar aplicaciones que colocan restricciones únicas en el directorio. Por ejemplo, las unidades de negocio individuales dentro de una organización pueden implementar aplicaciones habilitadas para el directorio que modifican el esquema de directorio que no están implementadas por otras unidades de negocio. Dado que el esquema de directorio se comparte entre todos los dominios del bosque, la creación de varios bosques es una solución para este tipo de escenario. Otros ejemplos se encuentran en las siguientes organizaciones y escenarios:

    - Organizaciones militares

    - Escenarios de hospedaje

    - Organizaciones que mantienen un directorio disponible tanto interna como externamente (por ejemplo, los usuarios a los que se puede tener acceso públicamente en Internet)

- **Requisitos legales**. Algunas organizaciones tienen requisitos legales para operar de una manera determinada, por ejemplo, restringir el acceso a cierta información, tal como se especifica en un contrato de negocio. Algunas organizaciones tienen requisitos de seguridad para operar en redes internas aisladas. Si no se cumplen estos requisitos, se puede producir la pérdida del contrato y, posiblemente, de una acción legal.

Parte de la identificación de los requisitos de diseño de bosque implica la identificación del grado en que los grupos de su organización pueden confiar en los posibles propietarios de bosque y sus administradores de servicios, e identificar los requisitos de aislamiento y autonomía para cada grupo de la organización.

El equipo de diseño debe documentar los requisitos de aislamiento y autonomía del servicio y la administración de los datos para cada grupo de la organización que pretenda usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que puedan afectar a la implementación de AD DS.

El equipo de diseño debe documentar los requisitos de aislamiento y autonomía del servicio y la administración de los datos para cada grupo de la organización que pretenda usar AD DS. El equipo también debe tener en cuenta las áreas de conectividad limitada que puedan afectar a la implementación de AD DS. Para obtener una hoja de cálculo que le ayude a documentar las regiones que identificó, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) y abra "requisitos de diseño de bosque" (DSSLOGI_2.doc).

## <a name="in-this-section"></a>En esta sección

- [Ámbito de autoridad del administrador de servicios](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)

- [Autonomía frente a aislamiento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)
