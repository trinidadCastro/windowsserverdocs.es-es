---
title: Administración de servidor de directivas de red con herramientas de administración
description: Puedes usar este tema para obtener información sobre las herramientas que puedes usar para administrar el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>Administración de servidor de directivas de red con herramientas de administración

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre las herramientas que puedes usar para administrar los servidores NPS.

Después de instalar NPS, puede administrar los servidores NPS:

- Localmente, mediante el complemento Microsoft Management Console de NPS \(MMC\), la consola NPS estática en Herramientas administrativas, comandos de Windows PowerShell o los comandos de \(Netsh\) de Shell de red para NPS.
- Desde un servidor NPS remoto, mediante el complemento MMC NPS, los comandos Netsh para NPS, los comandos de Windows PowerShell para NPS, o conexión a Escritorio remoto.
- Desde una estación de trabajo remota, mediante el uso de conexión a Escritorio remoto en combinación con otras herramientas, como el MMC NPS o Windows PowerShell.

>[!NOTE]
>En Windows Server 2016, puedes administrar el servidor NPS local mediante la consola NPS. Para administrar los servidores NPS locales y remotos, debes usar MMC snap\ en NPS.

Las siguientes secciones proporcionan instrucciones sobre cómo administrar los servidores NPS locales y remotos.

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>Configurar el servidor NPS Local mediante la consola NPS

Después de haber instalado NPS, puedes usar este procedimiento para administrar el servidor NPS local mediante el MMC NPS.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo de administradores.

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>Para configurar el servidor NPS local mediante la consola NPS

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola NPS.

2. En la consola NPS, haz clic en \(Local\) NPS. En el panel de detalles, elige **configuración estándar** o **Advanced Configuration**, y, a continuación, realiza una de las siguientes acciones según su selección:
    - Si eliges **configuración estándar**, selecciona un escenario de la lista y, a continuación, sigue las instrucciones para iniciar un asistente de configuración.
    - Si eliges **Advanced Configuration**, haz clic en la flecha para expandir **opciones de configuración avanzada**y, a continuación, revisar y configurar las opciones disponibles en función de en la que desee - funcionalidad NPS RADIUS server, proxy RADIUS o ambos.

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>Administrar varios servidores NPS mediante el complemento de MMC NPS Snap\ en

Puedes usar este procedimiento para administrar el servidor NPS local y varios servidores remotos de NPS mediante el MMC NPS en snap\.

Antes de realizar el siguiente procedimiento, debes instalar NPS en el equipo local y en los equipos remotos.

Según las condiciones de red y el número de servidores NPS que se administran mediante el MMC NPS en snap\, la respuesta de MMC snap\ en puede ser lento. Además, tráfico de configuración del servidor NPS se envía a través la red durante una sesión de administración remota mediante el uso de NPS en snap\. Asegúrate de que la red es físicamente segura y que los usuarios malintencionados no tienen acceso a este tráfico de red.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo de administradores.

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>Administrar varios servidores NPS mediante el NPS snap\ en

