---
title: Add Servers to Server Manager
description: Obtenga información acerca de cómo agregar servidores al grupo de servidores de Administrador del servidor.
ms.topic: article
ms.assetid: aab895f2-fe4d-4408-b66b-cdeadbd8969e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.localizationpriority: medium
ms.date: 02/01/2018
ms.openlocfilehash: bca7f9025822854ffb7110d3c7a4999f79759861
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965747"
---
# <a name="add-servers-to-server-manager"></a>Add Servers to Server Manager

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server puede administrar varios servidores remotos mediante una sola consola de Administrador del servidor. Los servidores que desea administrar mediante Administrador del servidor pueden ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Tenga en cuenta que no puede administrar una versión más reciente de Windows Server con una versión anterior de Administrador del servidor.

En este tema se describe cómo agregar servidores al grupo de servidores de Administrador del servidor.

> [!NOTE]
> En nuestras pruebas, se pueden usar Administrador del servidor en Windows Server 2012 y versiones posteriores de Windows Server para administrar hasta 100 servidores que estén configurados con una carga de trabajo típica. El número de servidores que puede administrar mediante una única consola de Administrador del servidor puede variar en función de la cantidad de datos que solicite a los servidores administrados y de los recursos de hardware y de red disponibles para el equipo que ejecuta Administrador del servidor. A medida que la cantidad de datos que desea mostrar se aproxima a la capacidad de recursos del equipo, puede experimentar respuestas lentas de Administrador del servidor y retrasos en la finalización de las actualizaciones. Para ayudar a aumentar el número de servidores que puede administrar mediante Administrador del servidor, se recomienda limitar los datos de evento que Administrador del servidor obtiene de los servidores administrados, mediante el uso de la configuración del cuadro de diálogo **configurar datos de eventos** . Configurar datos de eventos se puede abrir desde el menú **Tareas** del icono **Eventos**. Si necesita administrar un número de servidores de nivel empresarial en su organización, se recomienda evaluar los productos del conjunto de [Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
>
> El Administrador del servidor solo puede recibir el estado con o sin conexión de los servidores que ejecutan Windows Server 2003. Aunque puede usar Administrador del servidor para realizar tareas de administración en servidores que ejecutan Windows Server 2008 R2 o Windows Server 2008, no puede Agregar roles y características a los servidores que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.
>
> Administrador del servidor no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. Administrador del servidor que se ejecutan en Windows Server 2012 R2, Windows Server 2012, Windows 8.1 o Windows 8 no se pueden usar para administrar servidores que ejecutan Windows Server 2016.

En este tema se incluyen las siguientes secciones.

-   [Agregar servidores para administrar](#BKMK_add)

-   [Proporciona credenciales con el comando «Administrar como»](#BKMK_creds)

## <a name="provide-credentials-with-the-manage-as-command"></a><a name=BKMK_creds></a>Proporciona credenciales con el comando «Administrar como»
A medida que agregue servidores remotos a Administrador del servidor, algunos de los servidores que agregue pueden requerir credenciales de cuenta de usuario diferentes para tener acceso a ellos o administrarlos. Para especificar las credenciales de un servidor administrado que sean diferentes de las que usa para iniciar sesión en el equipo en el que se ejecuta Administrador del servidor, use el comando **administrar como** después de agregar un servidor a administrador del servidor, al que se puede tener acceso haciendo clic con el botón derecho en la entrada de un servidor administrado en el icono **servidores** de la Página principal de un rol o grupo. Al hacer clic en **Administrar como** se abre el cuadro de diálogo **Windows Security**, en el que puedes proporcionar un nombre de usuario que tenga derechos de acceso en el servidor administrado, en uno de los formatos siguientes.

-   *Nombre de usuario*

-   *Nombre de usuario*@example.domain.com

-   *Dominio* \\ de *Nombre de usuario*

El cuadro de diálogo **seguridad de Windows** que abre el comando **administrar como** no puede aceptar credenciales de tarjeta inteligente. no se admite proporcionar credenciales de tarjeta inteligente a través de Administrador del servidor. Las credenciales que se proporcionan para un servidor administrado mediante el comando **administrar como** se almacenan en caché y se conservan mientras se administra el servidor con el mismo equipo en el que se está ejecutando actualmente administrador del servidor, o siempre que no se sobrescriban especificando credenciales en blanco o diferentes para el mismo servidor. Si exporta la configuración de Administrador del servidor a otros equipos, o configura el perfil de dominio para que sea móvil para permitir que se use la configuración de Administrador del servidor en otros equipos, las credenciales de **administrar como** para los servidores del grupo de servidores no se almacenan en el perfil móvil. Administrador del servidor los usuarios deben agregarlas en cada equipo desde el que deseen ser administrados.

Después de agregar servidores para administrar siguiendo los procedimientos de este tema, pero antes de que uses el comando **Administrar como** para especificar credenciales alternativas que se podrían requerir para administrar un servidor que ya agregaste, se pueden mostrar los siguientes errores de estado de manejabilidad en el servidor:

-   Error de resolución de destino de Kerberos

-   Error de autenticación de Kerberos

-   En línea: acceso denegado

> [!NOTE]
> Los roles y las características que no admiten el comando **administrar como** incluyen servicios de escritorio remoto (RDS) y el servidor de administración de direcciones IP (IPAM). Si no puede administrar el servidor RDS o IPAM remoto con las mismas credenciales que usa en el equipo en el que se ejecuta Administrador del servidor, intente agregar la cuenta que usa normalmente para administrar estos servidores remotos en el grupo de administradores del equipo que ejecuta Administrador del servidor. A continuación, inicie sesión en el equipo que ejecuta Administrador del servidor con la cuenta que usa para administrar el servidor remoto que ejecuta rdS o IPAM.

## <a name="add-servers-to-manage"></a><a name=BKMK_add></a>Agregar servidores para administrar
Puede agregar servidores a Administrador del servidor para administrar mediante cualquiera de los tres métodos del cuadro de diálogo **agregar servidores** .

-   **Active Directory Domain Services** agregar servidores para administrar que Active Directory encuentra en el mismo dominio que el equipo local.

-   **Entrada de  Sistema de nombres de dominio (DNS)** Buscar servidores para administrar por nombre de equipo o dirección IP.

-   **Importar varios servidores** Especifique varios servidores para importar en un archivo que contenga los servidores enumerados por nombre de equipo o dirección IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Para agregar servidores en el grupo de servidores

1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

    -   En la pantalla **Inicio** de Windows, haga clic en el icono de administrador del servidor.

2.  En el menú **administrar** , haga clic en **agregar servidores**.

3.  Realice una de las acciones siguientes.

    -   En la pestaña **Active** Directory, seleccione los servidores que se encuentran en el dominio actual. Mientras selecciona, presione **Ctrl** para seleccionar varios servidores. Haga clic en el botón de flecha derecha para trasladar los servidores seleccionados a la lista **seleccionada** .

    -   En la pestaña **DNS**, escriba los primeros caracteres de un nombre de equipo o dirección IP y, a continuación, presione **Entrar** o haga clic en **Buscar**. Seleccione los servidores que desea agregar y, a continuación, haga clic en el botón de flecha derecha.

    -   En la pestaña **importar** , busque un archivo de texto que contenga los nombres DNS o las direcciones IP de los equipos que desea agregar, un nombre o una dirección IP por línea.

4.  Cuando haya terminado de agregar nombres, haga clic en **Aceptar**.

### <a name="add-and-manage-servers-in-workgroups"></a>Agregar y administrar servidores en grupos de trabajo
Aunque la adición de servidores que se encuentran en grupos de trabajo a Administrador del servidor puede ser correcta, una vez agregados, la columna **capacidad de administración** del icono **servidores** de una página de rol o grupo que incluye un servidor de grupo de trabajo puede mostrar **credenciales no válidas** que se producen al intentar conectarse o recopilar datos del servidor de grupo de trabajo remoto.

Se pueden producir estos errores o unos similares en las condiciones siguientes.

-   El servidor administrado está en el mismo grupo de trabajo que el equipo que ejecuta Administrador del servidor.

-   El servidor administrado se encuentra en un grupo de trabajo diferente del equipo en el que se ejecuta Administrador del servidor.

-   Uno de los equipos está en un grupo de trabajo, mientras que el otro está en un dominio.

-   El equipo que ejecuta Administrador del servidor se encuentra en un grupo de trabajo y los servidores remotos administrados se encuentran en una subred diferente.

-   Ambos equipos están en dominios, pero no hay ninguna relación de confianza entre los dos dominios.

-   Ambos equipos están en dominios, pero la relación de confianza entre los dos dominios es unidireccional.

-   Se ha agregado el servidor que quieres administrar mediante su dirección IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Para agregar servidores de grupo de trabajo remotos en el Administrador del servidor

1.  En el equipo que ejecuta Administrador del servidor, agregue el nombre del servidor de grupo de trabajo a la lista **TrustedHosts** . Este es un requisito de la autenticación NTLM. Para agregar un equipo a una lista existente de hosts de confianza, agrega el parámetro `Concatenate` al comando. Por ejemplo, para agregar el equipo `Server01` a una lista existente de hosts de confianza, utilice el comando siguiente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determine si el servidor de grupo de trabajo que desea administrar se encuentra en la misma subred que el equipo en el que se ejecuta Administrador del servidor.

    Si los dos equipos están en la misma subred o el perfil de red del servidor del grupo de trabajo está establecido en **privado** en el **centro de redes y recursos compartidos**, vaya al paso siguiente.

    Si no están en la misma subred o el perfil de red del servidor del grupo de trabajo no está establecido en **privado**, en el servidor de grupo de trabajo, cambie la configuración de entrada de **administración remota de Windows (http de entrada)** en firewall de Windows para permitir explícitamente conexiones desde equipos remotos agregando los nombres de equipo en la ficha **equipos** del cuadro de diálogo **propiedades** de la configuración.

3.  > [!IMPORTANT]
    > La ejecución de cmdlet en este paso invalida las medidas de Control de cuentas de usuario (UAC) que evitan la ejecución de procesos elevados en equipos del grupo de trabajo a menos que la cuenta predefinida de Administrador o la cuenta del Sistema esté ejecutando los procesos. El cmdlet permite que los miembros del grupo Administradores puedan manejar el servidor del grupo de trabajo sin iniciar sesión con la cuenta predefinida de Administrador. Permitir que otros usuarios administren el servidor del grupo de trabajo puede reducir la seguridad; sin embargo, esto es más seguro que proporcionar credenciales de la cuenta predefinida de administrador a varias personas para administrar el servidor del grupo de trabajo.

    Para invalidar las restricciones UAC en los procesos elevados en ejecución en los equipos del grupo de trabajo, cree una entrada del Registro llamada **LocalAccountTokenFilterPolicy** en el servidor de grupo de trabajo mediante la ejecución del siguiente cmdlet.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  En el equipo en el que se ejecuta Administrador del servidor, abra la página **todos los servidores** .

5.  Si el equipo que ejecuta Administrador del servidor y el servidor de grupo de trabajo de destino están en el mismo grupo de trabajo, vaya al último paso. Si los dos equipos no se encuentran en el mismo grupo de trabajo, haga clic con el botón secundario en el grupo de trabajo de destino en el icono **Servidores** y, a continuación, haga clic en **Administrar como**.

6.  Inicie sesión en el servidor del grupo de trabajo con la cuenta predefinida de administrador del servidor del grupo de trabajo.

7.  Para comprobar que Administrador del servidor puede conectarse a los datos del servidor de grupo de trabajo y recopilarlos, actualice la página **todos los servidores** y, a continuación, vea el estado de capacidad de administración del servidor de grupo de trabajo.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Para agregar servidores remotos cuando el Administrador del servidor se está ejecutando en un equipo del grupo de trabajo

1.  En el equipo que ejecuta Administrador del servidor, agregue servidores remotos a la lista **TrustedHosts** del equipo local en una sesión de Windows PowerShell. Para agregar un equipo a una lista existente de hosts de confianza, agrega el parámetro `Concatenate` al comando. Por ejemplo, para agregar el equipo `Server01` a una lista existente de hosts de confianza, utilice el comando siguiente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determine si el servidor que desea administrar se encuentra en la misma subred que el equipo del grupo de trabajo en el que se ejecuta Administrador del servidor.

    Si los dos equipos están en la misma subred o el perfil de red del equipo del grupo de trabajo está establecido en **privado** en el **centro de redes y recursos compartidos**, vaya al paso siguiente.

    Si no están en la misma subred o el perfil de red del equipo del grupo de trabajo no está configurado como **privado**, en el equipo del grupo de trabajo que ejecuta administrador del servidor, cambie la configuración de **administración remota de Windows (http de entrada)** en firewall de Windows para permitir explícitamente conexiones desde equipos remotos agregando los nombres de equipo en la ficha **equipos** del cuadro de diálogo **propiedades** de la configuración

3.  En el equipo en el que se ejecuta Administrador del servidor, abra la página **todos los servidores** .

4.  para comprobar que Administrador del servidor puede conectarse a los datos del servidor remoto y recopilarlos, actualice la página **todos los servidores** y, a continuación, vea el estado de capacidad de administración del servidor remoto. Si el icono **Servidores** aún muestra un error de capacidad de administración para el servidor remoto, vaya al siguiente paso.

5.  Cierre la sesión del equipo en el que está ejecutando Administrador del servidor y, a continuación, vuelva a iniciar sesión con la cuenta predefinida Administrador. Repita el paso anterior para comprobar que Administrador del servidor puede conectarse a los datos del servidor remoto y recopilarlos.

Si ha seguido los procedimientos de esta sección y sigue teniendo problemas al administrar equipos de grupo de trabajo o a administrar otros equipos desde equipos del grupo de trabajo, consulte [about_remote_Troubleshooting](/previous-versions/dd347642(v=technet.10)) en el sitio web de Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Agregar y administrar servidores en clústeres
Puede usar Administrador del servidor para administrar servidores que están en clústeres de conmutación por error (también denominados clústeres de servidores o MSCS). Los servidores que están en clústeres de conmutación por error (si los nodos del clúster son físicos o virtuales) tienen algunos comportamientos únicos y limitaciones de administración en Administrador del servidor.

-   Los servidores físicos y virtuales de los clústeres se agregan automáticamente a Administrador del servidor cuando se agrega un servidor del clúster a Administrador del servidor. Del mismo modo, cuando quite un servidor en clúster de Administrador del servidor, se le pedirá que quite otros servidores del clúster.

-   Administrador del servidor no muestra los datos de los servidores virtuales en clúster, porque los datos son dinámicos y son idénticos a los datos del servidor en el que se hospeda el nodo en clúster virtual. Puedes seleccionar el servidor que hospeda el servidor virtual para ver sus datos.

-   Si agrega un servidor a Administrador del servidor mediante el nombre de objeto del clúster virtual del servidor, el nombre del objeto virtual se muestra en Administrador del servidor en lugar del nombre del servidor físico (esperado).

-   No puedes instalar roles y características en un servidor virtual en clúster.

## <a name="see-also"></a>Consulte también
[Administrador del servidor](server-manager.md) 
 [crear y administrar grupos de servidores](create-and-manage-server-groups.md)