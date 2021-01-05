---
title: Mover la configuración y los datos al servidor de destino para la migración a Windows Server Essentials
description: Obtenga información acerca de cómo migrar la configuración y los datos al servidor de destino para la migración a Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 1b9ee5e4c52b812ab05a251257625772c361c1e8
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810688"
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Mover la configuración y los datos al servidor de destino para la migración a Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para mover la configuración y los datos al servidor de destino, haga lo siguiente:

1. [Copiar los datos en el servidor de destino](#copy-data-to-the-destination-server)

2. [Configurar la red](#configure-the-network)

3. [Asignar los equipos permitidos a cuentas de usuario](#map-permitted-computers-to-user-accounts)

## <a name="copy-data-to-the-destination-server"></a>Copiar los datos en el servidor de destino
 Antes de copiar los datos del servidor de origen en el servidor de destino, realice las siguientes tareas:

- Revise la lista de carpetas compartidas en el servidor de origen, incluidos los permisos para cada carpeta. Cree o personalice las carpetas en el servidor de destino para que coincidan con la estructura de carpetas que va a migrar desde el servidor de origen.

- Revise el tamaño de cada carpeta y asegúrese de que el servidor de destino tiene suficiente espacio de almacenamiento.

- Asigne permisos de solo lectura a las carpetas compartidas en el servidor de origen para todos los usuarios. De esta forma, no se escribirá en la unidad mientras se estén copiando archivos al servidor de destino.

#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Para copiar los datos del servidor de origen en el servidor de destino:

1. Inicie sesión en el servidor de destino como un administrador de dominio y, a continuación, abra una ventana de comandos.

2. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:

 `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`

 Donde:
 - \<SourceServerName\> es el nombre del servidor de origen.
 - \<SharedSourceFolderName\> es el nombre de la carpeta compartida en el servidor de origen.
 - \<DestinationServerName\> es el nombre del servidor de destino.
 - \<SharedDestinationFolderName\> es la carpeta compartida en el servidor de destino en la que se copiarán los datos.

3. Repita el paso anterior para cada carpeta compartida que vaya a migrar desde el servidor de origen.

## <a name="configure-the-network"></a>Configuración de la red
 Después de mover el rol de DHCP al enrutador, configure la red en el servidor de destino.

#### <a name="to-configure-the-network"></a>Para configurar la red

1. En el servidor de destino, abra el panel.

2. En la página **Inicio** del panel, haga clic en **CONFIGURACIÓN**, haga clic en **Configurar Acceso desde cualquier lugar** y, después, elija la opción **Haga clic para configurar el Acceso desde cualquier lugar**.

3. Siga las instrucciones del asistente para configurar el enrutador y los nombres de dominio.

 Si el enrutador no es compatible con el entorno UPnP, o si este se ha deshabilitado, puede aparecer un icono de advertencia amarillo junto al nombre del enrutador. Asegúrese de que los puertos siguientes están abiertos y dirigidos a la dirección IP del servidor de destino:

- Puerto 80: tráfico web HTTP

- Puerto 443: tráfico web HTTPS

## <a name="map-permitted-computers-to-user-accounts"></a>Asignar los equipos permitidos a cuentas de usuario
 Cada cuenta de usuario que se migra desde el servidor de origen debe asignarse a uno o más equipos.

#### <a name="to-map-user-accounts-to-computers"></a>Para asignar las cuentas de usuario a equipos:

1. Abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Usuarios**.

3. En la lista de cuentas de usuario, haga clic con el botón secundario en una cuenta de usuario y, con el botón primario, en **Ver las propiedades de la cuenta**.

4. Haga clic en la pestaña **Acceso desde cualquier lugar** y, a continuación, haga clic en **Permitir el Acceso Web remoto y obtener acceso a las aplicaciones de servicios web**.

5. Seleccione **Carpetas compartidas**, **Equipos**, **Enlaces a la página de inicio** y, a continuación, haga clic en **Aplicar**.

6. Haga clic en la pestaña **Acceso al equipo** y en el nombre del equipo al que desea permitir el acceso.

7. Repita los pasos 3, 4, 5 y 6 para cada una de las cuentas de usuario.

> [!NOTE]
> No es necesario cambiar la configuración del equipo cliente. Se configura automáticamente.
>
> Tras completar la migración, si se produce algún problema al crear la primera cuenta de usuario nueva en el servidor de destino, quite la cuenta de usuario que ha agregado y créela de nuevo.
