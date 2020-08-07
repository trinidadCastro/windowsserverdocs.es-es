---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisar los modelos de dominio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 326c9f5727fa529fc131fe7bac6589a30c54c30a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972262"
---
# <a name="reviewing-the-domain-models"></a>Revisar los modelos de dominio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes factores afectan al modelo de diseño de dominio que seleccione:

- Cantidad de capacidad disponible en la red que desea asignar a Active Directory Domain Services (AD DS). El objetivo es seleccionar un modelo que proporcione una replicación eficaz de la información con un impacto mínimo en el ancho de banda de red disponible.

- Número de usuarios de la organización. Si su organización incluye un gran número de usuarios, la implementación de más de un dominio le permite particionar los datos y proporciona más control sobre la cantidad de tráfico de replicación que pasará a través de una conexión de red determinada. Esto permite controlar dónde se replican los datos y reducir la carga creada por el tráfico de replicación en los vínculos de baja velocidad de la red.

El diseño de dominio más sencillo es un dominio único. En un diseño de dominio único, toda la información se replica en todos los controladores de dominio. Sin embargo, si es necesario, puede implementar dominios regionales adicionales. Esto puede ocurrir si partes de la infraestructura de red están conectadas mediante vínculos de baja velocidad y el propietario del bosque desea asegurarse de que el tráfico de replicación no supera la capacidad que se ha asignado a AD DS.

Es mejor minimizar el número de dominios que se implementan en el bosque. Esto reduce la complejidad general de la implementación y, como resultado, reduce el costo total de propiedad. En la tabla siguiente se enumeran los costos administrativos asociados con la adición de dominios regionales.

| Coste     | Implicaciones     |
| -------- | ---------------- |
| Administración de varios grupos de administradores de servicios|Cada dominio tiene sus propios grupos de administradores de servicios que deben administrarse de forma independiente. La pertenencia de estos grupos de administradores de servicios debe controlarse cuidadosamente.|
| Mantener la coherencia entre los valores de directiva de grupo que son comunes a varios dominios | Directiva de grupo Opciones de configuración que deben aplicarse en todo el bosque deben aplicarse por separado a cada dominio individual del bosque. |
| Mantener la coherencia entre el control de acceso y la configuración de auditoría que son comunes a varios dominios | La configuración de control de acceso y auditoría que debe aplicarse a través del bosque debe aplicarse por separado a cada dominio individual del bosque. |
| Mayor probabilidad de que los objetos se muevan entre dominios | Cuanto mayor sea el número de dominios, mayor será la probabilidad de que los usuarios deban pasar de un dominio a otro. Este movimiento puede afectar potencialmente a los usuarios finales. |

> [!NOTE]
> Las directivas de bloqueo de cuenta y contraseña específica de Windows Server también pueden afectar al modelo de diseño de dominio que seleccione. Antes de esta versión de Windows Server 2008, solo podía aplicar una directiva de bloqueo de cuenta y contraseña, que se especifica en la Directiva de dominio predeterminada de dominio, a todos los usuarios del dominio. Como resultado, si desea una configuración de bloqueo de cuenta y contraseña diferente para diferentes conjuntos de usuarios, tenía que crear un filtro de contraseña o implementar varios dominios. Ahora puede usar directivas de contraseña específica para especificar varias directivas de contraseñas y aplicar restricciones de contraseña y directivas de bloqueo de cuentas diferentes a distintos conjuntos de usuarios dentro de un solo dominio. Para obtener más información sobre las directivas de bloqueo de cuenta y contraseña específica, consulte el artículo [AD DS guía paso a paso de la Directiva de bloqueo de cuentas y contraseña](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770842(v=ws.10))específica.

## <a name="single-domain-model"></a>Modelo de dominio único

Un modelo de dominio único es el más fácil de administrar y el menos costoso de mantener. Consta de un bosque que contiene un solo dominio. Este dominio es el dominio raíz del bosque y contiene todas las cuentas de usuario y de grupo del bosque.

Un modelo de bosque de dominio único reduce la complejidad administrativa al proporcionar las siguientes ventajas:

- Cualquier controlador de dominio puede autenticar a cualquier usuario en el bosque.

- Todos los controladores de dominio pueden ser catálogos globales, por lo que no es necesario planear la ubicación del servidor de catálogo global.

En un bosque de dominio único, todos los datos de directorio se replican en todas las ubicaciones geográficas que hospedan controladores de dominio. Aunque este modelo es el más fácil de administrar, también crea el mayor tráfico de replicación de los dos modelos de dominio. La creación de particiones del directorio en varios dominios limita la replicación de objetos en regiones geográficas específicas, pero produce una mayor sobrecarga administrativa.

## <a name="regional-domain-model"></a>Modelo de dominio regional

Todos los datos de objetos de un dominio se replican en todos los controladores de dominio de ese dominio. Por esta razón, si el bosque incluye un gran número de usuarios que se distribuyen en diferentes ubicaciones geográficas conectadas mediante una red de área extensa (WAN), es posible que tenga que implementar dominios regionales para reducir el tráfico de replicación a través de los vínculos WAN. Los dominios regionales basados en geográficamente se pueden organizar según la conectividad WAN de red.

El modelo de dominio regional le permite mantener un entorno estable a lo largo del tiempo. Base las regiones utilizadas para definir los dominios del modelo en elementos estables, como los límites continental. Los dominios basados en otros factores, como los grupos de la organización, pueden cambiar con frecuencia y pueden requerir que se reestructure el entorno.

El modelo de dominio regional se compone de un dominio raíz del bosque y uno o varios dominios regionales. La creación de un diseño de modelo de dominio regional implica la identificación del dominio raíz del bosque y la determinación del número de dominios adicionales necesarios para satisfacer los requisitos de replicación. Si su organización incluye grupos que requieren aislamiento de datos o aislamiento de servicio de otros grupos de la organización, cree un bosque independiente para estos grupos. Los dominios no proporcionan aislamiento de datos ni aislamiento de servicio.
