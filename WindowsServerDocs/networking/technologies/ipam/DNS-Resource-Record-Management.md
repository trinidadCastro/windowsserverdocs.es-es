---
title: Administración de registros de recursos DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2db802fa928a4051fbe409fc0ba60d774bb0428
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284050"
---
# <a name="dns-resource-record-management"></a>Administración de registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre cómo administrar registros de recursos DNS mediante IPAM.  
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas de administración de registros de recursos DNS en esta sección.  
>   
> -   [Agregar un registro de recursos DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Eliminar registros de recursos DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrar la vista de registros de recursos DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Visualización de registros de recursos DNS para una dirección IP específica](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Información general sobre la administración de registros de recursos  
Al implementar IPAM en Windows Server 2016, puede realizar la detección de servidores para agregar los servidores DHCP y DNS a la consola de administración de servidor IPAM. El servidor IPAM, a continuación, dinámicamente recopila datos DNS cada seis horas desde los servidores DNS que se configura para administrar. IPAM mantiene una base de datos local donde almacena estos datos DNS. IPAM proporciona notificaciones del día y hora en que se recopilaron los datos del servidor, así como que le indica el día siguiente y tiempo de cuándo se realizará la recopilación de datos de los servidores DNS.  
  
La barra de estado amarillo en la siguiente ilustración muestra la ubicación de la interfaz de usuario de notificaciones de IPAM.  
  
![Notificación de IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Los datos DNS que se recopilan incluyen zona y el recurso de información del registro DNS. Puede configurar IPAM para recopilar información de zona de servidor DNS preferido.  IPAM recopila las zonas basados en archivos y Active Directory.  
  
> [!NOTE]  
> IPAM recopila datos únicamente de los servidores DNS de Microsoft unido al dominio. No se admiten los servidores DNS de terceros y servidores unidos a dominios que no son por IPAM.  
  
Siguiente es una lista de tipos de registros de recursos DNS que se recopilan por IPAM.  
  
-   Base de datos AFS  
  
-   Dirección ATM  
  
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
  
-   Enrutar a través de  
  
-   Ubicación del servicio  
  
-   SOA  
  
-   SRV  
  
-   Text  
  
-   Servicios muy conocidos  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
En Windows Server 2016, IPAM proporciona integración entre el inventario de direcciones IP, las zonas DNS y registros de recursos DNS:  
  
-   Puede usar IPAM para generar automáticamente un inventario de direcciones IP de los registros de recursos DNS.  
  
-   Puede crear manualmente un inventario de direcciones IP de DNS A y registros de recursos AAAA.  
  
-   Puede ver los registros de recursos DNS para una zona DNS específica y filtrar los registros en función de tipo, dirección IP, los datos de registro de recursos y otras opciones de filtrado.  
  
-   IPAM crea automáticamente una asignación entre los intervalos de direcciones IP y zonas de búsqueda inversa de DNS.  
  
-   IPAM crea las direcciones IP de los registros PTR que están presentes en la zona de búsqueda inversa y que se incluyen en ese intervalo de direcciones IP. Esta asignación se puede modificar también manualmente si es necesario.  
  
IPAM permite realizar las siguientes operaciones en los registros de recursos desde la consola de IPAM.  
  
-   Crear registros de recursos DNS  
  
-   Editar registros de recursos DNS  
  
-   Eliminar registros de recursos DNS  
  
-   Crear registros de recursos asociados  
  
IPAM registra automáticamente todos los cambios de configuración de DNS, asegúrese de usar la consola de IPAM.  
  
## <a name="see-also"></a>Vea también  
[Administrar IPAM](Manage-IPAM.md)  
  


