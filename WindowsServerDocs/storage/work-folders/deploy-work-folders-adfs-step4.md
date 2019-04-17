---
title: "Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 4; configurar el Proxy de aplicación web"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 6/242017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 1f452fd1e2f054c449660eb0ee12642fefe4da8f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: Paso 4; configurar el Proxy de aplicación web

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el cuarto paso para implementar Carpetas de trabajo con los Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web. Puedes encontrar el resto de pasos de este proceso en estos temas:  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web (WAP): información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 1; configurar AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 2; trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 3; configurar Carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: Paso 5; configurar clientes](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   Las instrucciones incluidas en esta sección toman como referencia un entorno de Server 2016. Si estás usando Windows Server 2012 R2, lee el artículo en el que se detallan las [instrucciones de Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Para configurar el Proxy de aplicación web y así poder usarlo con Carpetas de trabajo, usa los siguientes procedimientos.  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de AD FS y de Carpetas de trabajo  
Debes instalar los certificados de AD FS y Carpetas de trabajo que creaste anteriormente (en el paso 1, Configurar AD FS y, en el paso 3, Configurar carpetas de trabajo) en el almacén de certificados del equipo local que se encuentra en el equipo donde se instalará el rol Proxy de aplicación web.  
  
Como estás instalando certificados autofirmados cuyo origen no se puede rastrear hasta ningún editor del almacén de certificados Entidades de certificación raíz de confianza, debes copiar esos certificados en ese almacén.  
  
Para instalar los certificados, sigue estos pasos:  
  
1.  Haz clic en **Inicio**y, a continuación, en **Ejecutar**.  
  
2.  Escribe **MMC**.  
  
3.  En el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
4.  En la lista **Complementos disponibles**, selecciona **Certificados** y, a continuación, haz clic en **Agregar**. Se iniciará el asistente del complemento de certificados.  
  
5.  Selecciona **Cuenta de equipo** y, a continuación, haz clic en **Siguiente**.  
  
6.  Selecciona **Equipo local: (el equipo en el que se ejecuta la consola)** y haz clic en **Finalizar**.  
  
7.  Haz clic en **Aceptar**.  
  
8.  Expande la carpeta **Console Root\Certificates\(Equipo local)\Personal\Certificates**..  
  
9. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
10. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén de certificados.  
  
11. Repite los pasos 9 y 10, y esta vez busca el certificado de Carpetas de trabajo e impórtalo.  
  
12. Expande la carpeta **Console Root\Certificates\(Equipo local)\Trusted Root Certification Authorities\Certificates**.  
  
13. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
14. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén Entidades de certificación raíz de confianza.  
  
15. Repite los pasos 13 y 14; esta vez busca el certificado de Carpetas de trabajo e impórtalo.  
  
## <a name="install-web-application-proxy"></a>Instalar el Proxy de aplicación web  
Para instalar el Proxy de aplicación web, sigue estos pasos:  
  
1.  En el servidor donde vayas a instalar el Proxy de aplicación Web, abre **Administrador del servidor** e inicia el asistente para **Agregar roles y características**.  
  
2.  Haz clic en **Siguiente** en la primera y segunda páginas del asistente.  
  
3.  En la página **Selección de servidores**, selecciona tu servidor y haz clic en **Siguiente**.  
  
4.  En la página **Rol del servidor**, selecciona el rol **Acceso remoto** y, a continuación, haz clic en **Siguiente**.  
  
5.  En la página de características y acceso remoto, haz clic en **Siguiente**.  
  
6.  En la página **Servicios de rol**, selecciona **Proxy de aplicación web**, haz clic en **Agregar características** y, a continuación, haz clic en **Siguiente**.

7.  En la página **Confirmar selecciones de instalación**, haz clic en **Instalar**.  
  
## <a name="configure-web-application-proxy"></a>Configurar el Proxy de aplicación web  
Para configurar el Proxy de aplicación web, sigue estos pasos:  
  
1.  Haz clic en el marcador de advertencia que se encuentra en la parte superior del Administrador del servidor y, a continuación, haz clic en el vínculo para abrir el asistente para la configuración del Proxy de aplicación web.  
  
2.  En la página principal, haz clic en **Siguiente**.  
  
3.  En la página **Servidor de federación**, escribe el nombre del Servicio de federación. En este ejemplo de prueba, el valor es **blueadfs.contoso.com**.  
  
4.  Escribe las credenciales de una cuenta de administrador local en los servidores de federación. Recuerda que no debes acceder a las credenciales de dominio (por ejemplo, contoso\administrador); tienes que acceder a las credenciales locales (por ejemplo, administrador).  
  
5.  En la página **Certificado de proxy de AD FS**, selecciona el certificado de AD FS que hayas importado anteriormente. En este caso de prueba, el valor es **blueadfs.contoso.com**. Haz clic en **Siguiente**.  
  
6.  La página de confirmación te mostrará el comando de Windows PowerShell que se ejecutará para configurar el servicio. Haz clic en **Configurar**.  
  
## <a name="publish-the-work-folders-web-application"></a>Publicar la aplicación web Carpetas de trabajo  
En el siguiente paso publicaremos una aplicación web que pondrá a disposición de los clientes Carpetas de trabajo. Para publicar la aplicación web Carpetas de trabajo, sigue estos pasos:  
  
1.  Abre el **Administrador del servidor** y, en el menú **Herramientas**, haz clic en **Administración de acceso remoto** para abrir la consola de administración de acceso remoto.  
  
2.  En **Configuración**, haz clic en **Proxy de aplicación web**.  
  
3.  En **Tareas**, haz clic en **Publicar**. Se abrirá el asistente para publicar nuevas aplicaciones.  
  
4.  En la página principal, haz clic en **Siguiente**.  
  
5.  En la página **Autenticación previa**, selecciona **Servicios de federación de Active Directory (AD FS)** y, a continuación, haz clic en **Siguiente**.  
  
6.  En la página **Admitir clientes**, selecciona **OAuth2** y haz clic en **Siguiente**.

7.  En la página **Usuario de confianza**, selecciona **Carpetas de trabajo** y, a continuación, haz clic en **Siguiente**. Esta lista se publica en el Proxy de aplicación web de AD FS.  
  
8.  En la página **Configuración de publicación**, escribe lo siguiente y, a continuación, haz clic en **Siguiente**:  
  
    -   El nombre de la aplicación web  
  
    -   La dirección URL externa de Carpetas de trabajo  
  
    -   El nombre del certificado de Carpetas de trabajo  
  
    -   La dirección URL de back-end de Carpetas de trabajo  
  
    El asistente creará tanto la URL de back-end como la URL externa de manera predeterminada.  
  
    En el ejemplo de prueba, usa estos valores:  
  
    Nombre: **WorkFolders**  
  
    URL externa: **https://workfolders.contoso.com**  
  
    Certificado externo: **es el certificado de Carpetas de trabajo que se instaló anteriormente**  
  
    URL del servidor back-end: **https://workfolders.contoso.com**  
  
9.  La página de confirmación muestra el comando de Windows PowerShell que se ejecutará para publicar la aplicación. Haz clic en **Publicar**.  
  
10. En la página **Resultados** verás que la aplicación se publicó correctamente.
   >[!NOTE]
   > Si tienes varios servidores de Carpetas de trabajo, debes publicar una aplicación web de Carpetas de trabajo para cada servidor de Carpetas de trabajo (repite los pasos 1a10).  
  
Siguiente paso: [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 5; configurar clientes](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>Consulta también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)  
  

