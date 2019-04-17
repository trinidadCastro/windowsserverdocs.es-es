---
title: "Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 5; configurar clientes"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: fa8b2b15ff411a59b28308a329d7ca2341ef0886
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: Paso 5; configurar clientes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el quinto paso para implementar Carpetas de trabajo con los Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web. Puedes encontrar el resto de pasos de este proceso en estos temas:  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web (WAP): información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 1; configurar AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 2; trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 3; configurar Carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 4; configurar el Proxy de aplicación web](deploy-work-folders-adfs-step4.md)  
  
Usa los siguientes procedimientos para configurar los clientes de Windows que estén unidos (o no) a un dominio. Puedes usar a estos clientes para comprobar si los archivos de tus clientes se sincronizan correctamente en Carpetas de trabajo.  
  
## <a name="set-up-a-domain-joined-client"></a>Configurar un cliente unido a un dominio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de AD FS y de Carpetas de trabajo  
Debes instalar los certificados de AD FS y de Carpetas de trabajo que creaste anteriormente en el almacén de certificados de equipo local que se encuentra en el equipo cliente unido al dominio.  
  
Como estás instalando certificados autofirmados cuyo origen no se puede rastrear hasta ningún editor del almacén de certificados Entidades de certificación raíz de confianza, debes copiar esos certificados en ese almacén.  
  
Para instalar los certificados, sigue estos pasos:  
  
1.  Haz clic en **Inicio**y, a continuación, en **Ejecutar**.  
  
2.  Escribe **MMC**.  
  
3.  En el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
4.  En la lista **Complementos disponibles**, selecciona **Certificados** y, a continuación, haz clic en **Agregar**. Se iniciará el asistente para el complemento de certificados.  
  
5.  Selecciona **Cuenta de equipo** y, a continuación, haz clic en **Siguiente**.  
  
6.  Selecciona **Equipo local: (el equipo en el que se ejecuta la consola)** y haz clic en **Finalizar**.  
  
7.  Haz clic en **Aceptar**.  
  
8.  Expande la carpeta Console Root\Certificates\(Equipo local)\Personal\Certificates.  
  
9. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
10. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén de certificados.  
  
11. Repite los pasos 9 y 10, y esta vez busca el certificado de Carpetas de trabajo e impórtalo.  
  
12. Expande la carpeta Console Root\Certificates\(Equipo local)\Trusted Root Certification Authorities\Certificates.  
  
13. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
14. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén Entidades de certificación raíz de confianza.  
  
15. Repite los pasos 13 y 14; esta vez busca el certificado de Carpetas de trabajo e impórtalo.  
  
### <a name="configure-work-folders-on-the-client"></a>Configurar Carpetas de trabajo en el cliente  
Para configurar Carpetas de trabajo en el equipo cliente, sigue estos pasos:  
  
1.  En el equipo cliente, abre **Panel de Control** y haz clic en **Carpetas de trabajo**.  
  
2.  Haz clic en **Configurar Carpetas de trabajo**.  
  
3.  En la página **Escribe la dirección de correo electrónico del trabajo**, escribe la dirección de correo electrónico del usuario (por ejemplo, user@contoso.com) o la dirección URL de Carpetas de trabajo (en el ejemplo de prueba sería https://workfolders.contoso.com) y, a continuación, haz clic en **Siguiente**.  
  
4.  Si el usuario está conectado a la red corporativa, la autenticación se realiza mediante la autenticación integrada de Windows. Si el usuario no está conectado a la red corporativa, la autenticación se realiza mediante ADFS (OAuth); asimismo, se le pedirán al usuario las credenciales. Escribe las credenciales y haz clic en **Aceptar**.  
  
5.  Una vez que hayas realizado la autenticación, se mostrará la página **Introducción a Carpetas de trabajo**, donde tienes la opción de cambiar la ubicación del directorio de Carpetas de trabajo. Haz clic en **Siguiente**.  
  
6.  En la página **Directivas de seguridad** se indican las directivas de seguridad de Carpetas de trabajo que configuraste. Haz clic en **Siguiente**.  
  
7.  Se mostrará un mensaje que indica que Carpetas de trabajo comenzó la sincronización del equipo. Haz clic en **Cerrar**.  
  
8.  En la página **Administrar Carpetas de trabajo**, se muestra la cantidad de espacio disponible que hay en el servidor, el estado de la sincronización, etcétera. Si fuera necesario, puedes volver a escribir las credenciales aquí. Cierra la ventana  
  
9. La carpeta Carpetas de trabajo se abrirá automáticamente. Puedes agregar contenido a esta carpeta para sincronizarlo entre los dispositivos.  
  
    A modo de ejemplo, agrega un archivo de prueba a esta carpeta de Carpetas de trabajo. Después de configurar Carpetas de trabajo en el equipo que no está unido a un dominio, podrás sincronizar archivos entre las Carpetas de trabajo de cada equipo.  
  
## <a name="set-up-a-non-domain-joined-client"></a>Configurar un cliente que no esté unido a un dominio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar los certificados de AD FS y de Carpetas de trabajo  
Instala los certificados de AD FS y de Carpetas de trabajo en el equipo que no está unido a un dominio; para ello, debes usar el mismo procedimiento que usaste en el equipo unido al dominio.  
  
### <a name="update-the-hosts-file"></a>Actualizar el archivo de hosts  
Debes actualizar en el entorno de prueba el archivo de hosts que se encuentra en el cliente, porque no se crearon registros DNS públicos para Carpetas de trabajo. Agrega estas entradas en el archivo de hosts:  
  
-  workfolders.dominio  
  
-  nombre de servicio de AD FS.dominio  
  
-  enterpriseregistration.dominio  
  
En el ejemplo de prueba, usa estos valores:  
  
-  **10.0.0.10 workfolders.contoso.com**  
  
-  **10.0.0.10 blueadfs.contoso.com**  
  
-  **10.0.0.10 enterpriseregistration.contoso.com**  
  
### <a name="configure-work-folders-on-the-client"></a>Configurar Carpetas de trabajo en el cliente  
Configura Carpetas de trabajo en el equipo que no está unido a un dominio mediante el mismo procedimiento que usaste en el equipo unido al dominio.  
  
Cuando se abra la nueva carpeta de Carpetas de trabajo en este cliente, podrás ver que el archivo del equipo que está unido al dominio ya se sincronizó con el equipo que no está unido al dominio. Ya puedes agregar contenido a la carpeta para que se sincronice entre dispositivos.  
  
Así concluye el procedimiento para implementar Carpetas de trabajo, AD FS y el Proxy de aplicación web mediante la interfaz de usuario de Windows Server.  
  
## <a name="see-also"></a>Consulta también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)  
  

