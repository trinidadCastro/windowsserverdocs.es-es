---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: "Integración de AD DS en una infraestructura DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integración de AD DS en una infraestructura DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si la organización ya tiene un servicio de servidor de sistema de nombres de dominio (DNS) existente, el DNS de propietario de los servicios de dominio de Active Directory (AD DS) debe trabajar con el propietario DNS para la organización a integrar AD DS en la infraestructura existente. Esto implica crear un servidor DNS y la configuración del cliente DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Crear una configuración del servidor DNS  
Al integrar AD DS con un espacio de nombres DNS, te recomendamos que hagas lo siguiente:  
  
-   Instalar el servicio de servidor DNS en cada controlador de dominio del bosque. Esto proporciona tolerancia a errores si uno de los servidores DNS no está disponible. De este modo, los controladores de dominio no es necesario depender de otros servidores DNS para resolver el nombre. Esto también simplifica el entorno de administración porque todos los controladores de dominio tienen una configuración uniforme.  
  
-   Configurar el controlador de dominio de Active Directory bosque raíz para hospedar la zona DNS para el bosque de Active Directory.  
  
-   Configurar los controladores de dominio para cada dominio regional hospedar las zonas DNS que corresponden a sus dominios de Active Directory.  
  
-   Configurar la zona que contiene los registros de ubicador de todo el bosque de Active Directory (es decir, la _msdcs.* nombreBosque* zona) para replicar en todos los servidores DNS en el bosque mediante el uso de la partición de directorio de aplicación de todo el bosque DNS.  
  
    > [!NOTE]  
    > Cuando el servicio de servidor DNS se instala con el dominio servicios instalación Asistente para Active Directory (se recomienda esta opción), se realizan automáticamente todas las tareas anteriores. Para obtener más información, consulta [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > AD DS usa registros de todo el bosque ubicador para habilitar duplicadores para buscar entre sí y para permitir que los clientes buscar servidores de catálogo global. AD DS almacena los registros de todo el bosque localizador en _msdcs. *nombreBosque* zona. Dado que la información de la zona debe estar ampliamente disponible, esta zona se replica en todos los servidores DNS en el bosque mediante la partición de directorio de aplicación de todo el bosque DNS.  
  
La estructura de DNS permanece intacta. No es necesario mover los servidores o zonas. Simplemente debes crear una delegación a las zonas DNS integradas en Active Directory de la jerarquía DNS existente.  
  
## <a name="creating-the-dns-client-configuration"></a>Crear la configuración del cliente DNS  
Para configurar DNS en los equipos cliente, el DNS de propietario de AD DS debe especificar el equipo de nombres de esquema y cómo los clientes localizan los servidores DNS. La siguiente tabla enumeran nuestras configuraciones recomendadas para estos elementos de diseño.  
  
|Elemento de diseño|Configuración|  
|------------------|-----------------|  
|Nombres de equipo|Usa el nombre predeterminado. Cuando un Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o un equipo basado en Windows Vista, se une a un dominio, el equipo sí asigna un nombre de dominio completo (FQDN) que incluye el nombre de host del equipo y el nombre de dominio de Active Directory.|  
|Configuración de resolución del cliente|Configurar equipos cliente para apuntar a los servidores DNS en la red.|  
  
> [!NOTE]  
> Clientes de Active Directory y controladores de dominio pueden registrar dinámicamente sus nombres DNS incluso si no apuntan al servidor DNS que está autorizado para sus nombres.  
  
Un equipo puede tener otro nombre DNS existente si la organización, estáticamente registrado anteriormente el equipo en DNS o si la organización ha implementado una solución de protocolo de configuración dinámica de Host (DHCP) integrada anteriormente. Si los equipos cliente ya tienen un nombre DNS registrado, cuando se actualiza el dominio al que están unidos a AD DS de Windows Server 2008, tienen dos nombres diferentes:  
  
-   El nombre DNS existente  
  
-   El nuevo nombre de dominio completo (FQDN)  
  
Los clientes todavía se encuentra por nombre. Cualquier existentes de DNS, DHCP o solución DNS y DHCP integrada queda intacta. Los nuevos nombres principales se crea automáticamente y se actualiza mediante la actualización dinámica. Se borran automáticamente por medio de borrado.  
  
Si quieres sacar provecho de la autenticación Kerberos al conectarse a un servidor que ejecute Windows 2000, Windows Server 2003 o Windows Server 2008, debes asegurarte de que el cliente se conecta al servidor con el nombre principal.  
  


