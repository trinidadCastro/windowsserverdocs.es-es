---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrar AD DS en una infraestructura de DNS existente
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891046"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrar AD DS en una infraestructura de DNS existente

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si su organización ya tiene un servicio existente de servidor de sistema de nombres de dominio (DNS), el DNS para el propietario de los servicios de dominio de Active Directory (AD DS) debe trabajar con el propietario DNS para su organización para la integración de AD DS en la infraestructura existente. Esto implica la creación de un servidor DNS y la configuración del cliente DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Creación de una configuración de servidor DNS  
Cuando la integración de AD DS con un espacio de nombres DNS existente, se recomienda que haga lo siguiente:  
  
-   Instale el servicio servidor DNS en cada controlador de dominio del bosque. Esto proporciona tolerancia a errores si uno de los servidores DNS no está disponible. De este modo, los controladores de dominio no es necesario depender de otros servidores DNS para resolución de nombres. Esto también simplifica el entorno de administración porque todos los controladores de dominio tienen una configuración uniforme.  
  
-   Configurar el controlador de dominio de raíz de bosque de Active Directory para hospedar la zona DNS para el bosque de Active Directory.  
  
-   Configurar los controladores de dominio para cada dominio regional hospedar las zonas DNS que corresponden a sus dominios de Active Directory.  
  
-   Configurar la zona que contiene los registros de localizador de todo el bosque de Active Directory (es decir, la zona _msdcs. *forestname* zona) para replicar en todos los servidores DNS del bosque mediante el uso de la partición de directorio de aplicaciones DNS de todo el bosque.  
  
    > [!NOTE]  
    > Cuando el servicio servidor DNS se instala con (se recomienda esta opción) el Asistente de instalación de servicios de dominio de Active Directory, todas las tareas anteriores se realizan automáticamente. Para obtener más información, consulte [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > AD DS usa registros del ubicador de todo el bosque para permitir que los asociados de replicación para buscar entre sí y para permitir que los clientes encontrar servidores de catálogo global. AD DS almacena los registros del ubicador de todo el bosque en la zona _msdcs. *forestname* zona. Dado que la información de la zona debe estar ampliamente disponible, esta zona se replica en todos los servidores DNS del bosque mediante la partición de directorio de aplicaciones DNS de todo el bosque.  
  
La estructura DNS existente permanece intacta. No es necesario mover los servidores o zonas. Basta con crear una delegación para las zonas DNS integradas en Active Directory desde la jerarquía DNS existente.  
  
## <a name="creating-the-dns-client-configuration"></a>Creación de la configuración del cliente DNS  
Para configurar DNS en los equipos cliente, el DNS para el propietario de AD DS debe especificar el esquema y cómo los clientes localizar servidores DNS de nomenclatura del equipo. En la tabla siguiente se enumera nuestras configuraciones recomendadas para estos elementos de diseño.  
  
|Elemento de diseño|Configuración|  
|------------------|-----------------|  
|Nombre del equipo|Utilice la nomenclatura de forma predeterminada. Cuando un Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o equipo basado en Windows Vista se une a un dominio, el equipo asigna a sí mismo un nombre de dominio completo (FQDN) que incluye el nombre de host del equipo y el nombre de activo Dominio de Active.|  
|Configuración de la resolución de cliente|Configurar los equipos cliente para que señale a cualquier servidor DNS en la red.|  
  
> [!NOTE]  
> Clientes de Active Directory y los controladores de dominio pueden registrar dinámicamente sus nombres DNS incluso si no apuntan al servidor DNS que sea autoritativo para sus nombres.  
  
Un equipo podría tener otro nombre DNS existente si la organización, estáticamente había registrado previamente el equipo en DNS o si la organización ha implementado previamente una solución integrada de protocolo de configuración dinámica de Host (DHCP). Si los equipos cliente ya tienen un nombre DNS registrado, cuando se actualiza el dominio al que se unen a AD DS de Windows Server 2008, tienen nombres diferentes:  
  
-   El nombre DNS existente  
  
-   El nuevo nombre de dominio completo (FQDN)  
  
Todavía se pueden encontrar los clientes por cualquier nombre. Cualquier existente de DNS, DHCP o solución integrada de DNS y DHCP se deje intacto. Los nuevos nombres principales se crea automáticamente y se actualiza por medio de la actualización dinámica. Se limpian automáticamente mediante la eliminación de registros obsoletos.  
  
Si desea aprovechar las ventajas de la autenticación Kerberos al conectarse a un servidor que ejecuta Windows 2000, Windows Server 2003 o Windows Server 2008, debe asegurarse de que el cliente se conecta al servidor mediante el nombre principal.  
  


