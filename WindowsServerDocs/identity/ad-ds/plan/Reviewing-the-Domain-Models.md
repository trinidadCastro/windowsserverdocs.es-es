---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisión de los modelos de dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 23c1eb66eeecab8df63cbd7910a9398bc4e3c705
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870926"
---
# <a name="reviewing-the-domain-models"></a>Revisión de los modelos de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes factores afectan el modelo de diseño de dominio que seleccione:  
  
- Cantidad de capacidad disponible en la red que está dispuesto a asignar a los servicios de dominio de Active Directory (AD DS). El objetivo es seleccionar un modelo que proporciona replicación eficaz de información con un impacto mínimo en el ancho de banda de red disponible.  

- Número de usuarios de su organización. Si su organización incluye un gran número de usuarios, implementar más de un dominio le permite particionar los datos y le ofrece más control sobre la cantidad de tráfico de replicación que se pasará a través de una conexión de red determinado. Esto permite controlar dónde se replican los datos y reducir la carga creada por el tráfico de replicación de vínculos lentos en la red.  

El diseño más simple de dominio es un único dominio. En un diseño único dominio, toda la información se replica en todos los controladores de dominio. Sin embargo, si es necesario, puede implementar dominios regionales adicionales. Esto puede ocurrir si las partes de la infraestructura de red están conectadas por vínculos lentos y desea que el propietario del bosque para asegurarse de que el tráfico de replicación no supera la capacidad que se ha asignado a AD DS.  

Es mejor minimizar el número de dominios que se implementan en el bosque. Esto reduce la complejidad general de la implementación y, en consecuencia, reduce el costo total de propiedad. En la tabla siguiente se enumera los costos administrativos asociados con la incorporación de dominios regionales.  

|Coste|Implicaciones|  
|--------|----------------|  
|Administración de varios grupos de administrador de servicio|Cada dominio tiene sus propios grupos de administrador de servicio que deben administrarse de forma independiente. La pertenencia de estos grupos de administrador de servicio debe controlarse cuidadosamente.|  
|Mantener la coherencia entre la configuración de directiva de grupo que son comunes a varios dominios|Configuración de directiva de grupo que debe aplicarse para todo el bosque debe aplicarse por separado para cada dominio del bosque.|  
|Mantener la coherencia entre el control de acceso y la configuración que son comunes a varios dominios de auditoría|Control de acceso y la configuración que debe aplicarse en todo el bosque de auditoría deben aplicarse por separado para cada dominio del bosque.|  
|Aumenta la probabilidad de objetos mover entre dominios|Cuanto mayor sea el número de dominios, mayor será la probabilidad de que los usuarios tendrán que mover de un dominio a otro. Este cambio puede afectar potencialmente a los usuarios finales.|  

> [!NOTE]  
> Directivas de bloqueo de cuenta y contraseña específica de Windows Server, también pueden afectar el modelo de diseño de dominio que seleccionó. Antes de esta versión de Windows Server 2008, sólo una contraseña y la cuenta de directiva de bloqueo, que se especifica en la directiva predeterminada de dominio del dominio, puede aplicar a todos los usuarios del dominio. Como resultado, si deseara otra contraseña y la configuración de bloqueo de cuenta para distintos conjuntos de usuarios, tenía que crear un filtro de contraseñas o implementar múltiples dominios. Ahora puede usar directivas de contraseña específica para especificar varias directivas de contraseña y para aplicar directivas de bloqueo de cuenta y las restricciones de contraseña diferente para distintos conjuntos de usuarios dentro de un único dominio. Para obtener más información sobre las directivas de bloqueo de cuenta y contraseña específica, consulte el artículo [guía paso a paso para la configuración de directiva de bloqueo de cuenta y contraseña específica](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modelo de dominio único

Un modelo de dominio único es el más fácil de administrar y menos costoso de mantener. Consta de un bosque que contiene un único dominio. Este dominio es el dominio raíz del bosque, y contiene todas las cuentas de usuario y de grupo en el bosque.  

Un modelo de bosque de dominio único reduce la complejidad administrativa, ya que proporciona las siguientes ventajas:  

- Cualquier controlador de dominio puede autenticar a cualquier usuario del bosque.  

- Todos los controladores de dominio pueden ser catálogos globales, por lo que no es necesario planear la ubicación del servidor de catálogo global.  
  
En un bosque de dominio único, todos los datos de directorio se replican en todas las ubicaciones geográficas que hospedan controladores de dominio. Aunque este modelo es el más fácil de administrar, también crea el tráfico de replicación de la mayoría de los modelos de dominio de dos. Creación de particiones del directorio en varios dominios limita la replicación de objetos en regiones geográficas específicas, pero los resultados más sobrecarga administrativa.  
  
## <a name="regional-domain-model"></a>Modelo de dominio regional

Todos los datos de objeto dentro de un dominio se replica en todos los controladores de dominio de ese dominio. Por este motivo, si su bosque incluye un gran número de usuarios que se distribuyen entre diferentes ubicaciones geográficas conectadas mediante una red de área extensa (WAN), es posible que deba implementar dominios regionales para reducir el tráfico de replicación a través de los vínculos WAN. Geográficamente basado en dominios regionales pueden organizarse según la conectividad de red WAN.  
  
El modelo de dominio regional permite mantener un entorno estable con el tiempo. Basar las regiones que se usan para definir los dominios en el modelo en elementos estables, como los límites del continentales. Los dominios según otros factores, como los grupos dentro de la organización, pueden cambiar con frecuencia y pueden requerir que reestructurar el entorno existente.  
  
El modelo de dominio regional consta de un dominio raíz del bosque y uno o varios dominios regionales. Crear un diseño de modelo de dominio regional implica la identificación de qué dominio es el dominio raíz del bosque y determinar el número de dominios adicionales que deben cumplir los requisitos de replicación. Si su organización incluye grupos que requieran el aislamiento de los datos o el aislamiento del servicio de otros grupos de la organización, cree un bosque independiente para estos grupos. Dominios no proporcionan aislamiento de los datos o el aislamiento del servicio.  
