---
title: Administración del Servidor de directivas de redes con Herramientas de administración
description: Puede usar este tema para obtener información acerca de las herramientas que puede usar para administrar el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f963e849f2ae23869431de30de983c02b87d4f9c
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865544"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Administración del Servidor de directivas de redes con Herramientas de administración

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las herramientas que puede usar para administrar su NPSs.

Después de instalar NPS, puede administrar NPSs:

- Localmente, mediante el complemento MMC de Microsoft Management Console NPS \( \) , la consola NPS estática en herramientas administrativas, comandos de Windows PowerShell o los \( \) comandos Netsh de Shell de red para NPS.
- Desde un NPS remoto, mediante el complemento MMC NPS, los comandos Netsh para NPS, los comandos de Windows PowerShell para NPS o Conexión a Escritorio remoto.
- Desde una estación de trabajo remota, mediante Conexión a Escritorio remoto en combinación con otras herramientas, como la MMC de NPS o Windows PowerShell.

>[!NOTE]
>En Windows Server 2016, puede administrar el NPS local mediante la consola de NPS. Para administrar NPSs locales y remotos, debe usar el complemento MMC de NPS \- .

En las secciones siguientes se proporcionan instrucciones sobre cómo administrar los NPSs locales y remotos.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configuración del NPS local mediante la consola de NPS

Después de haber instalado NPS, puede usar este procedimiento para administrar el NPS local mediante el MMC de NPS.

**Credenciales administrativas**

Para completar este procedimiento, debe pertenecer al grupo Administradores.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Para configurar el NPS local mediante la consola de NPS

1. En el Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Servidor de directivas de redes**. Se abre la consola NPS.

2. En la consola NPS, haga clic en NPS \( local \) . En el panel de detalles, elija **Configuración estándar** o **Configuración avanzada** y, a continuación, realice una de las siguientes acciones en función de la selección:
    - Si elige **Configuración estándar**, seleccione un escenario de la lista y, a continuación, siga las instrucciones para iniciar un asistente de configuración.
    - Si elige **Configuración avanzada**, haga clic en la flecha para expandir **Opciones de configuración avanzadas** y, a continuación, revise y configure las opciones disponibles en función de la funcionalidad de NPS que desee: servidor RADIUS, proxy RADIUS o ambos.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Administrar varios NPSs mediante el complemento MMC de NPS \-

Puede usar este procedimiento para administrar el NPS local y varios NPSs remotos mediante el complemento MMC de NPS \- .

Antes de realizar el procedimiento siguiente, debe instalar NPS en el equipo local y en los equipos remotos.

En función de las condiciones de la red y el número de NPSs que se administran mediante el complemento MMC de NPS \- , la respuesta del complemento MMC \- puede ser lenta. Además, el tráfico de configuración de NPS se envía a través de la red durante una sesión de administración remota mediante el complemento NPS \- . Asegúrese de que la red es físicamente segura y que los usuarios malintencionados no tienen acceso a este tráfico de red.

**Credenciales administrativas**

Para completar este procedimiento, debe pertenecer al grupo Administradores.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Para administrar varios NPSs mediante \- el complemento NPS

