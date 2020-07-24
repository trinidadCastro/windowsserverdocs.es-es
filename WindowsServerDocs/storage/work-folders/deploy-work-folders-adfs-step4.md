---
title: 'Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4, configurar el proxy de aplicación Web'
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 06/24/2017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 78636aa1f245450f6c5f133fb1a11a38de17dc22
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965737"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el cuarto paso en la implementación de carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y proxy de aplicación Web. Puede encontrar los demás pasos de este proceso en estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   Las instrucciones que se describen en esta sección son para un entorno de Windows Server 2019 o Windows Server 2016. Si usa Windows Server 2012 R2, siga las instrucciones de [Windows server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).

Para configurar el proxy de aplicación web para su uso con carpetas de trabajo, utilice los procedimientos siguientes.  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de carpeta de AD FS y de trabajo  
Debe instalar los certificados de AD FS y carpetas de trabajo que creó anteriormente (en el paso 1, configurar AD FS y el paso 3, configurar carpetas de trabajo) en el almacén de certificados del equipo local en el equipo en el que se instalará el rol proxy de aplicación Web.  
  
Dado que está instalando certificados autofirmados de los que no se puede realizar un seguimiento en un publicador del almacén de certificados de entidades de certificación raíz de confianza, también debe copiar los certificados en ese almacén.  
  
Para instalar los certificados, siga estos pasos:  
  
1.  Haga clic en **Inicio**y, a continuación, haga clic en **Ejecutar**.  
  
2.  Escriba **MMC**.  
  
3.  En el menú **Archivo** , haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **complementos disponibles** , seleccione **certificados**y, a continuación, haga clic en **Agregar**. Se inicia el Asistente para complementos de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Seleccione **equipo local: (el equipo en el que se está ejecutando esta consola)** y, a continuación, haga clic en **Finalizar**.  
  
7.  Haga clic en **OK**.  
  
8.  Expanda la **consola de carpeta Root\Certificates \( equipo local) \personal\certificados**.  
  
9. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**y haga clic en **importar**.  
  
10. Vaya a la carpeta que contiene el certificado AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de certificados.  
  
11. Repita los pasos 9 y 10, esta vez, busque el certificado de carpetas de trabajo e impórtelo.  
  
12. Expanda la **consola de carpeta Root\Certificates \( equipo local) \Trusted raíz confianza\certificados**.  
  
13. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**y haga clic en **importar**.  
  
14. Vaya a la carpeta que contiene el certificado de AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de entidades de certificación raíz de confianza.  
  
15. Repita los pasos 13 y 14, esta vez, busque el certificado de carpetas de trabajo e impórtelo.  
  
## <a name="install-web-application-proxy"></a>Instalar el proxy de aplicación Web  
Para instalar el proxy de aplicación Web, siga estos pasos:  
  
1.  En el servidor donde va a instalar el proxy de aplicación Web, Abra **Administrador del servidor** e inicie el Asistente para **Agregar roles y características** .  
  
2.  Haga clic en **siguiente** en la primera y en la segunda página del asistente.  
  
3.  En la página **selección de servidor** , seleccione el servidor y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **rol de servidor** , seleccione el rol de **acceso remoto** y, a continuación, haga clic en **siguiente**.  
  
5.  En la página características y acceso remoto, haga clic en **siguiente**.  
  
6.  En la página **servicios de rol** , seleccione proxy de **aplicación web**, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.

7.  En la página **Confirmar selecciones de instalación**, haga clic en **Instalar**.  
  
## <a name="configure-web-application-proxy"></a>Configurar el Proxy de aplicación web  
Para configurar el proxy de aplicación Web, siga estos pasos:  
  
1.  Haga clic en la marca de advertencia en la parte superior de Administrador del servidor y, a continuación, haga clic en el vínculo para abrir el Asistente para la configuración del proxy de aplicación Web.  
  
2.  En la página de bienvenida, presione **siguiente**.  
  
3.  En la página **servidor de Federación** , escriba el nombre del servicio de Federación. En el ejemplo de prueba, es **blueadfs.contoso.com**.  
  
4.  Escriba las credenciales de una cuenta de administrador local en los servidores de Federación. No escriba las credenciales del dominio (por ejemplo, Contoso\Administrador), pero las credenciales locales (por ejemplo, administrador).  
  
5.  En la página **AD FS certificado de proxy** , seleccione el certificado de AD FS que importó anteriormente. En el caso de prueba, es **blueadfs.contoso.com**. Haga clic en **Next**.  
  
6.  La página Confirmación muestra el comando de Windows PowerShell que se ejecutará para configurar el servicio. Haga clic en **Configurar**.  
  
## <a name="publish-the-work-folders-web-application"></a>Publicar la aplicación Web de carpetas de trabajo  
El siguiente paso es publicar una aplicación web que pondrá las carpetas de trabajo a disposición de los clientes. Para publicar la aplicación Web de carpetas de trabajo, siga estos pasos:  
  
1. Abra **Administrador del servidor**y, en el menú **herramientas** , haga clic en **Administración de acceso remoto** para abrir la consola de administración de acceso remoto.  
  
2. En **configuración**, haga clic en **proxy de aplicación web**.  
  
3. En **tareas**, haga clic en **publicar**. Se abre el Asistente para publicar nueva aplicación.  
  
4. En la página de bienvenida, haga clic en **Siguiente**.  
  
5. En la página **autenticación previa** , seleccione **servicios de Federación de Active Directory (AD FS) (AD FS)** y haga clic en **siguiente**.  
  
6. En la página **clientes de soporte técnico** , seleccione **OAuth2**y haga clic en **siguiente**.

7. En la página **usuario de confianza** , seleccione **carpetas de trabajo**y, a continuación, haga clic en **siguiente**. Esta lista se publica en el proxy de aplicación web desde AD FS.  
  
8. En la página **configuración de publicación** , escriba lo siguiente y, a continuación, haga clic en **siguiente**:  
  
   -   El nombre que desea utilizar para la aplicación Web  
  
   -   La dirección URL externa para carpetas de trabajo  
  
   -   El nombre del certificado de carpetas de trabajo  
  
   -   La dirección URL de back-end para carpetas de trabajo  
  
   De forma predeterminada, el asistente hace que la dirección URL de back-end sea la misma que la dirección URL externa.  
  
   En el ejemplo de prueba, use estos valores:  
  
   Nombre: **WorkFolders**  
  
   Dirección URL externa:**https://workfolders.contoso.com**  
  
   Certificado externo: **el certificado de carpetas de trabajo que instaló anteriormente**  
  
   Dirección URL del servidor back-end:**https://workfolders.contoso.com**  
  
9. La página Confirmación muestra el comando de Windows PowerShell que se ejecutará para publicar la aplicación. Haga clic en **Publicar**.  
  
10. En la página **resultados** , debería ver que la aplicación se publicó correctamente.
    >[!NOTE]
    > Si tiene varios servidores de carpetas de trabajo, debe publicar una aplicación Web de carpetas de trabajo para cada servidor de carpetas de trabajo (Repita los pasos 1-10).  
  
Paso siguiente: [implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>Consulte también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)  
  
