---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Crear un diseño de vínculo de sitio
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 390cc0d69fa4d43a957500c0078d53dcdc69c10c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068777"
---
# <a name="creating-a-site-link-design"></a>Crear un diseño de vínculo de sitio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cree un diseño de vínculo de sitio para conectar sus sitios con vínculos a sitios. Los vínculos de sitio reflejan la conectividad entre sitios y el método que se usa para transferir el tráfico de replicación. Debe conectar sitios con vínculos a sitios para que los controladores de dominio de cada sitio puedan replicar Active Directory cambios.

## <a name="connecting-sites-with-site-links"></a>Conexión de sitios con vínculos a sitios

Para conectar sitios con vínculos a sitios, identifique los sitios miembro que desea conectar con el vínculo a sitios, cree un objeto de vínculo a sitios en el contenedor de transportes correspondiente Inter-Site y, a continuación, asigne un nombre al vínculo a sitios. Después de crear el vínculo a sitios, puede continuar para establecer las propiedades de vínculo a sitio.

Al crear vínculos a sitios, asegúrese de que todos los sitios se incluyen en un vínculo a sitios. Además, asegúrese de que todos los sitios estén conectados entre sí a través de otros vínculos a sitios para que los cambios se puedan replicar desde los controladores de dominio de cualquier sitio a todos los demás sitios. Si no lo hace, se genera un mensaje de error en el registro del servicio de directorio en Visor de eventos en el que se indica que la topología del sitio no está conectada.

Siempre que agregue sitios a un vínculo a sitios recién creado, determine si el sitio que se va a agregar es un miembro de otros vínculos a sitios y cambie la pertenencia a un vínculo a sitios del sitio si es necesario. Por ejemplo, si convierte un sitio en miembro del vínculo Default-First-Site-al crear el sitio inicialmente, asegúrese de quitar el sitio del vínculo Default-First-Site-después de agregar el sitio a un nuevo vínculo de sitio. Si no quita el sitio del vínculo predeterminado-primero-sitio, el comprobador de coherencia de la información (KCC) tomará decisiones de enrutamiento en función de la pertenencia de ambos vínculos a sitios, lo que puede dar lugar a un enrutamiento incorrecto.

Para identificar los sitios miembro que desea conectar con un vínculo a sitios, use la lista de ubicaciones y ubicaciones vinculadas que registró en la hoja de cálculo "ubicaciones geográficas y vínculos de comunicación" (DSSTOPO_1.doc). Si varios sitios tienen la misma conectividad y disponibilidad entre sí, puede conectarlos con el mismo vínculo a sitios.

El contenedor de Inter-Site transportes proporciona los medios para asignar vínculos de sitio al transporte que utiliza el vínculo. Cuando se crea un objeto de vínculo a sitios, se crea en el contenedor de IP, que asocia el vínculo a sitios con la llamada a procedimiento remoto (RPC) a través de un transporte IP o el contenedor de Protocolo simple de transferencia de correo (SMTP), que asocia el vínculo a sitios con el transporte SMTP.

> [!NOTE]
> La replicación SMTP no se admitirá en versiones futuras de Active Directory Domain Services (AD DS); por lo tanto, no se recomienda crear objetos de vínculos de sitio en el contenedor de SMTP.

Cuando se crea un objeto de vínculo a sitios en el contenedor de transportes respectivo Inter-Site, AD DS utiliza RPC sobre IP para transferir la replicación entre sitios y entre los controladores de dominio. Para mantener los datos seguros mientras están en tránsito, la replicación de RPC sobre IP usa tanto el protocolo de autenticación Kerberos como el cifrado de datos.

Cuando no hay disponible una conexión IP directa, puede configurar la replicación entre sitios para usar SMTP. Sin embargo, la funcionalidad de replicación de SMTP está limitada y requiere una entidad de certificación (CA) empresarial. SMTP solo puede replicar la configuración, el esquema y las particiones de directorio de aplicaciones, y no admite la replicación de particiones de directorio de dominio.

Para asignar nombres a los vínculos a sitios, use un esquema de nomenclatura coherente, como name_of_site1-name_of_site2. Grabe la lista de sitios, sitios vinculados y los nombres de los vínculos a sitios que conectan estos sitios en una hoja de cálculo. Para obtener una hoja de cálculo que le ayude a registrar nombres de sitio y nombres de vínculos de sitio asociados, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abrir "sitios y vínculos a sitios asociados" (DSSTOPO_5.doc).

## <a name="in-this-guide"></a>En esta guía

[Establecer las propiedades de vínculo de sitio](Setting-Site-Link-Properties.md)
