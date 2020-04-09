---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinar el número de bosques necesarios
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 237012b2a25f0c28beb3b0716287b4f6a554b625
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822558"
---
# <a name="determining-the-number-of-forests-required"></a>Determinar el número de bosques necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar el número de bosques que debe implementar, debe identificar y evaluar cuidadosamente los requisitos de aislamiento y autonomía de cada grupo de la organización y asignar esos requisitos a los modelos de diseño de bosque adecuados.  
  
A la hora de determinar el número de bosques para implementar en su organización, tenga en cuenta lo siguiente:  
  
-   Los requisitos de aislamiento limitan las opciones de diseño. Por lo tanto, si identifica los requisitos de aislamiento, asegúrese de que los grupos necesiten realmente aislamiento de datos y que la autonomía de los datos no sea suficiente para sus necesidades. Asegúrese de que los distintos grupos de su organización comprenden claramente los conceptos de aislamiento y autonomía.  
  
-   Negociar el diseño puede ser un proceso largo. Puede ser difícil para los grupos obtener un acuerdo sobre la propiedad y los usos de los recursos disponibles. Asegúrese de dejar tiempo suficiente para que los grupos de su organización lleven a cabo una investigación adecuada para identificar sus necesidades. Establezca plazos de firmes para las decisiones de diseño y obtenga consenso de todas las partes en las fechas límite establecidas.  
  
-   Determinar el número de bosques que se van a implementar implica el equilibrio de los costos en comparación con las ventajas. Un modelo de bosque único es la opción más rentable y requiere la menor cantidad de sobrecarga administrativa. Aunque un grupo de la organización podría preferir operaciones de servicio autónomas, podría ser más rentable para la organización suscribirse a la entrega de servicios desde un grupo de tecnologías de la información (TI) centralizado y de confianza. Esto permite que el grupo sea propietario de la administración de datos sin crear los costos agregados de la administración de servicios. El equilibrio entre los costos y los beneficios puede requerir la intervención del Patrocinador Ejecutivo.  
  
    Un solo bosque es la configuración más sencilla que se va a administrar. Permite la colaboración máxima en el entorno porque:  
  
    -   Todos los objetos de un único bosque se enumeran en el catálogo global. Por lo tanto, no se requiere ninguna sincronización entre bosques.  
  
    -   No es necesaria la administración de una infraestructura duplicada.  
  
-   Dos organizaciones de ti independientes y autónomas no recomiendan la copropiedad de un solo bosque. En el futuro, los objetivos de los dos grupos de TI pueden cambiar, por lo que ya no pueden aceptar el control compartido.  
  
-   No se recomienda la administración de servicios de outsourcing a más de un asociado externo. Las organizaciones multinacionales que tienen grupos en diferentes países o regiones pueden optar por externalizar la administración de servicios a otro asociado externo para cada país o región. Dado que varios asociados externos no se pueden aislar entre sí, las acciones de un asociado pueden afectar al servicio del otro, lo que dificulta la retención de los asociados a sus contratos de nivel de servicio.  
  
-   Solo debe existir una instancia de un dominio de Active Directory en cualquier momento. Microsoft no admite la clonación, la división o la copia de controladores de dominio de un dominio en un intento de establecer una segunda instancia del mismo dominio. Para obtener más información sobre esta limitación, consulte la sección siguiente.  
  
## <a name="restructuring-limitations"></a>Limitaciones de reestructuración  
Cuando una empresa adquiere otra empresa, unidad de negocio o línea de productos, es posible que la empresa de compras también desee adquirir los activos de ti correspondientes del vendedor. En concreto, es posible que el comprador desee adquirir algunos o todos los controladores de dominio que hospedan las cuentas de usuario, las cuentas de equipo y los grupos de seguridad correspondientes a los recursos de negocio que se van a adquirir. Los únicos métodos admitidos por el comprador para adquirir los recursos de ti que se almacenan en el bosque de Active Directory del vendedor son los siguientes:  
  
1.  Adquiera la única instancia del bosque, incluidos todos los controladores de dominio y los datos de directorio del bosque completo del vendedor.  
  
2.  Migre los datos de directorio necesarios del bosque o dominios del vendedor a uno o varios de los dominios del comprador. El destino de esta migración podría ser un bosque totalmente nuevo o uno o varios dominios existentes que ya están implementados en el bosque del comprador.  
  
Esta limitación de compatibilidad existe porque:  
  
-   A cada dominio de un bosque Active Directory se le asigna una identidad única durante la creación del bosque. La copia de controladores de dominio de un dominio original a un dominio clonado pone en peligro la seguridad de los dominios y del bosque. Entre las amenazas del dominio original y del dominio clonado se incluyen las siguientes:  
  
    -   Uso compartido de contraseñas que se pueden usar para obtener acceso a los recursos  
  
    -   Información sobre los grupos y las cuentas de usuario con privilegios  
  
    -   Asignación de direcciones IP a nombres de equipo  
  
    -   Adiciones, eliminaciones y modificaciones de la información de directorio si los controladores de dominio de un dominio clonado establecen la conectividad de red con los controladores de dominio del dominio original  
  
-   Los dominios clonados comparten una identidad de seguridad común. por lo tanto, no se pueden establecer relaciones de confianza entre ellos, incluso si se ha cambiado el nombre de uno de los dominios o de ambos.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Modelos de diseño de bosque](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Asignar los requisitos de diseño a los modelos de diseño de bosque](Forest-Design-Models.md)  
  
-   [Usar el modelo de bosque de dominio de la organización](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


