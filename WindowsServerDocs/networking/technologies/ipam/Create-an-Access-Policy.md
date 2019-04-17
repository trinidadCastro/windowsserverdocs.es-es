---
title: Crear una directiva de acceso
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>Crear una directiva de acceso

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para crear una directiva de acceso en la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Puedes crear una directiva de acceso para un usuario específico o para un grupo de usuarios en Active Directory. Al crear una directiva de acceso, debes seleccionar un rol IPAM integrado o una función personalizada que hayas creado. Para obtener más información sobre los roles personalizados, consulta [crear un rol de usuario para el Control de acceso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Para crear una directiva de acceso  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, haz clic en **el CONTROL de acceso**. En el panel de navegación inferior, haz clic en **las directivas de acceso**y, a continuación, haz clic en **Agregar directiva de acceso**.  
  
    ![Agregar directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  La **Agregar directiva de acceso** abre el cuadro de diálogo. En **configuración de usuario**, haz clic en **agregar**.  
  
    ![Agregar directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  La **Seleccionar usuarios o grupos** abre el cuadro de diálogo. Haz clic en **ubicaciones**.  
  
    ![Ubicaciones de usuario o grupo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  La **ubicaciones** abre el cuadro de diálogo. Busca la ubicación que contiene la cuenta de usuario, seleccione la ubicación y, a continuación, haz clic en **Aceptar**. La **ubicaciones** cierra el cuadro de diálogo.  
  
    ![Selecciona la ubicación](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  En la **Seleccionar usuarios o grupos** cuadro de diálogo **escribe el nombre de objeto para seleccionar**, escribe el nombre de cuenta de usuario para el que quieres crear una directiva de acceso. Haz clic en **Aceptar**.  
  
7.  En **Agregar directiva de acceso**, en **configuración de usuario**, **alias del usuario** contiene ahora la cuenta de usuario al que se aplica la directiva. En **configuración de acceso**, haz clic en **nueva**.  
  
    ![Nueva configuración de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  En **Agregar directiva de acceso**, **configuración de acceso** cambia a **nueva configuración**.  
  
    ![Nombre del cuadro de diálogo Cambiar a la nueva opción de configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Haz clic en **seleccione rol** para expandir la lista de funciones. Selecciona una de las funciones integradas, o bien, si han creado nuevas funciones, selecciona uno de los roles que creaste. Por ejemplo, si creaste el rol de IPAMSrv a aplicar al usuario, haz clic en **IPAMSrv**.  
  
    ![Selecciona el rol](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Haz clic en **Agregar configuración**.  
  
    ![Agregar la nueva opción de configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. La función se agregó a la directiva de acceso. Para crear directivas de acceso adicional, haz clic en **aplicar**y, a continuación, repite estos pasos para cada directiva que quieres crear. Si no desea crear directivas adicionales, haz clic en **Aceptar**.  
  
    ![Haz clic en Aplicar o en Aceptar](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. En el panel de pantalla de la consola IPAM cliente, comprueba que se crea la nueva directiva de acceso.  
  
    ![Ver la nueva directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Control de acceso basadas en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


