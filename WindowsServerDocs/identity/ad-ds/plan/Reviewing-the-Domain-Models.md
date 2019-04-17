---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisar los modelos de dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c57c23c0bd8694b66ab29b45bd9e47826ed1e01d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="reviewing-the-domain-models"></a>Revisar los modelos de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El modelo de diseño de dominio que selecciones afectan a los siguientes factores:  
  
-   Cantidad de capacidad disponible en la red que estás dispuesto a asignar a los servicios de dominio de Active Directory (AD DS). El objetivo es seleccionar un modelo que proporciona la replicación eficiente de información con un impacto mínimo en el ancho de banda de red disponible.  
  
-   Número de usuarios de la organización. Si su organización incluye un gran número de usuarios, implementar más de un dominio, podrás crear particiones de datos y te ofrece más control sobre la cantidad de tráfico de replicación que se pasa a través de una conexión de red determinada. Esto hace posible para que puedas controlar dónde se replican los datos y reducir la carga creada por el tráfico de replicación vínculos lento en la red.  
  
El diseño de dominio más simple es un solo dominio. En un diseño de dominio único, toda la información se replica en todos los controladores de dominio. Sin embargo, si es necesario, puedes implementar dominios regionales adicionales. Esto puede ocurrir si partes de la infraestructura de red están conectadas mediante vínculos lentos y desea que el propietario del bosque para asegurarte de que el tráfico de replicación no supera la capacidad que tiene asignada en AD DS.  
  
Es mejor minimizar la cantidad de dominios que hayas implementado en el bosque. Esto reduce la complejidad de la implementación global y, como resultado, reduce el costo total de propiedad. La siguiente tabla enumera los costos administrativos asociados con la adición de dominios regionales.  
  
|Costo|Implicaciones|  
|--------|----------------|  
|Administración de varios grupos de administrador de servicio|Cada dominio tiene sus propios grupos de administrador de servicio que necesitan administrarse de forma independiente. La pertenencia a estos grupos de administrador de servicio debe ser controlada cuidadosamente.|  
|Mantener la coherencia entre la configuración de directiva de grupo que son comunes a varios dominios|Configuración de directiva de grupo que se debe aplicar a todo el bosque debe aplicarse por separado para cada dominio en el bosque.|  
|Mantener la coherencia entre el control de acceso y auditoría opciones que son comunes a varios dominios|Control de acceso y auditoría opciones de configuración que se deben aplicar a través del bosque deben aplicarse por separado para cada dominio en el bosque.|  
|Aumenta las posibilidades de los objetos que se mueve entre dominios|Cuanto mayor sea el número de dominios, mayor será la probabilidad de que los usuarios, deberás mover de un dominio a otro. Este movimiento puede afectar potencialmente a los usuarios finales.|  
  
> [!NOTE]  
>  Contraseña muy específicas de Windows Server 2008 y las directivas de bloqueo de cuenta, también pueden afectar el modelo de diseño de dominio que selecciones. Antes de esta versión de Windows Server 2008, solo una cuenta y la contraseña directiva de bloqueo, que se especifica en la directiva de dominio predeterminada de dominio, puede aplicar a todos los usuarios del dominio. Como resultado, si quisieras contraseña diferente y la configuración de bloqueo de cuenta para diferentes grupos de usuarios, tenías que crear un filtro de contraseñas o implementar varios dominios. Ahora puedes usar directivas de contraseña específicas para especificar varias directivas de contraseña y para aplicar directivas de bloqueo de cuenta y de las restricciones de contraseñas diferentes para diferentes grupos de usuarios en un solo dominio. Para obtener más información sobre las directivas de bloqueo de cuenta y de contraseña muy específicas, consulta el Step-by-Step Guía de configuración de directiva de bloqueo de cuenta y contraseñas específicas ([https://go.microsoft.com/fwlink/?LinkID=91477](https://go.microsoft.com/fwlink/?LinkID=91477)).  
  
## <a name="single-domain-model"></a>Modelo de dominio único  
Un modelo de dominio único es menos costoso de mantener y más fácil de administrar. Consta de un bosque que contiene un solo dominio. Este es el dominio de raíz del bosque y contiene todas las cuentas de usuario y grupo en el bosque.  
  
Un modelo de bosque único dominio reduce la complejidad administrativa proporcionando las siguientes ventajas:  
  
-   Cualquier controlador de dominio puede autenticar a cualquier usuario en el bosque.  
  
-   Todos los controladores de dominio pueden ser catálogos globales, no es necesario planear para la colocación del servidor de catálogo global.  
  
En un bosque de dominio único, todos los datos de directorio se replica en todas las ubicaciones geográficas que hospedan los controladores de dominio. Aunque este modelo es más fácil de administrar, también se crea el tráfico de replicación la mayoría de los modelos de dominio. El directorio de la partición en varios dominios limita la replicación de objetos para regiones geográficas particulares pero da como resultado más sobrecarga administrativa.  
  
## <a name="regional-domain-model"></a>Modelo de dominio regional  
Todos los datos de objeto dentro de un dominio se replica en todos los controladores de dominio en ese dominio. Por este motivo, si el bosque incluye un gran número de usuarios que se distribuyen a través de diferentes ubicaciones geográficas conectadas mediante una red de área extensa (WAN), es posible que deberás implementar dominios regionales para reducir el tráfico de replicación sobre los vínculos WAN. En función de geográficamente dominios regionales pueden organizarse según la conectividad de red WAN.  
  
El modelo de dominio regional te permite mantener un entorno estable en el tiempo. Base de las regiones que se usa para definir los dominios en el modelo en elementos estables, como límites continentales. Dominios en función de otros factores, como grupos dentro de la organización, pueden cambiar con frecuencia y pueden requerir reestructurar su entorno.  
  
El modelo de dominio regional consta de un dominio raíz del bosque y uno o varios dominios regionales. Creación de un diseño de modelo de dominio regional implica identificar qué es el dominio de raíz del bosque y determinar el número de dominios adicionales que se deben cumplir los requisitos de replicación. Si su organización incluye grupos que requieran aislar el servicio de otros grupos de la organización o aislamiento de datos, crea un bosque independiente para estos grupos. No incluyas dominios aislar el servicio o aislamiento de datos.  
  


