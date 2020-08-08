---
title: 'Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes'
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: fd8015b1a72477c00fda0c3483bbdd6504ff86b8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965822"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el quinto paso en la implementación de carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y proxy de aplicación Web. Puede encontrar los demás pasos de este proceso en estos temas:

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general](deploy-work-folders-adfs-overview.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS](deploy-work-folders-adfs-step1.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web](deploy-work-folders-adfs-step4.md)

Use los procedimientos siguientes para configurar los clientes de Windows Unidos a un dominio y no Unidos a un dominio. Puede usar estos clientes para comprobar si los archivos se sincronizan correctamente entre las carpetas de trabajo de los clientes.

## <a name="set-up-a-domain-joined-client"></a>Configuración de un cliente unido a un dominio

### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de carpeta de AD FS y de trabajo
Debe instalar los certificados de AD FS y carpetas de trabajo que creó anteriormente en el almacén de certificados del equipo local en el equipo cliente unido al dominio.

Dado que está instalando certificados autofirmados de los que no se puede realizar un seguimiento en un publicador del almacén de certificados de entidades de certificación raíz de confianza, también debe copiar los certificados en ese almacén.

Para instalar los certificados, siga estos pasos:

1.  Haga clic en **Inicio**y, a continuación, haga clic en **Ejecutar**.

2.  Escriba **MMC**.

3.  En el menú **Archivo** , haga clic en **Agregar o quitar complemento**.

4.  En la lista **complementos disponibles** , seleccione **certificados**y, a continuación, haga clic en **Agregar**. \-Se inicia el Asistente para el complemento de certificados.

5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.

6.  Seleccione **equipo local: (el equipo en el que se está ejecutando esta consola)** y, a continuación, haga clic en **Finalizar**.

7.  Haga clic en **Aceptar**.

8.  Expanda la consola de carpeta Root\Certificates \( equipo local) \Personal\Certificates.

9. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**y haga clic en **importar**.

10. Vaya a la carpeta que contiene el certificado AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de certificados.

11. Repita los pasos 9 y 10, esta vez, busque el certificado de carpetas de trabajo e impórtelo.

12. Expanda la consola de carpeta Root\Certificates \( equipo local) \Trusted raíz Authorities\Certificates.

13. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**y haga clic en **importar**.

14. Vaya a la carpeta que contiene el certificado de AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de entidades de certificación raíz de confianza.

15. Repita los pasos 13 y 14, esta vez, busque el certificado de carpetas de trabajo e impórtelo.

### <a name="configure-work-folders-on-the-client"></a>Configurar carpetas de trabajo en el cliente
Para configurar carpetas de trabajo en el equipo cliente, siga estos pasos:

1. En el equipo cliente, abra el **Panel de control** y haga clic en carpetas de **trabajo**.

2. Haga clic en **configurar carpetas de trabajo**.

3. En la página **Escriba su dirección de correo electrónico del trabajo** , escriba la dirección de correo electrónico del usuario (por ejemplo, user@contoso.com ) o la dirección URL de carpetas de trabajo (en el ejemplo de prueba, https: \/ /workfolders.contoso.com) y, a continuación, haga clic en **siguiente**.

4. Si el usuario está conectado a la red corporativa, la autenticación se realiza mediante la autenticación integrada de Windows. Si el usuario no está conectado a la red corporativa, ADFS (OAuth) realiza la autenticación y se solicitarán las credenciales al usuario. Escriba sus credenciales y haga clic en **Aceptar**.

5. Una vez autenticado, se muestra la página **Introducción a las carpetas de trabajo** , donde puede cambiar la ubicación del directorio carpetas de trabajo de forma opcional. Haga clic en **Next**.

6. En la página **directivas de seguridad** se enumeran las directivas de seguridad que se configuran para carpetas de trabajo. Haga clic en **Next**.

7. Aparece un mensaje que indica que las carpetas de trabajo han empezado a sincronizarse con el equipo. Haga clic en **Cerrar**.

8. La página **administrar carpetas de trabajo** muestra la cantidad de espacio disponible en el servidor, el estado de sincronización, etc. Si es necesario, puede volver a escribir sus credenciales aquí. Cierre la ventana.

9. La carpeta carpetas de trabajo se abre automáticamente. Puede agregar contenido a esta carpeta para sincronizar entre los dispositivos.

    En el caso del ejemplo de prueba, agregue un archivo de prueba a esta carpeta carpetas de trabajo. Después de configurar carpetas de trabajo en el equipo no unido a un dominio, podrá sincronizar archivos entre las carpetas de trabajo de cada equipo.

## <a name="set-up-a-non-domain-joined-client"></a>Configuración de un cliente no unido a un dominio

### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de carpeta de AD FS y de trabajo
Instale los certificados de carpetas de AD FS y de trabajo en la máquina no unida a un dominio con el mismo procedimiento que usó para el equipo unido a un dominio.

### <a name="update-the-hosts-file"></a>Actualizar el archivo de hosts
El archivo de hosts en el cliente que no está unido al dominio debe actualizarse para el entorno de prueba, porque no se ha creado ningún registro DNS público para carpetas de trabajo. Agregue estas entradas al archivo de hosts:

-  workfolders. dominio

-  Nombre del servicio de AD FS. dominio

-  enterpriseregistration. dominio

En el ejemplo de prueba, use estos valores:

-  **10.0.0.10 workfolders.contoso.com**

-  **10.0.0.10 blueadfs.contoso.com**

-  **10.0.0.10 enterpriseregistration.contoso.com**

### <a name="configure-work-folders-on-the-client"></a>Configurar carpetas de trabajo en el cliente
Configure carpetas de trabajo en la máquina no unida a un dominio mediante el mismo procedimiento que usó para el equipo unido a un dominio.

Cuando se abre la carpeta carpetas de trabajo nueva en este cliente, puede ver que el archivo de la máquina unida al dominio ya se ha sincronizado con la máquina no unida a un dominio. Puede empezar a agregar contenido a la carpeta para sincronizar entre los dispositivos.

Esto concluye el procedimiento para implementar carpetas de trabajo, AD FS y el proxy de aplicación web a través de la interfaz de usuario de Windows Server.

## <a name="see-also"></a>Consulte también
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)


