---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Adición de un almacén de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837866"
---
# <a name="add-an-attribute-store"></a>Adición de un almacén de atributos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Cuentas de usuario y cuentas de equipo que requieren acceso a un recurso que está protegido por Active Directory Federation Services \(AD FS\) se almacenan en un almacén de atributos, como los servicios de dominio de Active Directory \(AD DS \). El motor de emisión de notificaciones utiliza almacenes de atributos para recopilar datos que es necesarios emitir notificaciones. Datos de los almacenes de atributos, a continuación, se proyectan como notificaciones.  
  
Puede usar el siguiente procedimiento para agregar un almacén de atributos para el servicio de federación.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para agregar un almacén de atributos  
  
1.  Abra **administración de AD FS**.  
  
2.  En **acciones** haga clic en **agregar un almacén de atributos**.  

![agregar el almacén de atributos](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  En el **agregar un almacén de atributos** diálogo cuadro, configure las siguientes propiedades para el almacén de atributos que desea agregar:  
  
    -   En **nombre para mostrar**, escriba el nombre que desea usar para identificar el almacén de atributos.  
  
    -   En **tipo de almacén de atributo**, seleccione un tipo de almacén de atributos admitidos, bien **Active Directory**, **LDAP**, o **SQL**.  
  
    -   En **cadena de conexión**, si ha seleccionado un protocolo de acceso de directorio ligero \(LDAP\) almacén o un lenguaje de consulta estructurado \(SQL\) almacenar, escriba la cadena que utiliza para establecer una conexión con el almacén de atributos. Para almacenes de atributos de Active Directory, no es necesaria; ninguna cadena de conexión por lo tanto, este campo está deshabilitado.  
  
        > [!NOTE]  
        > AD FS crea automáticamente un almacén de atributos de Active Directory de manera predeterminada.  
 
![agregar el almacén de atributos](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Haga clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[La función de los almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