1. Para abrir MMC, ejecute Windows PowerShell como administrador. En Windows PowerShell, escriba **MMC** y, a continuación, presione Entrar. Se abre Microsoft Management Console.
2. En MMC, en el menú **archivo** , haga clic en **Agregar o quitar \- complemento**. Se abrirá el cuadro de diálogo **Agregar o quitar complementos \-** .
3. En **agregar \- o quitar complementos**, en **Complementos \- disponibles**, desplácese hacia abajo en la lista, haga clic en **servidor de directivas de redes** y, a continuación, haga clic en **Agregar**. Se abre el cuadro de diálogo **Seleccionar equipo**.
4. En **seleccionar equipo**, compruebe que está seleccionado **equipo local \( en el que se está ejecutando \) esta consola** y, a continuación, haga clic en **Aceptar**. El complemento \- para el NPS local se agrega a la lista de **Complementos seleccionados \-**.
5. En **agregar \- o quitar complementos**, en **Complementos \- disponibles**, asegúrese de que el **servidor de directivas de redes** sigue seleccionado y, a continuación, haga clic en **Agregar**. Se volverá a abrir el cuadro de diálogo **seleccionar equipo** .
6. En **seleccionar equipo**, haga clic en **otro equipo** y, a continuación, escriba la dirección IP o el \( FQDN del nombre de dominio completo \) del NPS remoto que desea administrar mediante el complemento NPS \- . Opcionalmente, puede hacer clic en **Examinar para explorar** el directorio del equipo que desea agregar. Haga clic en **OK**.
7. Repita los pasos 5 y 6 para agregar más NPSs al complemento NPS \- . Cuando haya agregado todas las NPSs que desea administrar, haga clic en **Aceptar**.
8. Para guardar el complemento NPS para su uso posterior, haga clic en **archivo** y, a continuación, haga clic en **Guardar**. En el cuadro de diálogo **Guardar como** , vaya a la ubicación del disco duro donde desea guardar el archivo, escriba un nombre para el archivo. msc de Microsoft Management Console \( y, \) a continuación, haga clic en **Guardar**.

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Administrar un NPS mediante Conexión a Escritorio remoto

Puede usar este procedimiento para administrar un NPS remoto mediante Conexión a Escritorio remoto.

Mediante el uso de Conexión a Escritorio remoto, puede administrar de forma remota el NPSs que ejecuta Windows Server 2016. También puede administrar de forma remota NPSs desde un equipo que ejecute Windows 10 o versiones anteriores de los sistemas operativos de cliente de Windows.

Puede usar Escritorio remoto conexión para administrar varios NPSs mediante uno de estos dos métodos.

1. Cree una conexión Escritorio remoto a cada uno de los NPSs de forma individual.
2. Use Escritorio remoto para conectarse a un NPS y, a continuación, use el MMC de NPS en ese servidor para administrar otros servidores remotos. Para obtener más información, consulte la sección anterior **Administración de varios NPSs mediante el complemento MMC \- de NPS**.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo administradores en el NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Para administrar un NPS mediante Conexión a Escritorio remoto

1. En cada NPS que desea administrar de forma remota, en Administrador del servidor, seleccione **servidor local**. En el panel de detalles del Administrador del servidor, vea la configuración de **escritorio remoto** y realice una de las siguientes acciones.
    1. Si el valor de la opción **escritorio remoto** está **habilitado**, no es necesario realizar algunos de los pasos de este procedimiento. Vaya al paso 4 para empezar a configurar los permisos de usuario de Escritorio remoto.
    2. Si el valor de **escritorio remoto** está **deshabilitado**, haga clic en la palabra **deshabilitada**. Se abrirá el cuadro de diálogo **propiedades del sistema** en la pestaña acceso **remoto** .
2. En **escritorio remoto**, haga clic en **permitir conexiones remotas a este equipo**. Se abrirá el cuadro de diálogo **conexión a escritorio remoto** . Realice una de las acciones siguientes.
    1. Para personalizar las conexiones de red que se permiten, haga clic en **firewall de Windows con seguridad avanzada** y, a continuación, configure las opciones que desea permitir.
    2. Para habilitar Conexión a Escritorio remoto para todas las conexiones de red en el equipo, haga clic en **Aceptar**.
