---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Adición de un almacén de atributos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 352c66bbbd2aade0f212605ca1416fb6581dab0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962667"
---
# <a name="add-an-attribute-store"></a>Adición de un almacén de atributos


Las cuentas de usuario y las cuentas de equipo que requieren acceso a un recurso protegido por Servicios de federación de Active Directory (AD FS) \( AD FS \) se almacenan en un almacén de atributos, como Active Directory Domain Services \( AD DS \) . El motor de emisión de notificaciones usa almacenes de atributos para recopilar los datos necesarios para emitir notificaciones. Los datos de los almacenes de atributos se proyectan entonces como notificaciones.  
  
Puede usar el siguiente procedimiento para agregar un almacén de atributos al Servicio de federación.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para agregar un almacén de atributos  
  
1.  Abra **Administración de AD FS**.  
  
2.  En **acciones** , haga clic en **Agregar un almacén de atributos**.  

![Agregar almacén de atributos](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. En el cuadro de diálogo **Agregar un almacén de atributos** , configure las siguientes propiedades para el almacén de atributos que desea agregar:  
  
   -   En **nombre para mostrar**, escriba el nombre que desea usar para identificar el almacén de atributos.  
  
   -   En **tipo de almacén de atributos**, seleccione un tipo de almacén de atributos admitido, ya sea **Active Directory**, **LDAP**o **SQL**.  
  
   -   En **cadena de conexión**, si ha seleccionado un almacén LDAP de Protocolo ligero de acceso a directorios \( \) o un lenguaje de consulta estructurado \( \) almacén SQL, escriba la cadena que usó para establecer una conexión con el almacén de atributos. En el caso de Active Directory almacenes de atributos, no se necesita ninguna cadena de conexión; por lo tanto, este campo está deshabilitado.  
  
       > [!NOTE]  
       > AD FS crea automáticamente un almacén de atributos de Active Directory de manera predeterminada.  
 
![Agregar almacén de atributos](media/Add-an-Attribute-Store/addstore2.PNG) 

4. Haga clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../ad-fs-operations.md)
  
[El rol de los almacenes de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
