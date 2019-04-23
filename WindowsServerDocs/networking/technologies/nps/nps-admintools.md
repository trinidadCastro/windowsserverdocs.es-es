---
title: Administración del Servidor de directivas de redes con Herramientas de administración
description: Puede utilizar este tema para obtener información acerca de las herramientas que puede usar para administrar el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856886"
---
# <a name="network-policy-server-management-with-administration-tools"></a>Administración del Servidor de directivas de redes con Herramientas de administración

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información acerca de las herramientas que puede usar para administrar sus NPSs.

Después de instalar NPS, puede administrar NPSs:

- Localmente, mediante la consola de administración de Microsoft NPS \(MMC\) complemento, la consola de NPS estática en Herramientas administrativas, los comandos de Windows PowerShell o el Shell de red \(Netsh\) comandos para NPS.
- Desde un NPS remoto, utilizando el complemento MMC NPS, los comandos Netsh para NPS, los comandos de Windows PowerShell para NPS o conexión a Escritorio remoto.
- Desde una estación de trabajo remota, mediante el uso de conexión a Escritorio remoto en combinación con otras herramientas, como la MMC de NPS o Windows PowerShell.

>[!NOTE]
>En Windows Server 2016, puede administrar el NPS local mediante la consola NPS. Para administrar NPSs locales y remotos, debe usar el complemento MMC NPS\-en.

Las secciones siguientes proporcionan instrucciones sobre cómo administrar sus NPSs locales y remotos.

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>Configurar el NPS Local mediante la consola de NPS

Después de instalar NPS, puede usar este procedimiento para administrar el NPS local mediante la MMC de NPS.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo Administradores.

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>Para configurar el NPS local mediante la consola de NPS

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de NPS.

2. En la consola NPS, haga clic en NPS \(Local\). En el panel de detalles, elija **configuración estándar** o **Advanced Configuration**, y, a continuación, realice una de las siguientes acciones según su selección:
    - Si elige **configuración estándar**, seleccione un escenario de la lista y, a continuación, siga las instrucciones para iniciar un asistente de configuración.
    - Si elige **Advanced Configuration**, haga clic en la flecha para expandir **opciones de configuración avanzada**y, a continuación, revise y configure las opciones disponibles según la funcionalidad NPS que desea: Servidor RADIUS, proxy RADIUS o ambos.

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>Administrar varios NPSs mediante el complemento MMC NPS\-en

Puede usar este procedimiento para administrar el NPS local y varios NPSs remotos mediante el complemento MMC NPS\-en.

Antes de realizar el siguiente procedimiento, debe instalar NPS en el equipo local y en equipos remotos.

Según las condiciones de red y el número de NPSs se administran mediante el complemento MMC NPS\-de respuesta en el complemento MMC\-en podría ser lenta. Además, se envía tráfico de la configuración de NPS a través de la red durante una sesión de administración remota mediante el complemento NPS\-en. Asegúrese de que la red es físicamente segura y que los usuarios malintencionados no tienen acceso a este tráfico de red.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo Administradores.

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>Para administrar varios NPSs mediante el complemento NPS\-en

