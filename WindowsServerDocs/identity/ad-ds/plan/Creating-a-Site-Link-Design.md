---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Crear un diseño de vínculo de sitio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861936"
---
# <a name="creating-a-site-link-design"></a>Crear un diseño de vínculo de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear un diseño de vínculo de sitio para conectar los sitios con vínculos a sitios. Vínculos a sitios reflejan la conectividad entre sitios y el método utilizado para transferir el tráfico de replicación. Debe conectar los sitios con vínculos a sitios para que los controladores de dominio en cada sitio pueden replicar cambios de Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Conexión de sitios con vínculos a sitios

Para conectar sitios con vínculos a sitios, identifique los sitios miembro que desea conectar con el vínculo de sitio, cree un objeto de vínculo de sitio en el contenedor de transportes entre sitios respectivo y, a continuación, el nombre del vínculo a sitios. Después de crear el vínculo de sitio, podrá establecer propiedades de vínculo de sitio.  
  
Al crear vínculos a sitios, asegúrese de que todos los sitios se incluyen en un vínculo a sitios. Además, asegúrese de que todos los sitios están conectados entre sí mediante otros vínculos de sitio para que los cambios se pueden replicar desde controladores de dominio en cualquier sitio en todos los demás sitios. Si no hace esto, se genera un mensaje de error en el registro del servicio de directorio en el Visor de eventos que indica que la topología del sitio no está conectada.  
  
Cuando agrega sitios a un vínculo de sitio recién creado, determinar si el sitio que se va a agregar es un miembro de otros vínculos a sitios y cambiar la pertenencia de vínculo de sitio del sitio si es necesario. Por ejemplo, si realiza un sitio de un miembro de valor predeterminado-primer-sitio-Link al crear inicialmente el sitio, asegúrese de quitar el sitio predeterminado-primer-sitio-Link después de agregar el sitio a un nuevo vínculo a sitios. Si no quita el sitio predeterminado-primer-sitio-Link, el Comprobador de coherencia de la información (KCC) hará que las decisiones de enrutamiento basadas en la pertenencia de ambos vínculos de sitio, lo que puede provocar un enrutamiento incorrecto.  
  
Para identificar los sitios de miembro que se va a conectar con un vínculo a sitios, use la lista de ubicaciones y ubicaciones vinculadas que anotó en la hoja de cálculo "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc). Si varios sitios tienen la misma conectividad y disponibilidad entre sí, puede conectarlas con el mismo vínculo de sitio.  
  
El contenedor de transportes entre sitios proporciona los medios para la asignación de vínculos a sitios para el transporte que utiliza el vínculo. Cuando se crea un objeto de vínculo de sitio, crea en el contenedor IP, que asocia el vínculo de sitio con la llamada a procedimiento remoto (RPC) a través del transporte IP, o el contenedor de Protocolo Simple de transferencia de correo (SMTP), que asocia el vínculo a sitios SMTP transporte.  
  
> [!NOTE]  
> Replicación de SMTP no se admitirán en futuras versiones de los servicios de dominio de Active Directory (AD DS); por lo tanto, no se recomienda la creación de objetos de vínculos de sitio en el contenedor de SMTP.  
  
Cuando se crea un objeto de vínculo de sitio en el contenedor de transportes entre sitios respectivo, AD DS usa RPC sobre IP para transferir la replicación entre sitios y entre sitios entre los controladores de dominio. Para proteger datos en tránsito, RPC a través de la replicación IP usa tanto el Kerberos authentication protocol y cifrado de datos.  
  
Cuando una conexión directa de IP no está disponible, puede configurar la replicación entre sitios para usar SMTP. Sin embargo, la funcionalidad de replicación de SMTP está limitada y requiere una entidad de certificación (CA) empresarial. SMTP sólo puede replicar la configuración, esquema y las particiones de directorio de aplicación y no se admite la replicación de particiones de directorio de dominio.  
  
Nombre de vínculos a sitios, use un esquema de nomenclatura coherente, por ejemplo, name_of_site1 name_of_site2. Registro de la lista de sitios, sitios vinculados y los nombres de los vínculos a sitios conecta estos sitios en una hoja de cálculo. Para que una hoja de cálculo que le ayudarán a registrar los nombres de sitio y los nombres de vínculo de sitio asociado, vea [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, y Abra "Sitios y vínculos a sitios asociados" (DSSTOPO_5.doc).  
  
## <a name="in-this-guide"></a>En esta guía

[Establecer las propiedades de vínculo de sitio](Setting-Site-Link-Properties.md)  