1. Para abrir MMC, ejecutar Windows PowerShell como administrador. En Windows PowerShell, escribe **mmc**, y, a continuación, presione ENTRAR. Abre la consola de administración de Microsoft.
2. En la consola de MMC, en la **archivo** menú, haz clic en **agregar o quitar Snap\ en**. La **agregar o quitar complementos Snap\** abre el cuadro de diálogo.
3. En **agregar o quitar complementos Snap\**, en **disponibles snap\-ins**, desplázate hacia abajo de la lista, haz clic en **el servidor de directivas de red**y, a continuación, haz clic en **agregar**. La **Seleccionar equipo** abre el cuadro de diálogo.
4. En **Seleccionar equipo**, comprueba que **equipo Local \ (el equipo en el que esta consola es running\)** está seleccionado y, a continuación, haz clic en **Aceptar**. El snap\ en el servidor NPS local se agrega a la lista de **seleccionado snap\-ins**.
5. En **agregar o quitar complementos Snap\**, en **disponibles snap\-ins**, asegúrate de que **el servidor de directivas de red** es aún seleccionada y, a continuación, haz clic en **agregar**. La **Seleccionar equipo** vuelve a abre el cuadro de diálogo.
6. En **Seleccionar equipo**, haz clic en **otro equipo**y a continuación, escribe la dirección IP o el nombre de dominio completo \(FQDN\) del servidor NPS remoto que quieras administrar mediante la NPS snap\ en el nombre. Opcionalmente, puedes hacer clic en **examinar** para examinar el directorio del equipo que quieres agregar. Haz clic en **Aceptar**.
7. Repite los pasos 5 y 6 para agregar más servidores NPS a NPS en snap\. Una vez hayas agregado todos los servidores NPS que quieras administrar, haz clic en **Aceptar**.
8. Para guardar el complemento NPS para usarlo más adelante, haz clic en **archivo**y, a continuación, haz clic en **guardar**. En la **Guardar como** cuadro de diálogo, ve a la ubicación de disco duro donde quieres guardar el archivo, escribe un nombre para tu archivo \(.msc\) de Microsoft Management Console y, a continuación, haz clic en **guardar**. 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>Administrar un servidor NPS mediante conexión a Escritorio remoto

Puedes usar este procedimiento para administrar un servidor NPS remoto mediante la conexión a Escritorio remoto.

Mediante el uso de conexión a Escritorio remoto, puede administrar de forma remota los servidores NPS que ejecutan Windows Server 2016. Puede administrar remotamente servidores NPS desde un equipo que ejecute Windows 10 o sistemas de operativos de cliente de Windows anteriores.

Puedes usar conexión a Escritorio remoto para administrar varios servidores NPS mediante uno de dos métodos.

1. Crear una conexión a Escritorio remoto a cada uno de los servidores NPS individualmente.
2. Usar Escritorio remoto para conectarse a un servidor NPS y, a continuación, usar MMC NPS en el servidor para administrar otros servidores remotos. Para obtener más información, consulta la sección anterior **administrar varios servidores NPS mediante el MMC NPS Snap\ en**.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo Administradores en el servidor NPS.

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>Para administrar un servidor NPS mediante conexión a Escritorio remoto

1. En cada servidor NPS que quieras administrar de forma remota, en el administrador del servidor, selecciona **servidor Local**. En el panel de detalles del administrador del servidor, ver la **escritorio remoto** configuración y realiza una de las siguientes acciones. 
    1. Si el valor de la **escritorio remoto** configuración es **habilitado**, no es necesario realizar algunos de los pasos de este procedimiento. Omitir hasta el paso 4 para empezar a configurar los permisos de usuario de escritorio remoto.
    2. Si la **escritorio remoto** configuración es **deshabilitado**, haz clic en la palabra **deshabilitado**. La **propiedades del sistema** abre el cuadro de diálogo en el **remoto** pestaña.
2. En **escritorio remoto**, haz clic en **permitir conexiones remotas a este equipo**. La **conexión a Escritorio remoto** abre el cuadro de diálogo. Realiza una de las siguientes acciones.
    1. Para personalizar las conexiones de red que se pueden usar, haz clic en **Firewall de Windows con seguridad avanzada**y, a continuación, configura las opciones que desea permitir. 
    2. Para habilitar la conexión a Escritorio remoto para todas las conexiones en el equipo de red, haz clic en **Aceptar**.
