---
title: Administración de registros de recursos DNS
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ed781ef37243b80ea9da8aad27a29046b8dc8c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dns-resource-record-management"></a>Administración de registros de recursos DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona información sobre cómo administrar los registros de recursos DNS mediante el uso de IPAM.  
  
> [!NOTE]  
> Además de este tema, los siguientes temas de administración de registros de recursos DNS están disponibles en esta sección.  
>   
> -   [Agregar un registro de recursos DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Eliminar registros de recursos DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtra la vista de recursos DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Ver registros de recursos DNS para obtener una dirección IP específica](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Información general de administración de registros de recursos  
Cuando implementas IPAM en Windows Server 2016, puedes realizar la detección de servidores para agregar servidores DHCP y DNS a la consola de administración del servidor IPAM. El servidor IPAM, a continuación, dinámicamente recopila datos DNS cada seis horas de los servidores DNS que está configurado para administrar. IPAM mantiene una base de datos local donde almacena estos datos DNS. IPAM le notifica el día y que se recopilan los datos del servidor, así como que indica el día siguiente hora de y cuándo se producirá la recopilación de datos de servidores DNS.  
  
La barra de estado amarillo en la siguiente ilustración muestra la ubicación de la interfaz de usuario de las notificaciones de IPAM.  
  
![Notificaciones de IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Los datos DNS que se recopilan incluyen recursos y la zona de información de registros DNS. Puedes configurar IPAM para recopilar información de la zona de tu servidor DNS preferido.  IPAM recopila las zonas basados en archivos y Active Directory.  
  
> [!NOTE]  
> IPAM recopila datos únicamente a partir de servidores unidos a un dominio de DNS de Microsoft. IPAM no admite otros fabricantes servidores DNS y no del dominio combinadas.  
  
Siguiente es una lista de tipos de registros de recursos DNS que recopilan IPAM.  
  
-   Base de datos AFS  
  
-   Dirección de CAJERO  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Host A o AAAA  
  
-   Información de host  
  
-   ISDN (RDSI)  
  
-   MX  
  
-   Servidores de nombres  
  
-   Puntero (PTR)  
  
-   Persona responsable  
  
-   A través de la ruta  
  
-   Ubicación del servicio  
  
-   INICIO DE AUTORIDAD  
  
-   SRV  
  
-   Texto  
  
-   Servicios conocidos  
  
-   GANA  
  
-   WINS-R  
  
-   X.25  
  
En Windows Server 2016, IPAM proporciona integración entre inventario de dirección IP, zonas DNS y registros de recursos DNS:  
  
-   Puedes usar IPAM para crear automáticamente un inventario de la dirección IP de los registros de recursos DNS.  
  
-   Puede crear manualmente un inventario de la dirección IP de DNS A y registros de recursos AAAA.  
  
-   Puedes ver los registros de recursos DNS para una zona DNS específica y filtrar los registros en función de tipo, dirección IP, datos de registro de recursos y otras opciones de filtrado.  
  
-   IPAM crea automáticamente una correlación entre los intervalos de direcciones IP y zonas de búsqueda inversa de DNS.  
  
-   IPAM crea direcciones IP los registros de puntero que están presentes en la zona de búsqueda inversa y que se incluyen en ese intervalo de direcciones IP. Puedes modificar esta asignación también manualmente si es necesario.  
  
IPAM te permite realizar las siguientes operaciones en registros de recursos de la consola IPAM.  
  
-   Crear los registros de recursos DNS  
  
-   Editar los registros de recursos DNS  
  
-   Eliminar registros de recursos DNS  
  
-   Crear registros de recursos asociados  
  
IPAM registra automáticamente todos los cambios de configuración de DNS, asegúrate de usar la consola de IPAM.  
  
## <a name="see-also"></a>Consulta también  
[Administrar IPAM](Manage-IPAM.md)  
  


