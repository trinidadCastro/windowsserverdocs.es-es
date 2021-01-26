---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Creación de una relación de confianza para usuario autenticado
description: Obtén información sobre cómo crear una relación de confianza para usuario autenticado manualmente y usar metadatos de Federación.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 949ff74ea3fbb4518b1463709d798c945d1a3b5b
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781909"
---
# <a name="create-a-relying-party-trust"></a>Creación de una relación de confianza para usuario autenticado


En el siguiente documento se proporciona información sobre la creación manual de una relación de confianza para usuario autenticado y el uso de metadatos de Federación.

## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para crear una relación de confianza para usuario autenticado compatible con notificaciones manualmente

Para agregar una nueva relación de confianza para usuario autenticado mediante el complemento \- de administración de AD FS en y configurar manualmente los valores, realice el siguiente procedimiento en un servidor de Federación.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

1. En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2. En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.

    ![Captura de pantalla del cuadro de diálogo AD FS con la opción Agregar relación de confianza para usuario autenticado en el panel de acciones llamado.](media/Create-a-Relying-Party-Trust/addtrust1.PNG)

3. En la página de **bienvenida**, elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.

    ![Captura de pantalla de la Página principal del Asistente para agregar relación de confianza para usuario autenticado que muestra la opción notificaciones habilitadas.](media/Create-a-Relying-Party-Trust/addtrust2.PNG)

4. En la página **Seleccionar origen de datos**, haz clic en **Escribir manualmente los datos acerca del usuario de confianza** y luego en **Siguiente**.

    ![Captura de pantalla de la página Seleccionar origen de datos del Asistente para agregar relación de confianza para usuario autenticado que muestra la opción escribir datos sobre el usuario de confianza manualmente seleccionada.](media/Create-a-Relying-Party-Trust/addtrust3.PNG)

5. En la página **Especificar nombre para mostrar**, escriba un nombre en **Nombre para mostrar**, en **Notas** escriba una descripción de esta relación de confianza para usuario autenticado y luego haga clic en **Siguiente**.

    ![Captura de pantalla de la página especificar nombre para mostrar del Asistente para agregar relación de confianza para usuario autenticado.](media/Create-a-Relying-Party-Trust/addtrust4.PNG)

6. En la página **configurar certificado** , si tiene un certificado de cifrado de tokens opcional, haga clic en **examinar** para buscar un archivo de certificado y, a continuación, haga clic en **siguiente**.

    ![Captura de pantalla de la página configurar certificado del Asistente para agregar relación de confianza para usuario autenticado que muestra el botón examinar.](media/Create-a-Relying-Party-Trust/addtrust5.PNG)

7. En la página **configurar dirección URL** , realice una de las siguientes acciones o las dos, haga clic en **siguiente** y, a continuación, vaya al paso 8:

    - Active la casilla **Habilitar la compatibilidad con el \- Protocolo pasivo de WS Federation** . En **\- dirección URL del protocolo pasivo de Federación de WS** del usuario de confianza, escriba la dirección URL de esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.

    - Active la casilla **Habilitar compatibilidad con el protocolo SAML 2.0 WebSSO** . En **dirección URL del servicio SSO de saml 2,0 de usuario de confianza**, escriba la \( \) dirección URL del punto de conexión del servicio SAML de lenguaje de marcado de aserción de seguridad para esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.

    ![Captura de pantalla de la página configurar certificado del Asistente para agregar relación de confianza para usuario autenticado que muestra la configuración que se explicó anteriormente.](media/Create-a-Relying-Party-Trust/addtrust6.PNG)

8. En la página **Configurar identificadores**, especifica uno o varios identificadores para este usuario de confianza, haz clic en **Agregar** para agregarlos a la lista y haz clic en **Siguiente**.

    ![Captura de pantalla de la página configurar identificadores del Asistente para agregar relación de confianza para usuario autenticado que muestra los identificadores agregados a la sección identificadores de confianza para usuario autenticado.](media/Create-a-Relying-Party-Trust/addtrust8.PNG)