1. Para abrir el complemento MMC, ejecute Windows PowerShell como administrador. En Windows PowerShell, escriba **mmc**, y, a continuación, presione ENTRAR. Se abre Microsoft Management Console.
2. En MMC, en el **archivo** menú, haga clic en **agregar o quitar complemento\-en**. El **agregar o quitar complementos\-ins** abre el cuadro de diálogo.
3. En **agregar o quitar complementos\-ins**, en **complemento disponible\-ins**, desplácese hacia abajo en la lista, haga clic en **servidor de directivas de red**y, a continuación, haga clic en  **Agregar**. Se abre el cuadro de diálogo **Seleccionar equipo**.
4. En **Seleccionar equipo**, compruebe que **ordenador \(el equipo en el que se está ejecutando esta consola\)**  está seleccionada y, a continuación, haga clic en **Aceptar**. El complemento\-en para el NPS local se agrega a la lista en **seleccionado complemento\-ins**.
5. En **agregar o quitar complementos\-inicios**, en **complemento disponible\-ins**, asegúrese de que **servidor de directivas de red** sigue seleccionado y, a continuación, haga clic en  **Agregar**. El **Seleccionar equipo** abre el cuadro de diálogo nuevo.
6. En **Seleccionar equipo**, haga clic en **otro equipo**y, a continuación, escriba la dirección IP o nombre de dominio completo \(FQDN\) de NPS remoto que desea administrar mediante el uso de NPS ajustar\-en. Si lo desea, puede hacer clic en **examinar** a examinar detenidamente el directorio para el equipo que desea agregar. Haga clic en **Aceptar**.
7. Repita los pasos 5 y 6 para agregar más NPSs para el complemento NPS\-en. Cuando haya agregado todas las NPSs que desea administrar, haga clic en **Aceptar**.
8. Para guardar el complemento NPS para su uso posterior, haga clic en **archivo**y, a continuación, haga clic en **guardar**. En el **Guardar como** cuadro de diálogo, vaya a la ubicación de disco duro donde desea guardar el archivo, escriba un nombre para la consola de administración de Microsoft \(.msc\) de archivo y, a continuación, haga clic en **guardar**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>Administración de NPS mediante conexión a Escritorio remoto

Puede usar este procedimiento para administrar un NPS remoto mediante conexión a Escritorio remoto.

Al usar conexión a Escritorio remoto, puede administrar de forma remota sus NPSs que ejecuta Windows Server 2016. Puede administrar remotamente NPSs desde un equipo que ejecuta Windows 10 o sistemas de operativos de cliente de Windows anteriores.

Puede usar conexión a Escritorio remoto para administrar varios NPSs mediante uno de estos dos métodos.

1. Crear una conexión a Escritorio remoto a cada uno de sus NPSs individualmente.
2. Usar Escritorio remoto para conectarse a un servidor NPS y, a continuación, utilice el MMC de NPS en ese servidor para administrar otros servidores remotos. Para obtener más información, consulte la sección anterior **administrar varios NPSs utilizando el MMC de NPS ajustar\-en**.

**Credenciales administrativas** 

Para completar este procedimiento, debe ser miembro del grupo Administradores en el NPS.

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>Para administrar un NPS mediante conexión a Escritorio remoto

1. En cada NPS que desea administrar de forma remota, en el administrador del servidor, seleccione **servidor Local**. En el panel de detalles del administrador del servidor, puede ver el **escritorio remoto** configuración y realice una de las siguientes acciones. 
    1. Si el valor de la **escritorio remoto** configuración es **habilitado**, no es necesario realizar algunos de los pasos de este procedimiento. Vaya hasta el paso 4 para empezar a configurar los permisos de usuario de escritorio remoto.
    2. Si el **escritorio remoto** configuración es **deshabilitado**, haga clic en la palabra **deshabilitado**. El **las propiedades del sistema** abre el cuadro de diálogo en el **remoto** ficha.
2. En **escritorio remoto**, haga clic en **permitir conexiones remotas con este equipo**. El **conexión a Escritorio remoto** abre el cuadro de diálogo. Realice una de las siguientes acciones:
    1. Para personalizar las conexiones de red que se permiten, haga clic en **Firewall de Windows con seguridad avanzada**y, a continuación, configure las opciones que desee permitir. 
    2. Para habilitar la conexión a Escritorio remoto para todas las conexiones en el equipo de red, haga clic en **Aceptar**.
