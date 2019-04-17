---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: "Determinar el número de bosques necesarios"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a461dbb2b5bf9d2ca1bb6a336cb11ace775fb1cd
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-number-of-forests-required"></a>Determinar el número de bosques necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar el número de bosques que debes implementar, deberás identificar y evaluar los requisitos de autonomía y aislamiento de cada grupo de la organización y estos requisitos se asignan a los modelos de diseño de bosque correspondiente cuidadosamente.  
  
Al determinar el número de bosques para implementar para tu organización, ten en cuenta lo siguiente:  
  
-   Requisitos de aislamiento limitan la elección del diseño. Por lo tanto, si se identifican los requisitos de aislamiento, asegúrate de que los grupos realmente necesitan aislamiento de datos y que autonomía de datos no es suficiente para sus necesidades. Asegúrate de que los distintos grupos de la organización claramente comprenden los conceptos de autonomía y aislamiento.  
  
-   Negociar el diseño puede ser un proceso largo. Puede ser difícil de grupos para llegar a un acuerdo acerca de la propiedad y se usa para los recursos disponibles. Asegúrese de otorgar el tiempo necesario para los grupos de la organización para realizar una investigación adecuada para identificar sus necesidades. Establecer firme plazos de las decisiones de diseño y obtener consenso de todas las partes de los plazos establecidos.  
  
-   Determinar el número de bosques para implementar conlleva un equilibrio entre costos frente a ventajas. Un modelo de bosque único es la opción más rentable y requiere la menor cantidad de sobrecarga administrativa. Aunque un grupo en la organización puede preferir operaciones de servicio autónomo, podría ser más rentable para la organización que suscribirte a la implementación del servicio de un grupo de tecnología de información centralizada y de confianza. Esto permite que al grupo administración propios datos sin tener que crear los costos de administración de servicios adicionales. Equilibrio de costos frente a ventajas puede requerir la entrada desde el patrocinador ejecutivo.  
  
    Un bosque único es la configuración más sencilla para administrar. Permite la colaboración máximo dentro del entorno porque:  
  
    -   Todos los objetos en un bosque único se enumeran en el catálogo global. Por lo tanto, se requiere ninguna sincronización entre bosques.  
  
    -   Administración de una infraestructura duplicada no es necesaria.  
  
-   No recomendamos co-propiedad de un bosque único por dos independientes las organizaciones de TI. En el futuro, pueden cambiar los objetivos de los dos grupos de TI, por lo que ya no se puedan aceptar control compartido.  
  
-   No recomendamos administración del servicio subcontratación a más de una fuera de partners. Multinacionales que tienen los grupos en diferentes países o regiones puede delegar la administración del servicio a otro partner externo para cada país o región. Como varios partners externos pueden estar aislados entre sí, las acciones de un socio pueden afectar el servicio del otro, lo que hace que sea difícil mantener la responsabilidad para sus acuerdos de nivel de servicio de los socios.  
  
-   Solo una instancia de un dominio de Active Directory debe existir en cualquier momento. Microsoft no admite la clonación, división o copia los controladores de dominio de un dominio en un intento de establecer una segunda instancia del mismo dominio. Para obtener más información acerca de esta limitación, consulta la sección siguiente.  
  
## <a name="restructuring-limitations"></a>Limitaciones reestructuración  
Cuando una empresa adquiere otra empresa, empresa, o línea de productos, la empresa también puede adquirir los activos de TI correspondientes al vendedor. Específicamente, el comprador posible que quieras adquirir algunos o todos los controladores de dominio que las cuentas de usuario, cuentas de equipo y los grupos de seguridad que corresponden a los activos de negocio que se obtienen de host. Los métodos admitidos solo para el comprador adquirir los activos de TI que se almacenan en el bosque de Active Directory del vendedor son los siguientes:  
  
1.  Adquirir la única instancia del bosque, incluidos todos los controladores de dominio y los datos del directorio en todo el bosque del vendedor.  
  
2.  Migrar los datos de directorio necesarios de bosque del vendedor o dominios a uno o más de dominios del comprador. El objetivo de este tipo de migración puede ser un bosque de nuevo por completo o uno o varios dominios existentes que ya se implementan en el bosque del comprador.  
  
Esta limitación existe porque:  
  
-   Cada dominio en un bosque de Active Directory se le asigna una identidad única durante la creación del bosque. Copiar los controladores de dominio de un dominio original a pone en peligro un dominio clonados la seguridad de los dominios y del bosque. Amenazas para el dominio original y el dominio clonado incluyen lo siguiente:  
  
    -   Uso compartido de contraseñas que pueden usarse para tener acceso a recursos  
  
    -   Información sobre cuentas de usuario con privilegios y grupos  
  
    -   Asignación de direcciones IP a nombres de equipo  
  
    -   Adiciones, eliminaciones y modificaciones de información del directorio si los controladores de dominio de un dominio clonado nunca establecen conectividad de red con controladores de dominio del dominio original  
  
-   Dominios clonados comparten una identidad de seguridad comunes; por lo tanto, no se puede establecer relaciones de confianza entre ellas, incluso si se cambiaron uno o ambos de los dominios.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Modelos de diseño del bosque](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Requisitos de diseño de la asignación a los modelos de diseño del bosque](Forest-Design-Models.md)  
  
-   [Usar el modelo de bosque organizativa de dominio](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


