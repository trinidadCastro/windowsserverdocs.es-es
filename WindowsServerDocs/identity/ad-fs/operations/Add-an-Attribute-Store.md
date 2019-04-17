---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: "Agregar un almacén de atributo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>Agregar un almacén de atributo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Cuentas de usuario y cuentas de equipo que requieren acceso a un recurso que está protegido por los servicios de federación de Active Directory \(AD FS\) se almacenan en un almacén de atributo, como los servicios de dominio de Active Directory \(AD DS\). El motor de emisión reclamaciones emplea almacenes de atributo para recopilar los datos necesarios para emitir notificaciones. Datos de las tiendas de atributo, a continuación, se proyectan como notificaciones.  
  
Puedes usar el siguiente procedimiento para agregar un almacén de atributo a los servicios de federación.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para agregar un almacén de atributo  
  
1.  Abre **AD FS administración**.  
  
2.  En **acciones** haga clic en **agregar un almacén de atributo**.  

![Agregar atributo almacén](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  En la **agregar un almacén de atributo** diálogo cuadro, configura las siguientes propiedades para la tienda de atributo que quieres agregar:  
  
    -   En **nombre para mostrar**, escribe el nombre que quieras usar para identificar el almacén de atributo.  
  
    -   En **tipo de almacén de atributo**, selecciona un tipo de almacén atributo admitido, bien **Active Directory**, **LDAP**, o **SQL**.  
  
    -   En **cadena de conexión**, si se ha seleccionado un almacén de protocolo ligero de acceso a directorios \(LDAP\) o en un almacén de lenguaje de consulta estructurado \(SQL\), escribe la cadena que se usa para establecer una conexión a la tienda de atributo. Almacenes de atributo de Active Directory, no es necesaria; cadena de conexión por lo tanto, este campo está deshabilitado.  
  
        > [!NOTE]  
        > AD FS crea automáticamente un almacén de atributo de Active Directory, de manera predeterminada.  
 
![Agregar atributo almacén](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Haz clic en **Aceptar**.  
  
## <a name="additional-references"></a>Referencias adicionales  

[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
  
[El rol de tiendas de atributo](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
