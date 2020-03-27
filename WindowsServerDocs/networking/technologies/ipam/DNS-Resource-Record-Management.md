---
title: Administración de registros de recursos DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3aa20913a01a23291879b98d6f53fe60a7138670
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312450"
---
# <a name="dns-resource-record-management"></a>Administración de registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información acerca de la administración de registros de recursos DNS mediante IPAM.  
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas de administración de registros de recursos DNS en esta sección.  
>   
> -   [Agregar un registro de recursos DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Eliminar registros de recursos DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrar la vista de los registros de recursos DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Visualización de registros de recursos DNS para una dirección IP específica](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Introducción a la administración de registros de recursos  
Al implementar IPAM en Windows Server 2016, puede realizar la detección de servidores para agregar servidores DHCP y DNS a la consola de administración del servidor IPAM. A continuación, el servidor IPAM recopila dinámicamente los datos DNS cada seis horas desde los servidores DNS que está configurado para administrar. IPAM mantiene una base de datos local donde almacena estos datos DNS. IPAM proporciona una notificación del día y la hora en que se recopilaron los datos del servidor, así como para indicarle el siguiente día y hora en que se producirá la recopilación de datos de los servidores DNS.  
  
La barra de estado amarilla de la ilustración siguiente muestra la ubicación de la interfaz de usuario de las notificaciones de IPAM.  
  
![Notificación de IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Los datos DNS que se recopilan incluyen información de la zona DNS y del registro de recursos. Puede configurar IPAM para recopilar información de zona del servidor DNS preferido.  IPAM recopila las zonas de Active Directory y basadas en archivos.  
  
> [!NOTE]  
> IPAM recopila datos únicamente de los servidores DNS de Microsoft Unidos a un dominio. Los servidores DNS de terceros y los servidores no Unidos a un dominio no se admiten en IPAM.  
  
A continuación se muestra una lista de los tipos de registro de recursos DNS que se recopilan en IPAM.  
  
-   Base de datos AFS  
  
-   Dirección ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Host A o AAAA  
  
-   Información del host  
  
-   ISDN  
  
-   MX  
  
-   Servidores de nombres  
  
-   Puntero (PTR)  
  
-   Persona responsable  
  
-   Enrutar a través  
  
-   Ubicación del servicio  
  
-   SOA  
  
-   SRV  
  
-   Texto  
  
-   Servicios conocidos  
  
-   WINS  
  
-   WINS-R  
  
-   X. 25  
  
En Windows Server 2016, IPAM proporciona integración entre el inventario de direcciones IP, el Zonas DNS y los registros de recursos DNS:  
  
-   Puede usar IPAM para generar automáticamente un inventario de direcciones IP a partir de registros de recursos DNS.  
  
-   Puede crear manualmente un inventario de direcciones IP desde los registros de recursos DNS A y AAAA.  
  
-   Puede ver los registros de recursos DNS para una zona DNS específica y filtrar los registros en función del tipo, la dirección IP, los datos del registro de recursos y otras opciones de filtrado.  
  
-   IPAM crea automáticamente una asignación entre los intervalos de direcciones IP y las zonas de búsqueda inversa de DNS.  
  
-   IPAM crea direcciones IP para los registros PTR que se encuentran en la zona de búsqueda inversa y que se incluyen en ese intervalo de direcciones IP. También puede modificar manualmente esta asignación si es necesario.  
  
IPAM le permite realizar las siguientes operaciones en los registros de recursos desde la consola de IPAM.  
  
-   Crear registros de recursos DNS  
  
-   Editar registros de recursos DNS  
  
-   Eliminar registros de recursos DNS  
  
-   Crear registros de recursos asociados  
  
IPAM registra automáticamente todos los cambios de configuración de DNS que realice mediante la consola de IPAM.  
  
## <a name="see-also"></a>Consulta también  
[Administrar IPAM](Manage-IPAM.md)  
  


