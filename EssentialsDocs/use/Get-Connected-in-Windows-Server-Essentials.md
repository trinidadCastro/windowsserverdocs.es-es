---
title: Conéctese en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 04d09574046474da5bee4437628ade9646cf58ca
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371240"
---
# <a name="get-connected-in-windows-server-essentials"></a>Conéctese en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Puede conectar los equipos al servidor de Windows Server Essentials mediante el software del Conector. El software del Conector se instala cuando conecta un equipo al servidor mediante el asistente para conectar un equipo al servidor. Puede iniciar este asistente escribiendo **http://< servername\>/Connect**, donde **< ServerName\>** es el nombre del servidor.  

 En este tema:  


-   [Preparar la conexión de equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Conectar equipos al servidor mediante el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Uso de Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Preparar la conexión de equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Conectar equipos al servidor mediante el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Uso de Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  


##  <a name="BKMK_A"></a>Preparar la conexión de equipos al servidor  
 En esta sección se describe el software del Conector, los sistemas operativos que son compatibles con Windows Server Essentials, las tareas de requisitos previos que deben completarse antes de conectar los equipos al servidor y los cambios que realiza el servidor a los equipos al ejecutar el software del Conector.  


-   [Información general del software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Requisitos previos para conectar un equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Requisitos previos para conectar un equipo Mac a la red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Sistemas operativos compatibles para equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Cambios que realiza el servidor en un equipo cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Información de nombre de usuario y contraseña de red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Cuenta del administrador del servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a>Información general del software del conector  
 El software del Conector para el sistema operativo Windows Server Essentials conecta a los equipos de la red al servidor de Windows Server Essentials. Al conectar equipos al servidor, el software del Conector le permite hacer automáticamente una copia de seguridad de los equipos y supervisar su mantenimiento. El software del Conector también permite configurar y administrar de forma remota el servidor de Windows Server Essentials. El software del Conector se instala al conectar un equipo cliente al servidor. Para instrucciones sobre cómo conectar equipos cliente al servidor de Windows Server Essentials, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) más adelante en este tema.  

-   [Información general del software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Requisitos previos para conectar un equipo al servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Requisitos previos para conectar un equipo Mac a la red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Sistemas operativos compatibles para equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Cambios que realiza el servidor en un equipo cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Información de nombre de usuario y contraseña de red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Cuenta del administrador del servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a>Información general del software del conector  
 El software del Conector para el sistema operativo Windows Server Essentials conecta a los equipos de la red al servidor de Windows Server Essentials. Al conectar equipos al servidor, el software del Conector le permite hacer automáticamente una copia de seguridad de los equipos y supervisar su mantenimiento. El software del Conector también permite configurar y administrar de forma remota el servidor de Windows Server Essentials. El software del Conector se instala al conectar un equipo cliente al servidor. Para instrucciones sobre cómo conectar equipos cliente al servidor de Windows Server Essentials, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) más adelante en este tema.  


###  <a name="BKMK_2"></a>Requisitos previos para conectar un equipo al servidor  
 Antes de conectar un equipo a la red, deben cumplirse los siguientes requisitos:  

-   La instalación de Windows Server Essentials se ha completado y el servidor se está ejecutando. El software del Conector finalizará la instalación si no puede comunicarse con el servidor.  


-   El equipo cliente usa un sistema operativo compatible. Para más información, vea [Sistemas operativos compatibles para los equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).


-   El equipo cliente debe tener una conexión a Internet válida.  

-   El equipo cliente está en la misma subred IP que el servidor que ejecuta Windows Server Essentials cuando el equipo cliente está en la misma red que el servidor.  

-   El equipo cliente tiene instalado .NET Framework 4.5. El software del Conector lo instala automáticamente en el equipo.  

-   El equipo cliente cumple los siguientes requisitos mínimos del sistema:  

    -   Procesador de 1,4 GHz o superior  

    -   1 GB de RAM o más  

    -   1 GB de espacio disponible en disco (se liberará una parte del disco después de la instalación)  

-   La partición de arranque (es decir, la partición del disco donde está instalado el sistema operativo Windows) está formateada con el sistema de archivos NTFS.  

-   El nombre del equipo no incluye más de 15 caracteres.  

-   El nombre del equipo no incluye un carácter de subrayado (_).  

-   La configuración de fecha y hora del equipo se ajusta a la configuración del servidor.  

-   Un equipo cliente puede estar conectado a un solo servidor de Windows Server Essentials en cualquier momento.  

-   Un equipo cliente que ya se ha unido a otro dominio de Active Directory no puede unirse a un dominio de Windows Server Essentials.  

> [!NOTE]
> 
>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos al servidor sin agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos cliente compatibles, y características como la directiva de grupo y las redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para requisitos e instrucciones, vea [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Para instrucciones detalladas sobre cómo conectar un equipo al servidor que ejecuta Windows Server Essentials, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos al servidor sin agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos cliente compatibles, y características como la directiva de grupo y las redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para requisitos e instrucciones, vea [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Para instrucciones detalladas sobre cómo conectar un equipo al servidor que ejecuta Windows Server Essentials, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


###  <a name="BKMK_3"></a>Requisitos previos para conectar un equipo Mac a la red  
 Antes de conectar un equipo Mac a la red, deben cumplirse los siguientes requisitos:  

-   La instalación del sistema operativo del servidor se ha completado y el servidor se está ejecutando. El software del Conector no se instalará si no se puede comunicar con el servidor.  

-   El equipo usa Mac OS X 10.5 (Leopard) o una versión posterior.  

-   El equipo está en la misma subred IP que el servidor.  

-   El equipo debe tener una conexión a Internet válida.  

-   Asegúrese de que el equipo cumpla los siguientes requisitos mínimos del sistema:  

    -   Procesador de 1,4 GHz o superior  

    -   1 GB de RAM o más  

    -   1 GB de espacio disponible en disco (se liberará una parte del disco después de la instalación)  

-   Un equipo cliente puede estar conectado a un solo servidor en cualquier momento.  

###  <a name="BKMK_4"></a>Sistemas operativos compatibles para equipos cliente  
 Windows Server Essentials ofrece el mismo conjunto de características para todos los equipos cliente compatibles. Estas características incluyen la unión al dominio, Launchpad y notificaciones de estado del cliente.  

> [!IMPORTANT]
>  Windows Server Essentials no admite la unión al dominio de equipos que usan las versiones Home, Starter o Media Center de Windows. Además, no puede usar el acceso web remoto para conectarse a estos equipos.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Esta sección se aplica a un servidor que ejecuta Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de experiencia con Windows Server Essentials instalado. Se admiten los siguientes sistemas operativos:  

 **Sistemas operativos Windows 7**  

- Windows 7 Home Basic SP1 (x86 y x64)  

- Windows 7 Home Premium SP1 (x86 y x64)  

- Windows 7 Professional SP1 (x86 y x64)  

- Windows 7 Ultimate SP1 (x86 y x64)  

- Windows 7 Enterprise SP1 (x86 y x64)  

- Windows 7 Starter SP1 (x86)  

  **Sistemas operativos Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Enterprise  

  **Sistemas operativos Windows 8.1**  

- Windows 8.1  

- Windows 8.1 Pro  

- Windows 8,1 Enterprise  

  **Sistemas operativos Windows 10**  

- Windows 10  

- Windows 10 Pro  

- Windows 10 Enterprise  

- Windows 10 Education  

  **Equipos cliente Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

- Mac OS X v10.8 Mountain Lion  

> [!NOTE]
>  Puede ver el estado de mantenimiento y copia de seguridad de un equipo Mac desde el panel de Windows Server Essentials. Sin embargo, no se puede configurar la copia de seguridad del equipo ni iniciar una copia de seguridad desde el panel. Además, tampoco puede usar el acceso web remoto para conectarse a un equipo Mac.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Esta sección se aplica a un servidor que ejecuta Windows Server Essentials. Se admiten los siguientes sistemas operativos:  

 **Sistemas operativos Windows 7**  

- Windows 7 Home Basic (x86 y x64)  

- Windows 7 Home Premium (x86 y x64)  

- Windows 7 Professional (x86 y x64)  

- Windows 7 Ultimate (x86 y x64)  

- Windows 7 Enterprise (x86 y x64)  

- Windows 7 Starter (x86)  

  **Sistemas operativos Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Enterprise  

  **Sistemas operativos Windows 10**  

- Windows 10  

- Windows 10 Pro  

- Windows 10 Enterprise  

- Windows 10 Education  

  **Equipos cliente Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

> [!NOTE]
>  Puede ver el estado de mantenimiento y de copia de seguridad de un equipo Mac desde el panel de Windows Server Essentials. Sin embargo, no se puede configurar la copia de seguridad del equipo ni iniciar una copia de seguridad desde el panel. Además, tampoco puede usar el acceso web remoto para conectarse a un equipo Mac.  

###  <a name="BKMK_5"></a>Cambios que realiza el servidor en un equipo cliente  
 Cuando se conecta un equipo al servidor, el software de Windows Server Essentials realiza una serie de cambios en el equipo para que el equipo y el servidor puedan funcionar juntos.  

 El software hace lo siguiente:  

-   Instala el software del Conector en el equipo  

-   Instala Microsoft .NET Framework 4.5 en el equipo si aún no está instalado  

-   Crea accesos directos en el escritorio del equipo al panel y a Launchpad  

-   Configura los puertos del Firewall de Windows en el equipo para permitir que funcionen las siguientes características:  

    -   Redes principales  

    -   Servicios de Escritorio remoto  

-   Realiza los siguientes cambios en el equipo para poder hacer copias de seguridad:  

    -   Crea tareas programadas para ejecutar copias de seguridad automáticas  

    -   Instala servicios que administran las operaciones de copia de seguridad con el servidor  

    -   Instala un controlador de dispositivo virtual que se usa durante los procesos de restauración de archivos y carpetas  

-   Instala el agente de mantenimiento para detectar problemas y crear las notificaciones de alerta correspondientes  

-   Crea tareas programadas en el equipo para evaluaciones periódicas de mantenimiento y para sincronizar las definiciones de alerta de estado  

-   Agrega servicios al equipo que este usa para comunicarse con el servidor y con otras características de Windows Server Essentials  

-   Abre el puerto TCP 3389 en equipos cliente que usan clientes de Windows para permitir la ejecución de los Servicios de Escritorio remoto  

-   Implementa VPN en el equipo cliente y ofrece una experiencia de un solo clic si está habilitada la funcionalidad de VPN en Windows Server Essentials, o proporciona una experiencia de conexión automática si está habilitada la funcionalidad de VPN en Windows Server Essentials.  


 Para información sobre cómo conectar el equipo al servidor, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

###  <a name="BKMK_6"></a>Información de nombre de usuario y contraseña de red  
 Puede obtener la información de nombre de usuario y contraseña de red de la persona que administra el servidor. Puede usar estas credenciales para conectar el equipo al servidor y acceder a información ubicada en él.  

###  <a name="BKMK_6"></a>Información de nombre de usuario y contraseña de red  
 Puede obtener la información de nombre de usuario y contraseña de red de la persona que administra el servidor. Puede usar estas credenciales para conectar el equipo al servidor y acceder a información ubicada en él. 


 Si es el administrador del servidor, puede crear las credenciales de red agregando una cuenta de usuario desde la pestaña **Usuarios** del panel. Para más información sobre las cuentas de usuario, vea [Administrar cuentas de usuario mediante el panel](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  

###  <a name="BKMK_7"></a>Cuenta del administrador del servidor  
 Debe poder dar un nombre de cuenta de administrador de red y una contraseña para instalar el software del Conector. Una cuenta de administrador de red permite al usuario administrar la red de área local para su organización y le ayuda a administrar y mantener los dispositivos de red, como los conmutadores y enrutadores.  

 Entre las tareas que se pueden realizar mediante una cuenta de administrador de red se incluyen:  

- Instalación de aplicaciones en red y otro software  

- Administración de almacenamiento en el servidor  

- Distribución de actualizaciones de software  

- Realización de copias de seguridad periódicas  

- Supervisión de las actividades diarias en la red  

  En Windows Server Essentials, Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, puede asignar el nivel de acceso de administrador de red a cualquier cuenta de usuario. Esto otorga los permisos necesarios para realizar tareas de administrador de red. Cuando a un usuario se le asigna el nivel de acceso de administrador de red, se abre el mensaje del sistema **Control de cuentas de usuario** para cualquier tarea que requiera permisos de administrador.  

###  <a name="BKMK_8"></a>Quitar un equipo de un dominio de Windows  
 Para quitar un equipo de su dominio, se le pedirá el nombre de usuario y la contraseña de la cuenta de dominio.  

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Para quitar un equipo de un dominio de Windows  

1.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo** y después haga clic en **Propiedades**.  

2.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**.  

    > [!NOTE]
    >  Si se le pide una contraseña de administrador o una confirmación, escriba la contraseña del dominio o dé su confirmación.  

3.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en la pestaña **Nombre de equipo** y después haga clic en **Cambiar**.  

4.  En el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** , en **Miembro de**, haga clic en **Grupo de trabajo**y después elija una de las siguientes opciones:  

    1.  Para unirse a un grupo de trabajo existente, escriba el nombre del grupo de trabajo al que quiere unirse y después haga clic en **Aceptar**.  

    2.  Para crear un grupo de trabajo, escriba el nombre del grupo de trabajo que quiere crear y después haga clic en **Aceptar**.  

        > [!NOTE]
        >  El equipo se quitará del dominio y la cuenta de equipo de ese dominio se deshabilitará.  

##  <a name="BKMK_B"></a>Conectar equipos al servidor mediante el software del conector  
 En esta sección se da acceso a procedimientos e información que le ayudarán a instalar el software del Conector, conectar el equipo al servidor y solucionar problemas que surgen al conectar equipos al servidor.  


-   [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Instalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Traslado manual de los datos y la configuración del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Transferir varios perfiles de usuario durante la implementación del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Desconectar el equipo o volver a conectar el equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Cómo funciona la copia de seguridad con los modos de suspensión e hibernación](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Instalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Traslado manual de los datos y la configuración del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Transferir varios perfiles de usuario durante la implementación del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Desconectar el equipo o volver a conectar el equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Cómo funciona la copia de seguridad con los modos de suspensión e hibernación](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  


###  <a name="BKMK_9"></a>Conectar equipos al servidor  
 Cuando conecte un equipo a un servidor que ejecute Windows Server Essentials o Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, asegúrese de que el equipo cliente tenga una conexión válida a Internet.  

 Realice el procedimiento siguiente en todos los equipos cliente para conectarlos al servidor.  

 Para completar el procedimiento, necesitará la siguiente información:  

-   El nombre de usuario y la contraseña de la persona que va a usar el equipo en la nueva red  

-   El nombre de usuario y la contraseña de la cuenta de administrador local del equipo  

> [!NOTE]
> 
>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos al servidor sin agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos cliente compatibles, y características como la directiva de grupo y las redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para requisitos e instrucciones, vea [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
> 
>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos al servidor sin agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos cliente compatibles, y características como la directiva de grupo y las redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para requisitos e instrucciones, vea [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  


##### <a name="to-connect-a-client-computer-to-the-server"></a>Para conectar un equipo cliente al servidor  

1.  Inicie sesión en el equipo que desea conectar al servidor.  

    > [!NOTE]
    >  Si el equipo tiene varias cuentas de usuario, inicie sesión en aquella en la que se encuentren los documentos, imágenes y preferencias personales que quiere mantener después de conectar el equipo al servidor.  

2.  Abra un explorador de Internet, como, por ejemplo, Internet Explorer.  

3.  En la barra de direcciones, escriba **http://< servername\>/Connect**y, a continuación, presione Entrar.  

    > [!NOTE]
    >  Si el equipo está en una ubicación remota fuera de la red de Windows Server Essentials, para ejecutar el Asistente para conectar un equipo al servidor, escriba **http://< nombrededominio\>/Connect** en la barra de direcciones del explorador Web (donde < dominio\> es el nombre de dominio de la organización). El administrador de red puede proporcionarle la información del nombre de dominio.  

4.  Aparecerá la página **Conecte el equipo al servidor** . Realice una de las siguientes acciones:  

    -   En un equipo con el sistema operativo Windows, haga clic en **Descargar software para Windows**.  

    -   Si el equipo usa Mac OS X, o una versión posterior, haga clic en **Descargar software para Mac**.  

5.  Cuando aparezca el mensaje de advertencia de seguridad de descarga de archivos, haga clic en **Ejecutar**.  

6.  Si aparece el mensaje Control de cuentas de usuario, haga clic en **Sí** o escriba el nombre de usuario local y la contraseñas, si se lo pregunta.  

7.  Aparecerá el asistente para conectar un equipo al servidor. Realice los pasos siguientes con el asistente:  

    1.  Acepte el contrato de licencia de usuario final.  

    2.  En la página **Buscar mi servidor**, detecte automáticamente el servidor en las redes locales y seleccione el servidor al que quiere conectarse. O bien, si tiene la información, puede introducir manualmente el nombre del servidor o la dirección de dominio.  

    3.  En la página **Escriba su nuevo nombre de usuario y contraseña de red** , realice el procedimiento siguiente:  

        -   Si se trata del primer equipo que conecta al servidor y es el que va a usar para administrarlo, utilice la cuenta de administrador que creó durante la instalación.  

        -   Para todos los demás equipos, cree primero una cuenta de usuario de red en el servidor utilizando el panel. Cree la cuenta de usuario con privilegios de Administrador o Usuario estándar, según las tareas que vaya a realizar la persona que use el equipo.  

    4.  Si el equipo ejecuta Windows 8, Windows 8.1 o Windows 10, omita este paso. Si su equipo ejecuta Windows 7 y tiene documentos, imágenes o preferencias personales (por ejemplo, fondos de escritorio, protectores de pantalla o favoritos de Internet Explorer) que desea conservar después de unir el equipo a la nueva red, en la página **Elija si desea mover los datos y la configuración existentes** del asistente, seleccione la opción **Mover mis datos y configuración a la nueva cuenta de usuario de red**.  

    5.  Elija si desea reactivar este equipo automáticamente para crear una copia de seguridad en la página **Seleccione si desea reactivar el equipo para crear su copia de seguridad** .  

    6.  Siga las demás instrucciones del asistente para unir el equipo a la red.  

8.  Después de unir el equipo a la red, use el nombre de usuario y la contraseña nuevos para iniciar sesión en el equipo.  

    > [!NOTE]
    >  Cuando inicie sesión en un equipo que ejecute Windows 8 por primera vez con la cuenta de red después de conectarse al servidor, se le proporcionarán instrucciones para migrar los archivos y las aplicaciones desde la cuenta de usuario anterior. Siga las instrucciones de la página **¿Cómo migrar archivos y aplicaciones desde mi cuenta de usuario anterior?** para migrar todos los archivos y aplicaciones a la cuenta de usuario de red.  

9. Una vez que el equipo se ha conectado correctamente al servidor, aparecen accesos directos al bandeja del conector y al panel del servidor en el menú Inicio, que se puede usar de la siguiente manera (si el equipo ejecuta Windows 8, Windows 8.1 o Windows 10, el panel y el conector Bandeja estará disponible en la pantalla Inicio del equipo):  

    -   Si el equipo ejecuta Windows 8, Windows 8.1 o Windows 10, el panel y el conector bandeja se podrán buscar como una aplicación.  

    -   Desde la aplicación de bandeja del Conector, puede habilitar o deshabilitar la característica **Mantenerme conectado de manera remota**. También puede hacer doble clic en la aplicación de bandeja para iniciar Launchpad. Desde Launchpad puede acceder al acceso directo de Carpetas compartidas, configurar copias de seguridad del equipo, abordar alertas y abrir el sitio del acceso web remoto.  

    -   Desde el vínculo del **Panel**, puede administrar el servidor.  

###  <a name="BKMK_10"></a>Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio  
 En este tema se describe cómo agregar un equipo con Windows 7, Windows 8, Windows 8.1 o Windows 10 a una red de Windows Server Essentials sin unir el equipo al dominio de Windows Server Essentials en una implementación de cliente local. Este método de conexión se admite en Windows Server Essentials y Windows Server Essentials.  

 Esto es una alternativa al método habitual, que requiere unir el equipo al dominio de Windows Server Essentials. Con ese método, si el equipo está en otro dominio, debe quitarse del dominio antes de que pueda agregarse al dominio de Windows Server Essentials.  

#### <a name="feature-limits"></a>Límites de característica  
 Algunas características están limitadas cuando un equipo cliente no se agrega al dominio de Windows Server Essentials:  

-   No están disponibles todas las características que requieren que el equipo esté unido al dominio, incluidas las credenciales de dominio, directiva de grupo y redes privadas virtuales (VPN).  

-   Los complementos de terceros que requieran que el equipo se una al dominio no funcionarán correctamente.  

-   Este método no puede usarse para conectar un equipo remoto al servidor.  

#### <a name="prerequisites"></a>Requisitos previos  

-   El equipo debe tener una conexión física a la red local.  

-   El equipo debe tener instalado uno de los siguientes sistemas operativos:  

    -   Windows 10 Pro, Windows 10 Enterprise  

    -    Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 8 Pro, Windows 8 Enterprise  

    -    Windows 7 Professional (x86 y x64), Windows 7 Enterprise (x86 y x64), Windows 7 Ultimate (x86 y x64)  


-   El equipo debe cumplir el resto de requisitos para los equipos cliente de Windows Server Essentials. Para más información, vea [Requisitos previos para conectar un equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  


-   Para habilitar una conexión sin unirse al dominio, debe iniciar sesión en el equipo con una cuenta que sea miembro del grupo Administradores local.  

-   Para conectar el equipo al servidor de Windows Server Essentials, necesitará la siguiente información de cuenta:  

    -   Una cuenta con derechos de administrador en Windows Server Essentials (su cuenta).  

    -   El nombre de usuario y la contraseña para la cuenta de dominio de la persona que va a usar el equipo. La cuenta de dominio debe tener derechos de administrador en el servidor de Windows Server Essentials.  

#### <a name="connect-to-a-windows-server-essentials-network"></a>Conectarse a una red de Windows Server Essentials  
 Después de comprobar que se cumplan todos los requisitos previos, conecte el equipo a la red de Windows Server Essentials.  

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Para conectar un equipo de un dominio diferente a una red de Windows Server Essentials  

1.  Inicie sesión el equipo cliente con una cuenta que sea miembro del grupo Administradores local.  

2.  Abra un símbolo del sistema con derechos de administrador.  

    -   En Windows 10, haga clic en el botón **Inicio** , seleccione **todas las aplicaciones** -> **herramientas del sistema de Windows** -> **símbolo**del sistema, haga clic con el botón secundario en símbolo del sistema y, a continuación, haga clic en **Ejecutar como administrador**.  

    -   En Windows 8, en la página **Inicio** , escriba **comando** y, a continuación, presione Entrar. En los resultados, haga clic con el botón secundario en **Símbolo del sistema** y después haga clic en **Ejecutar como administrador**.  

    -   En Windows 7, en el menú **Inicio** , escriba **Command** en el cuadro de búsqueda, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**.  

3.  En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  


4.  Complete los pasos de [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


####  <a name="BKMK_SecondServer"></a>Unir un segundo servidor a la red  

###### <a name="to-join-a-second-server-to-the-network"></a>Para unir un segundo servidor a la red  

1.  Inicie sesión en el servidor que quiere conectar a la red de Windows Server Essentials.  

2.  Abra un explorador de Internet y, en la barra de direcciones, escriba **http://< servername\>/Connect**, donde *< ServerName\>* es el nombre del servidor que ejecuta Windows Server Essentials y, a continuación, presione Entrar.  

3.  Si la configuración de seguridad mejorada de Internet Explorer está habilitada en el servidor que intenta conectar a la red de Windows Server Essentials, haga lo siguiente; de lo contrario, omita este paso.  

    1.  Para aceptar el mensaje de bloqueo, haga clic en **Cerrar**.  

    2.  Agregue el sitio web de **http://< servername\>/Connect** a los sitios web de confianza de la siguiente manera:  

        1.  En el panel de navegación del explorador, haga clic en **Herramientas** y después, en **Opciones de Internet**.  

        2.  Haga clic en la pestaña **Seguridad** y después, en **Sitios de confianza**.  

        3.  Haga clic en **Sitios**.  

        4.  El sitio web debería aparecer en el campo **Agregar este sitio web a la zona de:** . Haga clic en **Agregar**.  

        5.  Haga clic en **Cerrar**y después, en **Aceptar**.  

    3.  Actualice la página web.  


    4.  Para conectar el segundo servidor a un servidor que ejecuta Windows Server Essentials, siga las instrucciones de [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:  
>   
>  -   There is no user profile migration; therefore, the current profile will not be migrated.  
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.  
~~~

 Después de unir el segundo servidor a un servidor que ejecuta Windows Server Essentials, se dan las siguientes características en el servidor conectado:  

- Las características de actualización y de estado de alertas funcionan igual que en otros equipos cliente.  

- Las características en línea y sin conexión funcionan igual que en otros equipos cliente.  

- Puede conectarse al segundo servidor mediante el acceso web remoto.  

- El segundo servidor se incluye en los informes de mantenimiento porque Windows Server Essentials genera alertas relacionadas con este servidor.  

  La administración del segundo servidor desde el servidor que ejecuta Windows Server Essentials difiere de la administración de otros equipos cliente en lo siguiente:  

- No hay ningún punto de entrada para la aplicación de bandeja, Launchpad y el cliente de escritorio.  

- El segundo servidor aparece dentro del grupo **Servidores** de la pestaña **Dispositivos**.  

- Como no se admite la copia de seguridad del equipo cliente para el segundo servidor, el estado de la copia de seguridad se muestra como **No compatible**. Además, si selecciona el segundo servidor y hace clic con el botón secundario, no se muestra ninguna tarea relacionada con copias de seguridad y restauración para el segundo servidor.  

- Si selecciona el segundo servidor y, a continuación, hace clic en la tarea **ver las propiedades del servidor** , no se muestra ninguna pestaña **copia de seguridad** en la página de propiedades del servidor.  

- Dado que no hay ningún Security Center en un sistema operativo Windows Server, el estado de seguridad del segundo servidor se muestra como **no aplicable**.  

- El estado directiva de grupo del segundo servidor se muestra como **no aplicable**.  

###  <a name="BKMK_11"></a>Instalar el software del conector  
 El software del Conector de Windows Server Essentials se instala cuando conecta un equipo al servidor mediante el asistente para conectar un equipo al servidor. Puede iniciar este asistente escribiendo **http://< servername\>/Connect** en la barra de direcciones del explorador Web (donde *< ServerName\>* es el nombre del servidor).  

> [!NOTE]
>  Si el equipo está en una ubicación remota, escriba **http://< nombredominio\>/Connect** en la barra de direcciones del explorador Web (donde *< dominio\>* es el nombre de dominio de su organización) para ejecutar el Asistente para conectar un equipo al servidor. El administrador de red puede proporcionarle la información del nombre de dominio.  

 El software del Conector hace lo siguiente:  

-   Conecta el equipo a Windows Server Essentials  

-   Hace una copia de seguridad automática del equipo cada noche (si se configura el servidor para crear copias de seguridad de clientes)  

-   Ayuda al administrador a supervisar el estado del equipo  

-   Le permite configurar y administrar de forma remota Windows Server Essentials desde su equipo doméstico  


 Para instrucciones detalladas sobre cómo conectar un equipo al servidor de Windows Server Essentials, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   


###  <a name="BKMK_12"></a>Traslado manual de los datos y la configuración del equipo  
  Windows Server Essentials y Windows Server Essentials solo admiten la migración de perfiles de usuario para equipos cliente que ejecutan el sistema operativo Windows 7. Cuando se conecta un equipo basado en Windows 7 al servidor, el asistente para conectar un equipo al servidor puede migrar automáticamente el perfil de usuario.  

 El perfil de usuario no se puede transferir automáticamente al conectar un equipo con Windows 8, Windows 8.1 o Windows 10 al servidor. Sin embargo, en un equipo con Windows 8, puede usar Windows Easy Transfer para transferir los datos y la configuración del usuario local original en el equipo unido al dominio. Para hacerlo, debe ser un administrador del equipo de origen y de destino con Windows 8. Para obtener información sobre cómo usar Windows Easy Transfer para transferir archivos y opciones de configuración, consulte el [artículo 2735227](https://support.microsoft.com/kb/2735227) en Microsoft Knowledge Base.  

###  <a name="BKMK_Transfer"></a>Transferir varios perfiles de usuario durante la implementación del equipo  
 Antes de conectar un equipo que usa el sistema operativo Windows 7 o Windows 7 SP1 al servidor de Windows Server Essentials, primero debe crear las cuentas de usuario de red correspondientes en el servidor para transferir varios perfiles de usuario local. Para más información sobre cómo crear cuentas de usuario de red, vea [Agregar una cuenta de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  

 La migración de perfiles de usuario solo se admite en un equipo que ejecute Windows 7 (para Windows Server Essentials) o Windows 7 SP1 (para Windows Server Essentials). Cuando conecta un equipo al servidor de Windows Server Essentials mediante el asistente para conectar un equipo al servidor, se le ofrece la opción de mover los datos y la configuración de usuario de las cuentas de usuario locales anteriores a las nuevas cuentas de usuario de red. Para hacerlo, en la página **Mover datos de usuario y configuración existentes** del asistente, asigne las cuentas de usuario de red a las cuentas de usuario local que existen en el equipo para transferir varios perfiles de usuario que se encuentran en el equipo cliente.  

###  <a name="BKMK_13"></a>Desinstalar el software del conector  
 Puede desinstalar el software del Conector desde un equipo mediante el Panel de control. Normalmente se hace esto si hay un problema con el software del Conector o si necesita instalar una versión más reciente del software del Conector. Debe haber iniciado sesión en el equipo como administrador para completar este procedimiento.  

> [!IMPORTANT]
>  Si actualiza el sistema operativo en un equipo cliente, el software del Conector se desinstala automáticamente. Debe reinstalar el software del Conector una vez completada la actualización. El método preferido es desinstalar el software del Conector antes de actualizar el sistema operativo. Aun así, es aceptable desinstalar el software del Conector una vez completada la actualización, pero esto puede dar como resultado un estado incoherente del equipo cliente con el servidor hasta que se haya desinstalado y reinstalado el software del Conector.  

##### <a name="to-uninstall-connector-software-from-a-computer"></a>Para desinstalar el software del Conector de un equipo  

1.  En un equipo que ejecute Windows 7, Windows 8, Windows 8.1 o Windows 10, abra el **Panel de control**y, a continuación, en la sección **programas** , haga clic en **ver actualizaciones instaladas**.  

2.  En la lista de programas instalados, seleccione **Conector de Windows Server Essentials**y después haga clic en **Desinstalar**.  

3.  En la página de advertencia, haga clic en **Sí**.  

4.  Si aparece la ventana **Control de cuentas de usuario** , haga clic en **Permitir**.  

5.  En Windows Server Essentials, si aparece la página del conector de Windows Server Essentials que sugiere cerrar el Launchpad, haga clic en **Aceptar**.  

6.  Espere a que el programa se desinstale. Después de quitar el software, **Conector de Windows Server Essentials** ya no aparece en la lista de programas o actualizaciones instalados. Además, los accesos directos al Launchpad y al panel ya no se muestran en el escritorio del equipo.  

> [!NOTE]
> - Al desinstalar el software del Conector, no se quita el equipo de la lista de equipos que se muestran en la pestaña **DISPOSITIVOS** del panel. Para quitar el equipo desde el panel, vea [Quitar un equipo del servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
>   -   Al desinstalar el software del Conector, no se eliminan las carpetas compartidas del equipo cliente que se habían asignado al servidor. Debe eliminar manualmente las carpetas compartidas que se asignan al servidor.  
> 
> -   Al desinstalar el software del Conector, no se separa el equipo del dominio original. El equipo debe separarse manualmente del dominio. Para instrucciones, vea [Quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  


###  <a name="BKMK_14"></a>Desconectar el equipo o volver a conectar el equipo al servidor  
 Para desconectar un equipo desde el servidor, debe completar los siguientes pasos:  


1. Desinstale el software del Conector desde el equipo mediante el Panel de control. Para instrucciones detalladas, vea [Desinstalar el software del Conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   


2. Separe el equipo del dominio de Windows Server Essentials y únalo al grupo de trabajo. Para obtener instrucciones detalladas sobre cómo unir Windows a un grupo de trabajo, consulte [Crear un grupo de trabajo o unirse a él](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

3. Quitar el equipo del servidor mediante el panel. Para instrucciones detalladas, vea [Quitar un equipo del servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  

   Para volver a conectar un equipo al servidor que previamente se ha desconectado de la red del servidor de Windows Server Essentials, debe completar los siguientes pasos:  


4. Desinstale el software del Conector desde el equipo mediante el Panel de control. Para instrucciones detalladas, vea [Desinstalar el software del Conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  

5. Separe el equipo del dominio de Windows Server Essentials y únalo al grupo de trabajo. Para obtener instrucciones detalladas sobre cómo unir Windows a un grupo de trabajo, consulte [Crear un grupo de trabajo o unirse a él](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

6. Conecte el equipo al servidor mediante el Asistente para conectar equipos. Para instrucciones detalladas, vea [Conectar equipos al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

###  <a name="BKMK_Sleep"></a>Cómo funciona la copia de seguridad con los modos de suspensión e hibernación  
 Si selecciona la opción **Reactivar este equipo para copia de seguridad** al conectar un equipo al servidor, el equipo se reactiva automáticamente desde el modo de suspensión o hibernación a diario tal como se especifica en la programación de copia de seguridad para que se puedan hacer copias de seguridad. Una vez finalizada la copia de seguridad, el equipo vuelve al modo de suspensión o hibernación, en función de la configuración de administración de energía. Si no se selecciona esta opción, el servidor no hace ninguna copia de seguridad de un equipo si el equipo está en modo de suspensión o hibernación. Para obtener más información, vea [Manage Client backup](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  

##  <a name="BKMK_C"></a>Uso de Launchpad  
 Puede usar Launchpad para tener acceso a los recursos compartidos del servidor de Windows Server Essentials, hacer copias de seguridad del equipo y responder a las alertas de mantenimiento del sistema.  

-   [Información general sobre LaunchPad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  

-   [Uso de Launchpad con un equipo Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  

## <a name="see-also"></a>Vea también  

-   [Solucionar problemas de conexión de equipos al servidor](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  

-   [Administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  


-   [Usar carpetas compartidas](Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Reproducir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Usar carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

