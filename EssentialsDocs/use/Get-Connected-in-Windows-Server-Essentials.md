---
title: Conectarse en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 1d7f3c33f1254c8dbe4af8bdf5baa4144c134248
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="get-connected-in-windows-server-essentials"></a>Conectarse en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Puedes conectar los equipos en el servidor de Windows Server Essentials utilizando el software del conector. Cuando se conecta un equipo en el servidor mediante la conectar un equipo para el Asistente de servidor, se instala el software del conector. Puede iniciar este asistente escribiendo **http://<servername\>/connect**, donde **< servername\ >** es el nombre del servidor.  
  
 En este tema:  
  

-   [Preparar conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Conectar equipos al servidor mediante el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Uso del Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Preparar conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Conectar equipos al servidor mediante el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Uso del Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

  
##  <a name="BKMK_A"></a>Preparar conectar equipos con el servidor  
 Esta sección describe el software del conector, los sistemas operativos que son compatibles con Windows Server Essentials, las tareas de requisitos previos que deben completarse antes de conectar los equipos servidor y los cambios que realiza el servidor en los equipos cuando se ejecuta el software del conector.  
  

-   [Información general de software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Requisitos previos para conectar un equipo con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Requisitos previos para conectar un equipo Mac a la red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemas operativos compatibles para los equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Cambios que realiza el servidor en un equipo cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Información de nombre y la contraseña de usuario de red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Cuenta de administrador s de servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Información general de software del conector  
 El software del conector para el sistema operativo de Windows Server Essentials conecta a los equipos de la red al servidor de Windows Server Essentials. Cuando se conectan a equipos con el servidor, el software del conector permite automáticamente copias de seguridad de los equipos y supervisar su estado. El software del conector también te permite configurar y administrar de forma remota el servidor de Windows Server Essentials. El software del conector está instalado al conectar un equipo cliente al servidor. Para obtener instrucciones sobre cómo conectar equipos cliente al servidor de Windows Server Essentials, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) más adelante en este tema.  

-   [Información general de software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Requisitos previos para conectar un equipo con el servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Requisitos previos para conectar un equipo Mac a la red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemas operativos compatibles para los equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Cambios que realiza el servidor en un equipo cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Información de nombre y la contraseña de usuario de red](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Cuenta de administrador s de servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Información general de software del conector  
 El software del conector para el sistema operativo de Windows Server Essentials conecta a los equipos de la red al servidor de Windows Server Essentials. Cuando se conectan a equipos con el servidor, el software del conector permite automáticamente copias de seguridad de los equipos y supervisar su estado. El software del conector también te permite configurar y administrar de forma remota el servidor de Windows Server Essentials. El software del conector está instalado al conectar un equipo cliente al servidor. Para obtener instrucciones sobre cómo conectar equipos cliente al servidor de Windows Server Essentials, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) más adelante en este tema.  

  
###  <a name="BKMK_2"></a>Requisitos previos para conectar un equipo con el servidor  
 Antes de conectar un equipo a la red, deben cumplirse los siguientes requisitos:  
  
-   La instalación de Windows Server Essentials completa, y se ejecuta el servidor. El software del conector cerrará la instalación, si no puede comunicarse con el servidor.  
  

-   El equipo cliente ejecuta un sistema operativo compatible. Para obtener más información, consulta [sistemas operativos admitidos para los equipos cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).

  
-   El equipo cliente debe tener una conexión válida a Internet.  
  
-   El equipo cliente está en la misma subred IP que el servidor que ejecuta Windows Server Essentials cuando el equipo cliente está en la misma red que el servidor.  
  
-   El equipo cliente tiene instalado .NET Framework 4.5. El software del conector automáticamente la instala en el equipo.  
  
-   El equipo cliente cumple los siguientes requisitos mínimos del sistema:  
  
    -   1,4 GHz o un procesador más rápido  
  
    -   1 GB de RAM o más  
  
    -   1 GB de espacio en disco duro (una parte de este espacio se liberará tras la instalación)  
  
-   La partición de arranque (es decir, la partición de disco donde está instalado el sistema operativo Windows) está formateada con el sistema de archivos NTFS.  
  
-   El nombre del equipo no incluye más de 15 caracteres.  
  
-   El nombre del equipo no incluye un carácter de subrayado (_).  
  
-   La configuración de fecha y hora del equipo s alinea a la configuración en el servidor.  
  
-   Un equipo cliente puede estar conectado a un único servidor de Windows Server Essentials en cualquier momento.  
  
-   Un equipo cliente que ya esté unido a otro dominio de Active Directory no puede unirse a un dominio de Windows Server Essentials.  
  
> [!NOTE]

>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos en el servidor sin necesidad de agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos de cliente compatibles y características como la directiva de grupo y redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para los requisitos e instrucciones, consulta [conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Para que obtener instrucciones paso a paso conectar un equipo al servidor que ejecuta Windows Server Essentials, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos en el servidor sin necesidad de agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos de cliente compatibles y características como la directiva de grupo y redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para los requisitos e instrucciones, consulta [conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Para que obtener instrucciones paso a paso conectar un equipo al servidor que ejecuta Windows Server Essentials, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
###  <a name="BKMK_3"></a>Requisitos previos para conectar un equipo Mac a la red  
 Antes de conectar un equipo Mac a la red, deben cumplirse los siguientes requisitos:  
  
-   La instalación del sistema operativo de servidor se completa, y se ejecuta el servidor. No se instalará el software del conector si no puede comunicarse con el servidor.  
  
-   El equipo está ejecutando Mac OS X 10.5 (Leopard) o posterior.  
  
-   El equipo está en la misma subred IP que el servidor.  
  
-   El equipo debe tener una conexión válida a Internet.  
  
-   Asegúrate de que el equipo cumple los siguientes requisitos mínimos del sistema:  
  
    -   1,4 GHz o un procesador más rápido  
  
    -   1 GB de RAM o más  
  
    -   1 GB de espacio en disco duro (una parte de este espacio se liberará tras la instalación)  
  
-   Un equipo cliente puede estar conectado a un solo servidor en un momento determinado.  
  
###  <a name="BKMK_4"></a>Sistemas operativos compatibles para los equipos cliente  
 Windows Server Essentials proporciona el mismo conjunto de características para todos los equipos de cliente compatibles. Estas características incluyen unirse a un dominio, Launchpad y notificaciones de estado de cliente.  
  
> [!IMPORTANT]
>  Windows Server Essentials no admite la unión ambos equipos que se ejecutan las versiones de Windows principal, Starter o Media Center al dominio. Además, no puedes usar acceso Web remoto para conectarse a estos equipos.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  En esta sección se aplica a un servidor que ejecuta Windows Server Essentials, o a un servidor que ejecuta Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de Windows Server Essentials Experience instalado. Se admiten los siguientes sistemas operativos de equipo:  
  
 **Sistemas operativos Windows 7**  
  
-    Windows 7 Home SP1 básica (x 86 y x64)  
  
-    Windows 7 Home Premium SP1 (x 86 y x64)  
  
-    Windows 7 Professional SP1 (x 86 y x64)  
  
-    Windows 7 Ultimate SP1 (x 86 y x64)  
  
-    Windows 7 SP1 de Enterprise (x 86 y x64)  
  
-    Windows 7 Starter SP1 (x 86)  
  
 **Sistemas operativos de Windows 8**  
  
-   Windows 8  
  
-   Windows 8 Professional  
  
-   Windows 8 Enterprise  
  
 **Sistemas operativos de Windows 8.1**  
  
-   Windows 8.1  
  
-   Windows 8.1 Professional  
  
-   Windows 8.1 Enterprise  
  
 **Sistemas operativos de Windows 10**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Equipos cliente Mac**  
  
-   Mac OS X v10.5 Leopard  
  
-   V10.6 de Mac OS X Snow Leopard  
  
-   León de Mac OS X v10.7  
  
-   León de Mac OS X v10.8 montaña  
  
> [!NOTE]
>  Puedes ver el estado y el estado de copia de seguridad de un equipo Mac desde el escritorio de Windows Server Essentials. Sin embargo, no puede configurar copia de seguridad del equipo o iniciar una copia de seguridad desde el panel. Además, no puedes usar acceso Web remoto para conectar un equipo Mac.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  En esta sección se aplica a un servidor que ejecuta Windows Server Essentials. Se admiten los siguientes sistemas operativos de equipo:  
  
 **Sistemas operativos Windows 7**  
  
-    Windows 7 Home Basic (x86 y x64)  
  
-    Windows 7 Home Premium (x86 y x64)  
  
-    Windows 7 Professional (x86 y x64)  
  
-    Windows 7 Ultimate (x86 y x64)  
  
-    Windows 7 Enterprise (x86 y x64)  
  
-    Windows 7 Starter (x86)  
  
 **Sistemas operativos de Windows 8**  
  
-   Windows 8  
  
-   Windows 8 Professional  
  
-   Windows 8 Enterprise  
  
 **Sistemas operativos de Windows 10**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Equipos cliente Mac**  
  
-   Mac OS X v10.5 Leopard  
  
-   V10.6 de Mac OS X Snow Leopard  
  
-   León de Mac OS X v10.7  
  
> [!NOTE]
>  Puedes ver el estado y el estado de copia de seguridad de un equipo Mac desde el escritorio de Windows Server Essentials. Sin embargo, no puede configurar copia de seguridad del equipo o iniciar una copia de seguridad desde el panel. Además, no puedes usar acceso Web remoto para conectar un equipo Mac.  
  
###  <a name="BKMK_5"></a>Cambios que realiza el servidor en un equipo cliente  
 Cuando un equipo se conecta al servidor, el software de Windows Server Essentials realiza una serie de cambios en el equipo para que el equipo y el servidor pueden trabajar juntas.  
  
 El software hace lo siguiente:  
  
-   Instala el software del conector en el equipo  
  
-   Instala Microsoft .NET Framework 4.5 en el equipo si no está ya instalada  
  
-   Crea accesos directos en el escritorio del equipo s en el panel y del Launchpad  
  
-   Configura los puertos del Firewall de Windows en el equipo para permitir que las siguientes características para que funcione:  
  
    -   Redes principales  
  
    -   Servicios de escritorio remoto  
  
-   Realiza los siguientes cambios en el equipo para facilitar las copias de seguridad:  
  
    -   Crea tareas programadas para ejecutar copias de seguridad automáticas  
  
    -   Instala los servicios que administran las operaciones de copia de seguridad con el servidor  
  
    -   Instala un controlador de dispositivo virtual que se usa durante procesos de restauración de archivos y carpetas  
  
-   Instala el agente de mantenimiento para detectar problemas y para crear las notificaciones de alerta correspondientes  
  
-   Las tareas programadas se crea en el equipo para que las evaluaciones de salud periódicos y para sincronizar las definiciones de alertas de estado  
  
-   Agrega servicios al equipo, que el equipo se usa para comunicarse con el servidor y otras características de Windows Server Essentials  
  
-   Se abre el puerto TCP 3389 en equipos cliente que ejecutan los clientes de Windows para permitir que los servicios de escritorio remoto ejecutar  
  
-   Implementa VPN en el equipo cliente y proporciona una experiencia de solo clic si la funcionalidad de la VPN está habilitada en Windows Server Essentials o proporciona una experiencia auto-connect si está habilitada la funcionalidad VPN en Windows Server Essentials  
  

 Para obtener información acerca de cómo conectar el equipo al servidor, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_6"></a>Información de nombre y la contraseña de usuario de red  
 Puedes obtener la información de nombre y la contraseña de usuario de red de la persona que administra el servidor. Puedes usar estas credenciales para conectar el equipo al servidor y acceder a la información del servidor.  
  
###  <a name="BKMK_6"></a>Información de nombre y la contraseña de usuario de red  
 Puedes obtener la información de nombre y la contraseña de usuario de red de la persona que administra el servidor. Puedes usar estas credenciales para conectar el equipo al servidor y acceder a la información del servidor. 

  
 Si eres el administrador del servidor, puedes crear las credenciales de red agregando una cuenta de usuario de la **usuarios** ficha del panel. Para obtener más información acerca de las cuentas de usuario, consulta [administrar cuentas de usuario mediante el panel](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  
  
###  <a name="BKMK_7"></a>Cuenta de administrador s de servidor  
 Debe poder proporcionar un nombre de cuenta de administrador de red y una contraseña para instalar el software del conector. Una cuenta de administrador de red permite al usuario administrar la red de área local de la organización y ayuda a administrar y mantener los dispositivos de red como conmutadores y enrutadores.  
  
 Las tareas que se pueden realizar mediante una cuenta de administrador de red pueden incluir:  
  
-   Instalar aplicaciones en red y otro software  
  
-   Administrar el almacenamiento en el servidor  
  
-   Distribución de actualizaciones de software  
  
-   Realizar copias de seguridad rutinarias  
  
-   Supervisar las actividades diarias de la red  
  
 En Windows Server Essentials, Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, puedes asignar el Administrador de red acceder a nivel de cualquier cuenta de usuario. Esto concede los permisos necesarios para realizar tareas de administrador de red. Cuando un usuario se asigna el Administrador de red acceder a nivel, el **Control de acceso de usuario** se abre el símbolo del sistema para cualquier tarea que requiere permisos de administrador.  
  
###  <a name="BKMK_8"></a>Quitar un equipo de un dominio de Windows  
 Para quitar un equipo de su dominio, se te pedirá el nombre de usuario y contraseña de la cuenta de dominio.  
  
##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Para quitar un equipo de un dominio de Windows  
  
1.  Haz clic en **inicio**, haz clic en **equipo**y, a continuación, haz clic en **propiedades**.  
  
2.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**.  
  
    > [!NOTE]
    >  Si se te pide una contraseña de administrador o una confirmación, escribe la contraseña de dominio o proporciona la confirmación.  
  
3.  En la **propiedades del sistema** cuadro de diálogo, haz clic en el **nombre de equipo** pestaña y, a continuación, haz clic en **cambio**.  
  
4.  En la **cambios de dominio o el nombre de equipo** cuadro de diálogo **miembro de**, haz clic en **grupo de trabajo**, y, a continuación, realiza una de las siguientes acciones:  
  
    1.  Para unirte a un grupo de trabajo existente, escribe el nombre del grupo de trabajo que quieres unirte a y, a continuación, haz clic en **Aceptar**.  
  
    2.  Para crear un grupo de trabajo, escribe el nombre del grupo de trabajo que quieres crear y, a continuación, haz clic en **Aceptar**.  
  
        > [!NOTE]
        >  El equipo se quitará del dominio y se deshabilitará la cuenta de equipo en ese dominio.  
  
##  <a name="BKMK_B"></a>Conectar equipos al servidor mediante el software del conector  
 Esta sección proporciona acceso a información que te ayudarán a instalar el software del conector, conectar el equipo al servidor y solucionar problemas de equipos que se conectan al servidor y procedimientos.  
  

-   [Conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Instalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Mueve los datos del equipo y la configuración manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transferir varios perfiles de usuario durante la implementación del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Desconecta el equipo desde o volver a conectar el equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [¿Cómo una copia de seguridad funciona con el modo de suspensión e hibernación modos](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Instalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Mueve los datos del equipo y la configuración manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transferir varios perfiles de usuario durante la implementación del equipo](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Desconecta el equipo desde o volver a conectar el equipo al servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [¿Cómo una copia de seguridad funciona con el modo de suspensión e hibernación modos](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

  
###  <a name="BKMK_9"></a>Conectar equipos con el servidor  
 Cuando un equipo se conecta a un servidor que ejecuta Windows Server Essentials o Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, asegúrate de que el equipo cliente tiene una conexión válida a Internet.  
  
 Completa el siguiente procedimiento en todos los equipos cliente conectan al servidor.  
  
 Para realizar el procedimiento, debes tener la siguiente información:  
  
-   El nombre de usuario y contraseña de la persona que se va a usar el equipo en la nueva red  
  
-   El nombre de usuario y contraseña de la cuenta de administrador local del equipo s  
  
> [!NOTE]

>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos en el servidor sin necesidad de agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos de cliente compatibles y características como la directiva de grupo y redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para los requisitos e instrucciones, consulta [conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

>  En una implementación de cliente local para Windows Server Essentials o Windows Server Essentials, puede conectar equipos en el servidor sin necesidad de agregarlos al dominio de Windows Server Essentials. Este método no está disponible para todos los sistemas operativos de cliente compatibles y características como la directiva de grupo y redes privadas virtuales (VPN), que requieren que un equipo esté conectado al dominio, no están disponibles. Para los requisitos e instrucciones, consulta [conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

  
##### <a name="to-connect-a-client-computer-to-the-server"></a>Para conectar un equipo cliente al servidor  
  
1.  Iniciar sesión en el equipo que desea conectarse al servidor.  
  
    > [!NOTE]
    >  Si este equipo tiene varias cuentas de usuario, iniciar sesión con la cuenta de usuario que tiene documentos, imágenes y las preferencias del usuario que quieres mantener después de conectar el equipo al servidor.  
  
2.  Abre un explorador de Internet, como Internet Explorer.  
  
3.  En la barra de direcciones, escriba **http://<servername\>/Connect**, y, a continuación, presione ENTRAR.  
  
    > [!NOTE]
    >  Si el equipo está en una ubicación remota fuera de la red de Windows Server Essentials, para ejecutar un equipo de la conexión al Asistente de servidor, escriba **http://<domainname\>/connect** en la barra de direcciones del explorador web (donde < domain\ > es el nombre de dominio de su organización). Puedes obtener la información del nombre de dominio desde el Administrador de red.  
  
4.  La **conectar el equipo al servidor** aparecerá la página. Realiza una de las siguientes acciones:  
  
    -   Para un equipo que ejecute un sistema operativo de Windows, haz clic en **descargar software para Windows**.  
  
    -   En un equipo que ejecuta Mac OS X o posterior, haz clic en **descargar software para Mac**.  
  
5.  En el mensaje de advertencia de seguridad de descarga de archivo, haz clic en **ejecutar**.  
  
6.  Si aparece el mensaje del Control de cuentas de usuario, haz clic en **Sí** o escribe el nombre de usuario local y la contraseña, si se te solicita.  
  
7.  Aparecerá un equipo para el Asistente de servidor de conectar. Haz lo siguiente para completar al asistente:  
  
    1.  Acepta el acuerdo de licencia del usuario final.  
  
    2.  En la **encontrar mi servidor** página, detectar automáticamente el servidor en las redes locales y selecciona el servidor que desee conectarse. O bien, si tienes la información, que se puede introducir manualmente la dirección de dominio o el nombre del servidor s.  
  
    3.  En la **escribe el nuevo nombre de usuario de red y la contraseña** página, haz lo siguiente:  
  
        -   Si este es el primer equipo que se conecta al servidor, y este es el equipo que vas a usar para administrar el servidor, usa la cuenta de administrador que creaste durante la instalación.  
  
        -   Para todos los demás equipos, primero crea una cuenta de usuario de la red en el servidor mediante el panel. Crear la cuenta de usuario con el administrador o estándar privilegios de usuario, en función de las tareas que realizan la persona que usa el equipo.  
  
    4.  Si tu equipo ejecuta Windows 8, Windows 8.1 o Windows 10, omite este paso. Si tu equipo ejecuta Windows 7, y si tiene documentos, imágenes o las preferencias personales (como fondos de escritorio, los protectores de pantalla o los favoritos de Internet Explorer) que quieres mantener después de unir el equipo a la nueva red, en la **elegir si quieres mover tu configuración y datos existentes** página del asistente, selecciona el **mover los datos y la configuración a mi cuenta de usuario de red nueva**.  
  
    5.  Elige si quieres activar automáticamente el equipo para crear una copia de seguridad en el **elegir si desea activar este equipo para crear su copia de seguridad** página.  
  
    6.  Sigue las instrucciones en el Asistente para unir el equipo a la red.  
  
8.  Después de unir el equipo a la red, usa el nuevo nombre de usuario y contraseña para iniciar sesión en el equipo.  
  
    > [!NOTE]
    >  Cuando inicies sesión en un equipo que ejecuta Windows 8 por primera vez mediante su cuenta de red, una vez que se conecte al servidor, aparecen instrucciones para migrar archivos y las aplicaciones de la cuenta de usuario anterior. ¿Sigue las instrucciones de la **cómo realiza la migración archivos y las aplicaciones de mi cuenta de usuario anterior? ** página para migrar todos los archivos y aplicaciones a la cuenta de usuario de la red.  
  
9. Después de que el equipo está conectado correctamente al servidor, aparecen los accesos directos a la TrayApp conector y el panel de servidor en el menú Inicio, que se puede usar de manera (si su equipo ejecuta Windows 8, Windows 8.1 o Windows 10, el panel y conector TrayApp estará disponible desde la pantalla de inicio de equipo s):  
  
    -   Si tu equipo ejecuta Windows 8, Windows 8.1 o Windows 10, el panel y conector TrayApp será búsqueda como una aplicación.  
  
    -   Desde el conector TrayApp, puedes habilitar o deshabilitar la **Mantenerme conectado de forma remota** característica. También hacer doble clic en el TrayApp para iniciar el Launchpad. Desde el Launchpad, el acceso compartido carpetas directo, configurar copias de seguridad del equipo, alertas de dirección y abre el sitio Web de acceso Web remoto.  
  
    -   Desde el **panel** vínculo, puede administrar tu servidor.  
  
###  <a name="BKMK_10"></a>Conectar equipos a un servidor de Windows Server Essentials sin unirse al dominio  
 Este tema describe cómo agregar un equipo de Windows 7, Windows 8, Windows 8.1 o Windows 10 a una red de Windows Server Essentials sin unir el equipo al dominio de Windows Server Essentials en una implementación de cliente local. Este método de conexión se admite en Windows Server Essentials y Windows Server Essentials.  
  
 Esto es una alternativa al método habitual, lo que requiere unir el equipo al dominio de Windows Server Essentials. Con ese método, si el equipo está en otro dominio, se debe quitar de ese dominio antes de que se puede agregar al dominio de Windows Server Essentials.  
  
#### <a name="feature-limits"></a>Límites de la característica  
 Algunas características están limitadas cuando un equipo cliente no se agrega al dominio de Windows Server Essentials:  
  
-   Todas las características que requieren que el equipo estar unido al dominio? incluidos redes privadas virtuales (VPN), directiva de grupo y las credenciales de dominio? no están disponibles.  
  
-   Los complementos de terceros que requieren que el equipo estar unido al dominio no funcionará correctamente.  
  
-   Este método no puede usarse para conectarse al servidor en un equipo externo.  
  
#### <a name="prerequisites"></a>Requisitos previos  
  
-   El equipo debe tener una conexión física a la red local.  
  
-   Uno de los siguientes sistemas operativos debe instalarse en el equipo:  
  
    -   Windows 10 Pro, Windows 10 Enterprise  
  
    -    Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 8 Pro, Windows 8 Enterprise  
  
    -    Windows 7 Professional (x86 y x64), Windows 7 Enterprise (x86 y x64), Windows 7 Ultimate (x86 y x64)  
  

-   El equipo debe cumplir con todos los otros requisitos para los equipos cliente de Windows Server Essentials. Para obtener más información, consulta [requisitos previos para conectar un equipo con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  

  
-   Para habilitar una conexión sin unirse al dominio, debes iniciar sesión el equipo con una cuenta que es un miembro del grupo de administradores local.  
  
-   Para conectar el equipo al servidor de Windows Server Essentials, necesitarás la siguiente información de cuenta:  
  
    -   Una cuenta con derechos de administrador en Windows Server Essentials (tu cuenta).  
  
    -   El nombre de usuario y contraseña para la cuenta de dominio de la persona que usará el equipo. La cuenta de dominio debe tener derechos de administrador en el servidor de Windows Server Essentials.  
  
#### <a name="connect-to-a-windows-server-essentials-network"></a>Conectarse a una red de Windows Server Essentials  
 Después de comprobar que se hayan cumplido todos los requisitos previos, conecta el equipo a la red de Windows Server Essentials.  
  
###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Para conectar un equipo en un dominio distinto a una red de Windows Server Essentials  
  
1.  Inicia sesión el equipo cliente con una cuenta que es un miembro del grupo de administradores local.  
  
2.  Abre un símbolo del sistema con derechos de administrador.  
  
    -   En Windows 10, haz clic en el **inicio** botón, selecciona **todas las aplicaciones** -> **herramientas del sistema Windows** -> **símbolo**, haz clic en el símbolo del sistema y, a continuación, haz clic en **ejecutar como administrador**.  
  
    -   En Windows 8, en la **inicio** , escriba **comando** y, a continuación, presione ENTRAR. En los resultados, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador**.  
  
    -   En Windows 7, en la **inicio** menú, escribe **comando** en el cuadro de búsqueda, haz clic en **símbolo**y, a continuación, haz clic en **ejecutar como administrador**.  
  
3.  En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  
  

4.  Completar los pasos de [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
####  <a name="BKMK_SecondServer"></a>Unirse a un segundo servidor a la red  
  
###### <a name="to-join-a-second-server-to-the-network"></a>Para unirte a un segundo servidor a la red  
  
1.  Iniciar sesión en el servidor que desea conectarse a la red de Windows Server Essentials.  
  
2.  Abre un explorador de Internet y en la barra de direcciones, escriba **http://<servername\>/Connect**, donde *< servername\ >* es el nombre del servidor que ejecuta Windows Server Essentials y, a continuación, presione ENTRAR.  
  
3.  Si se habilita la configuración de seguridad mejorada de Internet Explorer en el servidor que intenta conectarse a la red de Windows Server Essentials, completa las siguientes claves: de lo contrario, se omite este paso.  
  
    1.  Para aceptar el mensaje de bloqueo, haz clic en **cerrar**.  
  
    2.  Agregar el **http://<servername\>/Connect** sitio Web a los sitios Web de confianza de esta manera:  
  
        1.  En el panel de navegación del explorador, haz clic en **herramientas**y, a continuación, haz clic en **opciones de Internet**.  
  
        2.  Haz clic en el **seguridad** pestaña y, a continuación, haz clic en **sitios de confianza**.  
  
        3.  Haz clic en **sitios**.  
  
        4.  El sitio Web debe mostrarse en la **agregar este sitio Web a la zona** campo. Haz clic en **agregar**.  
  
        5.  Haz clic en **cerrar**y, a continuación, haz clic en **Aceptar**.  
  
    3.  Actualiza la página web.  
  

    4.  Para conectar el segundo servidor a un servidor que ejecuta Windows Server Essentials, sigue las instrucciones de [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
    > [!NOTE]
    >  Conectar un segundo servidor a un servidor que ejecuta Windows Server Essentials difiere de la conexión a un equipo cliente como sigue:  
    >   
    >  -   No hay ninguna migración de perfiles de usuario; por lo tanto, no se migrará el perfil actual.  
    > -   No se copia el segundo servidor mediante el uso de copia de seguridad de equipo cliente, y no hay ninguna opción para reactivar el equipo para la copia de seguridad.  
  
 Después de unir el segundo servidor a un servidor que ejecuta Windows Server Essentials, las siguientes características se proporcionan en el servidor conectado:  
  
-   Actualización y funcionalidades de estado de alertas funcionan igual que otros equipos cliente.  
  
-   Las funcionalidades en línea y sin conexión funcionan igual que otros equipos cliente.  
  
-   Puede conectar a su segundo servidor mediante el uso de acceso Web remoto.  
  
-   El segundo servidor se incluirá en los informes de estado porque Windows Server Essentials generará alertas relacionadas con este servidor.  
  
 Administración del segundo servidor desde el servidor que ejecuta Windows Server Essentials serán diferentes de administrar los equipos cliente como sigue:  
  
-   No habrá ningún punto de entrada para TrayApp, Launchpad y el cliente de escritorio.  
  
-   El segundo servidor se enumeran en la **servidores** grupo en el **dispositivos** pestaña.  
  
-   Dado que no se admite la copia de seguridad de equipo cliente para el segundo servidor, se muestra el estado de copia de seguridad como **no admite**. Además, si seleccionas el segundo servidor y con el botón derecho, no hay ninguna copia de seguridad y restauración tareas relacionadas que se muestra para el segundo servidor.  
  
-   Si selecciona el segundo servidor y, a continuación, haz clic en el **ver las propiedades del servidor** de tareas, no hay ninguna **copia de seguridad** pestaña que se muestra en la página de propiedades de servidor s.  
  
-   Dado que no hay ningún centro de seguridad en un sistema operativo Windows Server, el estado de seguridad del servidor s segundo se muestra como **no es aplicable**.  
  
-   El segundo servidor s estado de la directiva de grupo se muestra como **no es aplicable**.  
  
###  <a name="BKMK_11"></a>Instalar el software del conector  
 Cuando se conecta el equipo al servidor mediante la conectar un equipo para el Asistente de servidor, se instala el software del conector en Windows Server Essentials. Puedes iniciar este asistente escribiendo **http://<ServerName\>/connect** en la barra de direcciones del explorador web (donde *< ServerName\ >* es el nombre del servidor).  
  
> [!NOTE]
>  Si el equipo está en una ubicación remota, para ejecutar un equipo de la conexión al Asistente de servidor, escriba **http://<domainname\>/connect** en la barra de direcciones del explorador web (donde *< domain\ >* es el nombre de dominio de su organización). Puedes obtener la información del nombre de dominio desde el Administrador de red.  
  
 El software del conector hace lo siguiente:  
  
-   Conecta el equipo a Windows Server Essentials  
  
-   Copia automáticamente el equipo durante la noche (si estableces el servidor para crear copias de seguridad de cliente)  
  
-   Ayuda a administrador supervisar el estado del equipo  
  
-   Te permite configurar y administrar Windows Server Essentials de forma remota desde su equipo doméstico  
  

 Para obtener instrucciones paso a paso sobre cómo conectar el equipo al servidor de Windows Server Essentials, consulte [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   

  
###  <a name="BKMK_12"></a>Mueve los datos del equipo y la configuración manualmente  
  Windows Server Essentials y Windows Server Essentials admiten la migración de perfiles de usuario solo para equipos cliente que ejecutan el sistema operativo Windows 7. Cuando un equipo basado en Windows 7 se conecta al servidor, el equipo conectarse al servidor asistente puede migrar automáticamente el perfil de usuario.  
  
 El perfil de usuario no se puede transferir automáticamente al conectar un Windows 8, Windows 8.1 o equipo de Windows 10 con el servidor. Sin embargo, en un equipo de Windows 8, puedes usar a Windows Easy Transfer para transferir configuraciones y datos de usuario local original para el equipo unido al dominio. Para ello, debes ser administrador en el equipo de origen de Windows 8 y el equipo de destino de Windows 8. Para obtener información sobre cómo usar Windows Easy Transfer para transferir archivos y configuraciones, vea [artículo 2735227](https://support.microsoft.com/kb/2735227) en Microsoft Knowledge Base.  
  
###  <a name="BKMK_Transfer"></a>Transferir varios perfiles de usuario durante la implementación del equipo  
 Antes de conectar un equipo que ejecute el sistema de operativo de Windows 7 SP1 o Windows 7 en el servidor de Windows Server Essentials, para poder transferir varios perfiles de usuario local, primero debe crear las cuentas de usuario de red correspondiente en el servidor. Para obtener más información sobre la creación de cuentas de usuario de red, consulta [agregar una cuenta de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
  
 Solo se admite la migración de perfiles de usuario en un equipo que ejecuta Windows 7 (para Windows Server Essentials) o Windows 7 SP1 (para Windows Server Essentials). Cuando se conecta a un equipo con el servidor de Windows Server Essentials mediante la conectar el equipo al Asistente de servidor, se le proporcionará una opción para mover los datos de usuario y la configuración anterior cuentas de usuario local a las nuevas cuentas de usuario de red. Para ello, en la **mover datos de usuario existentes y configuraciones** página del asistente, se asignan las cuentas de usuario de la red a las cuentas de usuario local que existen en el equipo para transferir varios perfiles de usuario que se encuentran en el equipo cliente.  
  
###  <a name="BKMK_13"></a>Desinstalar el software del conector  
 Puedes desinstalar el software del conector desde un equipo mediante el Panel de Control. Normalmente se hacerlo si hay un problema con el software del conector o si debes instalar una versión más reciente del software del conector. Debes iniciar sesión en el equipo como administrador para completar este procedimiento.  
  
> [!IMPORTANT]
>  Si actualizas el sistema operativo en un equipo cliente, el software del conector se desinstalará automáticamente. Debe reinstalar el software del conector una vez completada la actualización. Es el método preferido desinstalar el software del conector antes de actualizar el sistema operativo. Desinstalar el software del conector una vez completada la actualización es aún aceptable; Sin embargo, esto podría hacer un estado incoherente para el equipo cliente con el servidor hasta que se desinstala y vuelve a instalar el software del conector.  
  
##### <a name="to-uninstall-connector-software-from-a-computer"></a>Para desinstalar el software del conector desde un equipo  
  
1.  Desde un equipo que ejecuta Windows 7, Windows 8, Windows 8.1 o Windows 10, abre **Panel de Control**y, a continuación, en la **programas** sección, haz clic en **ver actualizaciones instaladas**.  
  
2.  En la lista de programas instalados, selecciona **conector de Windows Server Essentials**y, a continuación, haz clic en **desinstalar**.  
  
3.  En la página de advertencia, haz clic en **Sí**.  
  
4.  Si la **Control de cuentas de usuario** aparece en la ventana, haz clic en **permitir**.  
  
5.  En Windows Server Essentials, si la página de Windows Server Essentials Connector aparece lo que sugiere para cerrar el Launchpad, haz clic en **Aceptar**.  
  
6.  Espera a que el programa que desea desinstalar. Después de quita el software, **conector de Windows Server Essentials** ya no aparece en la lista de programas instalados o actualizaciones. Además, los accesos directos para el Launchpad y el panel ya no se muestran en el escritorio del equipo s.  
  
> [!NOTE]
>  -   Desinstalar el software del conector no elimina el equipo de la lista de equipos que se muestran en la **dispositivos** ficha del panel. Para quitar el equipo desde el panel, vea [quitar un equipo desde el servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
> -   Al desinstalar el software del conector, no se eliminan las carpetas compartidas en el equipo cliente que se asignaron al servidor. Debe eliminar manualmente las carpetas compartidas que se asignan al servidor.  

> -   Desinstalar el software del conector no hace que el equipo desune el dominio original. Manualmente debe desune el equipo del dominio. Para obtener instrucciones, consulta [quitar un equipo de un dominio de Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  

  
###  <a name="BKMK_14"></a>Desconecta el equipo desde o volver a conectar el equipo al servidor  
 Para desconectar un equipo desde el servidor, debes realizar los siguientes pasos:  
  

1.  Desinstalar el software del conector desde el equipo mediante el Panel de Control. Para obtener instrucciones detalladas, consulta [desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   

  
2.  Desune el equipo del dominio de Windows Server Essentials y unirse a él al grupo de trabajo. Para obtener instrucciones paso a paso para unir Windows a un grupo de trabajo, [crear un grupo de trabajo o unirse a](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Quitar el equipo desde el servidor mediante el panel. Para obtener instrucciones detalladas, consulta [quitar un equipo desde el servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
  
 Para volver a conectar un equipo al servidor que anteriormente se desconectó de la red del servidor de Windows Server Essentials, debes realizar los siguientes pasos:  
  

1.  Desinstalar el software del conector desde el equipo mediante el Panel de Control. Para obtener instrucciones detalladas, consulta [desinstalar el software del conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
2.  Desune el equipo del dominio de Windows Server Essentials y unirse a él al grupo de trabajo. Para obtener instrucciones paso a paso para unir Windows a un grupo de trabajo, [crear un grupo de trabajo o unirse a](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Conecta el equipo al servidor mediante el Asistente para conectarse de equipo. Para obtener instrucciones detalladas, consulta [conectar equipos con el servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_Sleep"></a>¿Cómo una copia de seguridad funciona con el modo de suspensión e hibernación modos  
 Si seleccionas la **activar este equipo para la copia de seguridad** opción al conectar un equipo con el servidor, el equipo automáticamente se reactive del estado de suspensión o el modo de hibernación cada día según se especifica en la programación de copia de seguridad para que se puede ser una copia de seguridad. Cuando termine la copia de seguridad, se devolverá el equipo de modo de suspensión o hibernación, en función de su configuración de administración de energía. Si no seleccionas esta opción, el servidor no se hacer una copia de un equipo si el equipo está en modo de suspensión o hibernación. Para obtener más información, consulta [administrar copias de seguridad de cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_C"></a>Uso del Launchpad  
 Puedes usar el Launchpad para acceder a recursos compartidos del servidor de Windows Server Essentials, realizar copias de seguridad del equipo y responder a las alertas de estado del sistema.  
  
-   [Información general del LaunchPad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  
  
-   [Uso del Launchpad con un equipo Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
## <a name="see-also"></a>Consulta también  
  
-   [Solucionar problemas de equipos que se conectan al servidor](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  
  
-   [Administrar cuentas de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  

-   [Usa las carpetas compartidas](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabajar de forma remota](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Reproducir medios digitales](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Usa las carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabajar de forma remota](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Reproducir medios digitales](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