3. En **propiedades del sistema**, en **escritorio remoto**, decide si vas a habilitar **permitir conexiones solo de equipos que ejecuten Escritorio remoto con autenticación a nivel de red**y realice una selección.
4. Haz clic en **Seleccionar usuarios**. La **usuarios de escritorio remoto** abre el cuadro de diálogo.
5. En **usuarios de escritorio remoto**, para conceder permiso a un usuario para conectarse remotamente al servidor NPS, haz clic en **agregar**y, a continuación, escribe el nombre de usuario de la cuenta del usuario. Haz clic en **Aceptar**.
6. Repite el paso 5 para cada usuario para el que desea conceder permiso de acceso remoto en el servidor NPS. Cuando hayas terminado de agregar usuarios, haz clic en **Aceptar** para cerrar la **usuarios de escritorio remoto** cuadro de diálogo y **Aceptar** nuevamente para cerrar la **propiedades del sistema** cuadro de diálogo.
7. Para conectarse a un servidor NPS remoto que hayas configurado siguiendo los pasos anteriores, haz clic en **inicio**, desplázate hacia abajo de la lista alfabética y, a continuación, haz clic en **accesorios de Windows**y haz clic en **conexión a Escritorio remoto**. La **conexión a Escritorio remoto** abre el cuadro de diálogo.
8. En la **conexión a Escritorio remoto** cuadro de diálogo **equipo**, escribe la dirección IP o el nombre del servidor NPS. Si lo prefieres, haz clic en **opciones**, configurar opciones de conexión adicionales y, a continuación, haz clic en **guardar** para guardar la conexión para usarlos.
9. Haz clic en **conectar**y cuando se te solicite proporcionar las credenciales de cuenta de usuario para una cuenta con permisos para iniciar sesión y configurar el servidor NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>Usar los comandos Netsh NPS para administrar un servidor NPS

Puedes usar comandos en el contexto de Netsh NPS para mostrar y establecer la configuración de la autenticación, autorización, cuentas y utilizado NPS y el servicio de acceso remoto de la base de datos de auditoría. Use los comandos en el contexto Netsh NPS:

- Configurar o volver a configurar un servidor NPS, incluidos todos los aspectos de NPS que también están disponibles para la configuración mediante la consola NPS en la interfaz de Windows.
- Exportar la configuración de un servidor NPS (servidor de origen), incluidas las claves del registro y el almacén de configuración de NPS, como una secuencia de comandos Netsh.
- Importar la configuración a otro servidor NPS mediante una secuencia de comandos Netsh y el archivo de configuración exportados desde el servidor NPS de origen.

Puedes ejecutar estos comandos de la línea de comandos de Windows Server 2016 o de Windows PowerShell. También puedes ejecutar comandos netsh nps en scripts y archivos por lotes.

**Credenciales administrativas** 

Para realizar este procedimiento, debe ser miembro del grupo Administradores en el equipo local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>Para entrar en el contexto de Netsh NPS en un servidor NPS

1. Abre Windows PowerShell o el símbolo del sistema.
2. Tipo **netsh**, y, a continuación, presione ENTRAR.
3. Tipo **nps**, y, a continuación, presione ENTRAR.
4. Para ver una lista de comandos disponibles, escribe un signo de interrogación \(?\) y presiona ENTRAR.


Para obtener más información acerca de los comandos Netsh NPS, consulta [comandos Netsh para el servidor de directivas de red en Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), o descargar todo el [referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) desde la Galería de TechNet. Esta descarga es el completa red Shell técnica referencia para Windows Server 2008 y Windows Server 2008 R2. El formato es \(*.chm\) ayuda de Windows en un archivo zip. Estos comandos son siguen presentes en Windows Server 2016 y Windows 10, para que puedas usar netsh en estos entornos, aunque se recomienda usar Windows PowerShell.

## <a name="use-windows-powershell-to-manage-nps-servers"></a>Usar Windows PowerShell para administrar los servidores NPS

Puedes usar comandos de Windows PowerShell para administrar los servidores NPS. Para obtener más información, consulta los siguientes temas de referencia de comandos de Windows PowerShell.

- [Cmdlets de directiva de servidor (NPS) en Windows PowerShell de red](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Puedes usar estos comandos netsh en Windows Server 2012 R2 o sistemas operativos posteriores.
- [Módulo NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Puedes usar estos comandos netsh en Windows Server 2016.

Para obtener más información acerca de la administración de NPS, consulta [servidor de directivas de redes (NPS) administrar](nps-manage-top.md).
