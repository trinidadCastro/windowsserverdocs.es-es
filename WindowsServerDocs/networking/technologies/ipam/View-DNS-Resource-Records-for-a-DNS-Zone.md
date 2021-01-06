---
title: Visualización de registros de recursos DNS para una zona DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 63b05967e35ae878e5002d83ae2c0148f15c589b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948331"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Visualización de registros de recursos DNS para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para ver los registros de recursos DNS para una zona DNS en la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-view-dns-resource-records-for-a-zone"></a>Para ver los registros de recursos DNS de una zona

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.

3.  En el panel de navegación inferior, haga clic en **búsqueda directa** y, a continuación, expanda la lista dominio y zona para buscar y seleccionar la zona que desea ver. Por ejemplo, si tiene una zona denominada Dublín, haga clic en **Dublín**.

    ![Seleccione la zona que desea ver](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)


4.  En el panel de información, la vista predeterminada es de los servidores DNS de la zona. Para cambiar la vista, haga clic en **vista actual** y, a continuación, haga clic en **registros de recursos**.

    ![Cambiar la vista a los registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)

5.  Se muestran los registros de recursos DNS para la zona. Para filtrar los registros, escriba el texto que desea buscar en **filtro**.

    ![Escribir texto para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)

6.  Para filtrar los registros de recursos por tipo de registro, ámbito de acceso u otros criterios, haga clic en **Agregar criterios** y, a continuación, realice selecciones en la lista criterios y haga clic en **Agregar**.

    ![Usar criterios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)

## <a name="see-also"></a>Consulte también
Administración de zonas [DNS](DNS-Zone-Management.md) 
 [Administrar IPAM](Manage-IPAM.md)