9. En **Choose Access Control Policy** (Elegir directiva de control de acceso), seleccione una directiva y haga clic en **Siguiente**.  Para obtener más información acerca de las directivas de Access Control, consulte [directivas de Access Control en AD FS](Access-Control-Policies-in-AD-FS.md).

    ![Captura de pantalla de la página elegir Directiva de Access Control del Asistente para agregar relación de confianza para usuario autenticado que muestra la opción permitir todos y requerir MFA resaltada.](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. En la página **Ready to Add Trust** (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.

    ![Captura de pantalla de la página listo para agregar confianza del Asistente para agregar relación de confianza para usuario autenticado.](media/Create-a-Relying-Party-Trust/addtrust10.PNG)

11. En la página **Finalizar**, haz clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo **Editar reglas de notificaciones**.

    ![Captura de pantalla de la página finalizar del Asistente para agregar relación de confianza para usuario autenticado.](media/Create-a-Relying-Party-Trust/addtrust11.PNG)

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para crear una relación de confianza para usuario autenticado compatible con notificaciones mediante metadatos de Federación

Para agregar una nueva relación de confianza para usuario autenticado con el complemento Administración de AD FS, importando automáticamente los datos de configuración sobre el socio desde los metadatos de Federación que el asociado publicó en una red local o en Internet, realice el siguiente procedimiento en un servidor de Federación de la organización del asociado de cuenta.

>[!NOTE]
>Aunque la práctica habitual es usar certificados con nombres de host sin calificar https://myserver , como, estos certificados no tienen ningún valor de seguridad y pueden permitir que un atacante suplante un servicio de Federación que está publicando metadatos de Federación. Por lo tanto, al consultar los metadatos de Federación, solo debe usar un nombre de dominio completo como https://myserver.contoso.com .

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

1. En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2. En **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.

    ![Otra captura de pantalla del cuadro de diálogo AD FS con la opción Agregar relación de confianza para usuario autenticado en el panel de acciones.](media/Create-a-Relying-Party-Trust/addtrust1.PNG)

3. En la página de **bienvenida**, elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.

    ![Otra captura de pantalla de la Página principal del Asistente para agregar relación de confianza para usuario autenticado que muestra la opción notificaciones habilitadas.](media/Create-a-Relying-Party-Trust/addtrust2.PNG)

4. En la página **Seleccionar origen de datos**, haz clic en **Importar los datos acerca del usuario de confianza publicados en línea o en una red local**. En **Dirección de metadatos de federación (nombre de host o dirección URL)**, escribe la dirección URL de los metadatos de federación o el nombre de host del asociado y haz clic en **Siguiente**.

    ![Captura de pantalla de la página Seleccionar origen de datos del Asistente para agregar relación de confianza para usuario autenticado que muestra la opción Importar datos sobre la persona de confianza Publicada en línea o en una red local seleccionada.](media/Create-a-Relying-Party-Trust/addtrust12.PNG)

5. En la página especificar nombre para mostrar, escriba un nombre en **nombre para mostrar**, en notas escriba una descripción para esta relación de confianza para usuario autenticado y, a continuación, haga clic en **siguiente**.

6. En la página Elegir reglas de autorización de emisión , selecciona **Permitir a todos los usuarios el acceso a este usuario de confianza** o **Denegar a todos los usuarios el acceso a este usuario de confianza** y haz clic en **Siguiente**.

7. En la página Ready to Add Trust (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.

8. En la página Finalizar , haga clic en **Cerrar**. Esta acción muestra automáticamente el cuadro de diálogo Editar reglas de notificaciones. Para obtener más información sobre cómo continuar agregando reglas de notificaciones para esta relación de confianza para usuario autenticado, consulta Referencias adicionales.




## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
