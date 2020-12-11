---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creación de una relación de confianza para usuario autenticado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0add6e50ec7f2efd0d8dce380a11164c3bd9796d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048903"
---
# <a name="create-a-relying-party-trust"></a>Creación de una relación de confianza para usuario autenticado


En el siguiente documento se proporciona información sobre la creación manual de una relación de confianza para usuario autenticado y el uso de metadatos de Federación.

## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para crear una relación de confianza para usuario autenticado compatible con notificaciones manualmente

Para agregar una nueva relación de confianza para usuario autenticado mediante el complemento \- de administración de AD FS en y configurar manualmente los valores, realice el siguiente procedimiento en un servidor de Federación.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

1. En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)

3.  En la página de **bienvenida**, elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG)

4.  En la página **Seleccionar origen de datos**, haz clic en **Escribir manualmente los datos acerca del usuario de confianza** y luego en **Siguiente**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust3.PNG)

5.  En la página **Especificar nombre para mostrar**, escriba un nombre en **Nombre para mostrar**, en **Notas** escriba una descripción de esta relación de confianza para usuario autenticado y luego haga clic en **Siguiente**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust4.PNG)

6. En la página **configurar certificado** , si tiene un certificado de cifrado de tokens opcional, haga clic en **examinar** para buscar un archivo de certificado y, a continuación, haga clic en **siguiente**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust5.PNG)

7.  En la página **configurar dirección URL** , realice una de las siguientes acciones o las dos, haga clic en **siguiente** y, a continuación, vaya al paso 8:

    -   Active la casilla **Habilitar la compatibilidad con el \- Protocolo pasivo de WS Federation** . En **\- dirección URL del protocolo pasivo de Federación de WS** del usuario de confianza, escriba la dirección URL de esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.

    -   Active la casilla **Habilitar compatibilidad con el protocolo SAML 2.0 WebSSO** . En **dirección URL del servicio SSO de saml 2,0 de usuario de confianza**, escriba la \( \) dirección URL del punto de conexión del servicio SAML de lenguaje de marcado de aserción de seguridad para esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust6.PNG)

8. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust8.PNG)

9.  En **Choose Access Control Policy** (Elegir directiva de control de acceso), seleccione una directiva y haga clic en **Siguiente**.  Para obtener más información acerca de las directivas de Access Control, consulte [directivas de Access Control en AD FS](Access-Control-Policies-in-AD-FS.md).
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. En la página **Ready to Add Trust** (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust10.PNG)
11. En la página **Finalizar**, haz clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.
![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust11.PNG)

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para crear una relación de confianza para usuario autenticado compatible con notificaciones mediante metadatos de Federación

Para agregar una nueva relación de confianza para usuario autenticado con el complemento Administración de AD FS, importando automáticamente los datos de configuración sobre el socio desde los metadatos de Federación que el asociado publicó en una red local o en Internet, realice el siguiente procedimiento en un servidor de Federación de la organización del asociado de cuenta.

>[!NOTE]
>Aunque la práctica habitual es usar certificados con nombres de host sin calificar https://myserver , como, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplante un servicio de Federación que está publicando metadatos de Federación. Por lo tanto, al consultar los metadatos de Federación, solo debe usar un nombre de dominio completo como https://myserver.contoso.com .

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).


1. En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2. En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust1.PNG)

3. En la página de **bienvenida**, elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust2.PNG)

4. En la página **Seleccionar origen de datos** , haga clic en <strong>importar datos acerca del usuario de confianza publicados en línea o en una red local *. En * * dirección de metadatos de Federación (nombre de host o dirección URL)</strong>, escriba la dirección URL de metadatos de Federación o el nombre de host del asociado y, a continuación, haga clic en **siguiente**.
   ![usuario de confianza](media/Create-a-Relying-Party-Trust/addtrust12.PNG)

5. En la página especificar nombre para mostrar, escriba un nombre en **nombre para mostrar**, en notas escriba una descripción para esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.

6. En la página Elegir reglas de autorización de emisión , selecciona **Permitir a todos los usuarios el acceso a este usuario de confianza** o **Denegar a todos los usuarios el acceso a este usuario de confianza** y haz clic en **Siguiente**.

7. En la página Ready to Add Trust (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.

8. En la página Finalizar , haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo Editar reglas de notificaciones. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta relación de confianza para usuario autenticado, consulta Referencias adicionales.




## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