3. En **propiedades del sistema**, en **escritorio remoto**, decida si desea habilitar **permitir conexiones solo desde equipos que ejecutan escritorio remoto con autenticación a nivel de red** y realice la selección.
4. Haga clic en **Seleccionar usuarios**. Se abre el cuadro de diálogo **escritorio remoto usuarios** .
5. En **escritorio remoto usuarios**, para conceder permiso a un usuario para conectarse de forma remota al NPS, haga clic en **Agregar** y, a continuación, escriba el nombre de usuario de la cuenta de usuario. Haga clic en **OK**.
6. Repita el paso 5 para cada usuario para el que desea conceder permiso de acceso remoto al NPS. Cuando haya terminado de agregar usuarios, haga clic en **Aceptar** para cerrar el cuadro de diálogo **escritorio remoto usuarios** y en **Aceptar** de nuevo para cerrar el cuadro de diálogo **propiedades del sistema** .
7. Para conectarse a un NPS remoto que ha configurado mediante los pasos anteriores, haga clic en **Inicio**, desplácese hacia abajo en la lista alfabética, haga clic en **accesorios de Windows** y, a continuación, haga clic en **conexión a escritorio remoto**. Se abrirá el cuadro de diálogo **conexión a escritorio remoto** .
8. En el cuadro de diálogo **conexión a escritorio remoto** , en **equipo**, escriba el nombre o la dirección IP de NPS. Si lo prefiere, haga clic en **Opciones**, configure opciones de conexión adicionales y, a continuación, haga clic en **Guardar** para guardar la conexión para su uso repetido.
9. Haga clic en **conectar** y, cuando se le solicite, proporcione las credenciales de la cuenta de usuario para una cuenta que tenga permisos para iniciar sesión en y configurar el NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Usar comandos Netsh NPS para administrar un NPS

Puede usar comandos en el contexto Netsh NPS para mostrar y establecer la configuración de la base de datos de autenticación, autorización, cuentas y auditoría utilizada por NPS y el servicio de acceso remoto. Use los comandos del contexto Netsh NPS para:

- Configure o vuelva a configurar un NPS, incluidos todos los aspectos de NPS que también están disponibles para la configuración mediante la consola de NPS en la interfaz de Windows.
- Exporte la configuración de un NPS (el servidor de origen), incluidas las claves del registro y el almacén de configuración de NPS, como un script de Netsh.
- Importe la configuración a otro NPS mediante un script netsh y el archivo de configuración exportado desde el NPS de origen.

Puede ejecutar estos comandos desde el símbolo del sistema de Windows Server 2016 o desde Windows PowerShell. También puede ejecutar comandos Netsh NPS en scripts y archivos por lotes.

**Credenciales administrativas**

Para llevar a cabo este procedimiento, debe pertenecer al grupo de administradores del equipo local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Para especificar el contexto Netsh NPS en un NPS

1. Abra el símbolo del sistema o Windows PowerShell.
2. Escriba **netsh** y, a continuación, presione Entrar.
3. Escriba **NPS** y, a continuación, presione Entrar.
4. Para ver una lista de los comandos disponibles, escriba un signo de interrogación \( ? \) y presione Entrar.


Para obtener más información acerca de los comandos Netsh NPS, consulte [comandos Netsh para el servidor de directivas de redes en Windows server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754428(v=ws.10))o descargue la [referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) completa desde la galería de TechNet. Esta descarga es una referencia técnica completa de Shell de red para Windows Server 2008 y Windows Server 2008 R2. El formato es Windows Help \( *. chm \) en un archivo zip. Estos comandos siguen presentes en Windows Server 2016 y Windows 10, por lo que puede usar Netsh en estos entornos, aunque se recomienda usar Windows PowerShell.

## <a name="use-windows-powershell-to-manage-npss"></a>Usar Windows PowerShell para administrar NPSs

Puede usar los comandos de Windows PowerShell para administrar NPSs. Para obtener más información, vea los siguientes temas de referencia de comandos de Windows PowerShell.

- [Cmdlets del servidor de directivas de redes (NPS) en Windows PowerShell](/powershell/module/nps/). Puede usar estos comandos Netsh en Windows Server 2012 R2 o sistemas operativos posteriores.
- [Módulo NPS](/powershell/module/nps/). Puede usar estos comandos Netsh en Windows Server 2016.

Para obtener más información acerca de la administración de NPS, consulte [Administración del servidor de directivas de redes (NPS)](nps-manage-top.md).
