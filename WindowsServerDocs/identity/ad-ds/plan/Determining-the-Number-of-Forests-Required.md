---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinar el número de bosques necesarios
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1721190bf592b6f7a1274d60d47bbc755eeff1c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820656"
---
# <a name="determining-the-number-of-forests-required"></a>Determinar el número de bosques necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar el número de bosques que se deben implementar, deberá identificar y evaluar los requisitos de aislamiento y la autonomía de cada grupo de su organización y asignar esos requisitos para los modelos de diseño de bosque adecuado con cuidado.  
  
Al determinar el número de bosques para implementar en su organización, tenga en cuenta lo siguiente:  
  
-   Los requisitos de aislamiento limitan las opciones de diseño. Por lo tanto, si identifica los requisitos de aislamiento, asegúrese de que los grupos realmente necesitan aislamiento de datos y que la autonomía de datos no es suficiente para sus necesidades. Asegúrese de que los distintos grupos de su organización comprenden claramente los conceptos de aislamiento y autonomía.  
  
-   Negociar el diseño puede ser un proceso largo. Puede ser difícil para los grupos llegar a un acuerdo sobre la propiedad y se utiliza para los recursos disponibles. Asegúrese de dar tiempo suficiente para los grupos de su organización para llevar a cabo una investigación adecuada para identificar sus necesidades. Establezca fechas límite firme de decisiones de diseño y obtenga un consenso de todas las partes en los plazos establecidos.  
  
-   Determinar el número de bosques para implementar implica costos y ventajas de equilibrio. Un modelo de bosque único es la opción más rentable y requiere la menor cantidad de sobrecarga administrativa. Aunque un grupo de la organización podría preferir las operaciones de servicio autónomos, podría ser más rentable para la organización para suscribirse a la prestación de servicios de un grupo de tecnología (TI) de la información centralizada y de confianza. Esto permite al grupo de administración de los propios datos sin necesidad de crear los costos adicionales de administración de servicios. Equilibrio de costos y beneficios puede requerir la entrada desde el patrocinador ejecutivo.  
  
    Un bosque único es la configuración más sencilla de administrar. Permite la colaboración máximo dentro del entorno porque:  
  
    -   Se muestran todos los objetos en un solo bosque en el catálogo global. Por lo tanto, se necesita ninguna sincronización entre bosques.  
  
    -   Administración de una infraestructura duplicada no es necesaria.  
  
-   No se recomienda por dos organizaciones de TI independientes y autónomas co-propiedad de un único bosque. En el futuro, pueden cambiar los objetivos de los dos grupos de TI, por lo que ya no puede aceptar el control compartido.  
  
-   No se recomienda administración de servicios de subcontratación a más de un fuera asociado. Las organizaciones multinacionales que tienen grupos en distintos países o regiones es posible que desee externalizar la administración del servicio a un asociado externo diferente para cada país o región. Dado que varios socios comerciales externos no pueden ser completamente aisladas entre sí, las acciones de un asociado pueden afectar al servicio de la otra, lo que dificulta mantener la responsabilidad a sus contratos de nivel de servicio de los asociados.  
  
-   Solo una instancia de un dominio de Active Directory debe existir en cualquier momento. Microsoft no admite la clonación, dividir o copiando los controladores de dominio de un dominio en un intento para establecer una segunda instancia del mismo dominio. Para obtener más información acerca de esta limitación, consulte la sección siguiente.  
  
## <a name="restructuring-limitations"></a>Limitaciones de reestructuración  
Si una compañía adquiere otra compañía, unidad de negocio, o línea de productos, la compañía de compra que le interese adquirir recursos de TI correspondientes del vendedor. En concreto, el comprador podría adquirir algunos o todos los controladores de dominio que hospedan las cuentas de usuario, las cuentas de equipo y grupos de seguridad que corresponden a los activos de negocio que se obtienen. Los únicos métodos admitidos para el comprador adquirir los activos de TI que se almacenan en el bosque de Active Directory del vendedor son los siguientes:  
  
1.  Adquirir la única instancia del bosque, incluidos todos los controladores de dominio y los datos de directorio en todo el bosque del vendedor.  
  
2.  Migrar los datos de directorio necesarios de dominios o bosque el vendedor a uno o varios de los dominios del comprador. El destino de este tipo de migración podría ser un bosque completamente nuevo o uno o varios dominios existentes que ya están implementados en el bosque del comprador.  
  
Esta limitación existe porque:  
  
-   Cada dominio en un bosque de Active Directory se asigna una identidad única durante la creación del bosque. Copiar los controladores de dominio desde un dominio original a un compromisos de dominio clonado la seguridad de los dominios y el bosque. Las amenazas en el dominio original y el dominio clonado incluyen lo siguiente:  
  
    -   Uso compartido de las contraseñas que pueden utilizarse para tener acceso a recursos  
  
    -   Información sobre grupos y cuentas de usuario con privilegios  
  
    -   Asignación de direcciones IP a los nombres de equipo  
  
    -   Las adiciones, eliminaciones y modificaciones de la información de directorio si los controladores de dominio en un dominio clonado nunca establecer conectividad de red con los controladores de dominio del dominio original  
  
-   Dominios clonados comparten una identidad de seguridad común; por lo tanto, no se puede establecer relaciones de confianza entre ellos, incluso si se han cambiado uno o ambos de los dominios.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Modelos de diseño de bosque](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Requisitos de diseño de la asignación a los modelos de diseño de bosque](Forest-Design-Models.md)  
  
-   [Mediante el modelo de bosque de dominio organizativo](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


