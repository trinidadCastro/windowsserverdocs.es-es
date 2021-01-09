---
title: Adición de un registro de recursos DNS
description: Obtenga información acerca de cómo agregar uno o más registros de recursos DNS nuevos mediante la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5ced255a8cc15be6c0d8d6b6ec04f5031816eed5
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040495"
---
# <a name="add-a-dns-resource-record"></a>Adición de un registro de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para agregar uno o más registros de recursos DNS nuevos mediante la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-add-a-dns-resource-record"></a>Para agregar un registro de recursos DNS

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.

3.  En el panel de navegación inferior, haga clic en **búsqueda directa**. Todas las zonas de búsqueda directa de DNS administradas por IPAM se muestran en los resultados de la búsqueda del panel de información. Haga clic con el botón secundario en la zona en la que desea agregar un registro de recursos y, a continuación, haga clic en **Agregar registro de recursos DNS**.

    ![Agregar registro de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)

4.  Se abre el cuadro de diálogo **agregar registros de recursos DNS** . En **propiedades de registro de recursos**, haga clic en **servidor DNS** y seleccione el servidor DNS en el que desea agregar uno o más registros de recursos nuevos. En **configurar registros de recursos DNS**, haga clic en **nuevo**.

    ![Configuración de registros de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)

5.  El cuadro de diálogo se expande para mostrar el **nuevo registro de recursos**. Haga clic en **tipo de registro de recurso**.

    ![Tipo de registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)

6.  Se muestra la lista de tipos de registros de recursos. Haga clic en el tipo de registro de recursos que desea agregar.

    ![Seleccionar el tipo de registro que se va a agregar](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)

7.  En **nuevo registro de recursos,** en **nombre**, escriba un nombre de registro de recurso. En **dirección IP**, escriba una dirección IP y, a continuación, seleccione las propiedades de registro de recursos adecuadas para su implementación. Haga clic en **Agregar registro de recursos**.

    ![Agregar registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)

8.  Si no desea crear más registros de recursos nuevos, haga clic en **Aceptar**. Si desea crear más registros de recursos nuevos, haga clic en **nuevo**.

    ![Haga clic en aceptar o en nuevo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)

9. El cuadro de diálogo se expande para mostrar el **nuevo registro de recursos**. Haga clic en **tipo de registro de recurso**. Se muestra la lista de tipos de registros de recursos. Haga clic en el tipo de registro de recursos que desea agregar.

10. En **nuevo registro de recursos,** en **nombre**, escriba un nombre de registro de recurso. En **dirección IP**, escriba una dirección IP y, a continuación, seleccione las propiedades de registro de recursos adecuadas para su implementación. Haga clic en **Agregar registro de recursos**.

    ![Agregar registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)

11. Si desea agregar más registros de recursos, repita el proceso para crear registros. Cuando haya terminado de crear nuevos registros de recursos, haga clic en **aplicar**.

    ![Creación de registros de recursos completos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)

12. El cuadro de diálogo **Agregar registro de recursos** muestra un resumen de los registros de recursos mientras IPAM crea los registros de recursos en el servidor DNS especificado. Cuando los registros se crean correctamente, el **Estado** del registro es **correcto**.

    ![Estado de adición de registro](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)

13. Haga clic en **OK**.

## <a name="see-also"></a>Consulte también
Administración de registros de [recursos DNS](DNS-Resource-Record-Management.md) 
 [Administrar IPAM](Manage-IPAM.md)