3. En **las propiedades del sistema**, en **escritorio remoto**, decida si desea habilitar **permitir conexiones sólo desde equipos que ejecuten Escritorio remoto con autenticación a nivel de red**y realice su selección.
4. Haz clic en **Seleccionar usuarios**. El **Remote Desktop Users** abre el cuadro de diálogo.
5. En **Remote Desktop Users**, para conceder permiso a un usuario para conectarse de forma remota al NPS, haga clic en **agregar**y, a continuación, escriba el nombre de usuario para la cuenta de usuario. Haga clic en **Aceptar**.
6. Repita el paso 5 para cada usuario para el que desea conceder permiso de acceso remoto a NPS. Cuando haya terminado de agregar usuarios, haga clic en **Aceptar** para cerrar el **Remote Desktop Users** cuadro de diálogo y **Aceptar** otra vez para cerrar el **propiedades del sistema**cuadro de diálogo.
7. Para conectarse a un NPS remoto que ha configurado mediante el uso de los pasos anteriores, haga clic en **iniciar**, desplácese hacia abajo en la lista alfabética y, a continuación, haga clic en **accesorios de Windows**y haga clic en **remoto Conexión a escritorio**. El **conexión a Escritorio remoto** abre el cuadro de diálogo.
8. En el **conexión a Escritorio remoto** cuadro de diálogo **equipo**, escriba la dirección IP o nombre NPS. Si lo prefiere, haga clic en **opciones**, configurar opciones de conexión adicionales y, a continuación, haga clic en **guardar** para guardar la conexión para su uso repetido.
9. Haga clic en **Connect**y cuando se le solicite proporcionar las credenciales de cuenta de usuario para una cuenta que tenga permisos para iniciar una sesión y configurar el NPS.

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>Use los comandos de Netsh NPS para administrar un NPS

Puede usar comandos en el contexto Netsh NPS para mostrar y establecer la configuración de la autenticación, autorización, cuentas y auditoría de base de datos utilizada tanto por NPS y el servicio de acceso remoto. Utilice los comandos en el contexto Netsh NPS para:

- Configurar o volver a configurar NPS, incluidos todos los aspectos de NPS que también están disponibles para la configuración mediante la consola NPS en la interfaz de Windows.
- Exportar la configuración de un almacén NPS (servidor de origen), incluidas las claves del registro y la configuración de NPS, como una secuencia de comandos de Netsh.
- Importar la configuración a otro NPS mediante el uso de una secuencia de comandos Netsh y el archivo de configuración exportado desde el origen de NPS.

Puede ejecutar estos comandos desde la línea de comandos de Windows Server 2016 o desde Windows PowerShell. También puede ejecutar los comandos netsh nps en scripts y archivos por lotes.

**Credenciales administrativas** 

Para llevar a cabo este procedimiento, debe pertenecer al grupo de administradores del equipo local.

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>Para entrar en el contexto Netsh NPS en NPS

1. Abra el símbolo del sistema o Windows PowerShell.
2. Tipo **netsh**, y, a continuación, presione ENTRAR.
3. Tipo **nps**, y, a continuación, presione ENTRAR.
4. Para ver una lista de comandos disponibles, escriba un signo de interrogación \(?\) y presione ENTRAR.


Para obtener más información acerca de los comandos Netsh NPS, consulte [comandos Netsh para el servidor de directivas de redes en Windows Server 2008](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx), o descargar toda la [referencia técnica de Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0) desde la Galería de TechNet. Esta descarga es la completa red Shell técnica referencia para Windows Server 2008 y Windows Server 2008 R2. El formato es ayudar a Windows \(*.chm\) en un archivo zip. Estos comandos están aún presentes en Windows Server 2016 y Windows 10, por lo que puede usar netsh en estos entornos, aunque se recomienda usar Windows PowerShell.

## <a name="use-windows-powershell-to-manage-npss"></a>Usar Windows PowerShell para administrar NPSs

Puede usar los comandos de Windows PowerShell para administrar NPSs. Para obtener más información, consulte los siguientes temas de referencia de comandos de Windows PowerShell.

- [Cmdlets de directiva de servidor (NPS) en Windows PowerShell de red](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx). Puede usar estos comandos de netsh en Windows Server 2012 R2 o sistemas operativos posteriores.
- [Módulo NPS](https://technet.microsoft.com/itpro/powershell/windows/nps/index). Puede usar estos comandos de netsh en Windows Server 2016.

Para obtener más información acerca de la administración de NPS, consulte [Administrar servidor de directivas de redes (NPS)](nps-manage-top.md).
