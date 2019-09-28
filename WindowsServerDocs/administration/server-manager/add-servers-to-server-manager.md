---
title: Add Servers to Server Manager
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aab895f2-fe4d-4408-b66b-cdeadbd8969e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.localizationpriority: medium
ms.date: 02/01/2018
ms.openlocfilehash: ad30a8f1c4c1e0aa317512eb68fffbd76413175b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383260"
---
# <a name="add-servers-to-server-manager"></a>Add Servers to Server Manager

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server puede administrar varios servidores remotos mediante una sola consola del Administrador de servidores. Los servidores que deseas administrar con el Administrador de servidores pueden ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Ten en cuenta que no puede administrar una versión más reciente de Windows Server con una versión anterior del Administrador de servidores.

En este tema se describe cómo agregar servidores al grupo de servidores del Administrador de servidores.

> [!NOTE]
> En nuestras pruebas, pueden usarse el Administrador de servidores en Windows Server 2012 y versiones posteriores de Windows Server para administrar hasta 100 servidores configurados con una carga de trabajo típica. El número de servidores que se pueden administrar con una sola consola del Administrador de servidores puede variar según la cantidad de datos que solicite de los servidores administrados así como los recursos de hardware y de red disponibles para el equipo que ejecuta el Administrador de servidores. Como la cantidad de datos que desea mostrar se acerca a la capacidad de los recursos del equipo, puede experimentar respuestas lentas del administrador de servidores y retrasos en la finalización de las actualizaciones. Para ayudar a aumentar el número de servidores que se pueden administrar mediante el Administrador de servidores, se recomienda limitar los datos de eventos que el Administrador de servidores obtiene de los servidores administrados utilizando la configuración del cuadro de diálogo **Configurar datos de eventos** . Configurar datos de eventos se puede abrir desde el menú **Tareas** del icono **Eventos** . Si tienes que administrar un número de nivel de empresa de servidores de su organización, se recomienda evaluar productos en la [suite Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).
> 
> El Administrador de servidores solo puede recibir estado en línea o sin conexión de los servidores que ejecutan Windows Server 2003. Aunque puedes usar el Administrador de servidores para realizar las tareas de administración en los servidores que ejecutan Windows Server 2008 R2 o Windows Server 2008, no puedes agregar roles y características a los servidores que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.
> 
> El Administrador de servidores no se puede usar para administrar una versión más reciente del sistema operativo Windows Server. El Administrador de servidores que se ejecuta en Windows Server 2012 R2, Windows Server 2012, Windows 8.1 o Windows 8 no puede usarse para administrar los servidores que ejecutan Windows Server 2016.

En este tema se incluyen las siguientes secciones.

-   [Agregar servidores para administrar](#BKMK_add)

-   [Proporcionar credenciales con el comando administrar como](#BKMK_creds)

## <a name="BKMK_creds"></a>Proporcionar credenciales con el comando administrar como
Al agregar servidores remotos al Administrador de servidores, algunos de los servidores que agregue es posible que requieran diferentes credenciales de cuenta de usuario para tener acceso o administrarlos. Para especificar las credenciales para un servidor administrado que sean diferentes de las que se utilizan para iniciar sesión en el equipo en el que ejecuta el Administrador de servidores, usa el comando **Administrar como** después de agregar un servidor al Administrador de servidores, al que se accede haciendo clic en la entrada de un servidor administrado en la ventana **Servidores** de una página principal de rol o de grupo. Al hacer clic en **Administrar como** se abre el cuadro de diálogo **Windows Security** , en el que puedes proporcionar un nombre de usuario que tenga derechos de acceso en el servidor administrado, en uno de los formatos siguientes.

-   *Nombre de usuario.*

-   *Nombre de usuario*@example.domain.com

-   *Dominio*\\*Nombre de usuario*

El cuadro de diálogo **Seguridad de Windows** que se abre mediante el comando **Administrar como** no puede aceptar las credenciales de tarjeta inteligente; no se admite la especificación de credenciales de tarjetas inteligentes mediante el Administrador de servidores. Las credenciales que proporcione para un servidor administrado mediante el comando **Administrar como** se almacenan en caché y se conservan mientras administre el servidor utilizando el mismo equipo en el que se ejecute el Administrador de servidores, o mientras no las sobrescriba especificando credenciales en blanco o diferentes para el mismo servidor. Si exportas la configuración del Administrador de servidores a otros equipos o configura el perfil de su dominio para roaming para permitir que la configuración del Administrador de servidores se use en otros equipos, las credenciales de **Administrar como** de los servidores del grupo de servidores no se almacenan en el perfil de roaming. Los usuarios del Administrador de servidores deben agregarlas en cada equipo desde el que desean administrar.

Después de agregar servidores para administrar siguiendo los procedimientos de este tema, pero antes de que uses el comando **Administrar como** para especificar credenciales alternativas que se podrían requerir para administrar un servidor que ya agregaste, se pueden mostrar los siguientes errores de estado de manejabilidad en el servidor:

-   Error de resolución de destino de Kerberos

-   Error de autenticación de Kerberos

-   En línea: acceso denegado

> [!NOTE]
> Entre los roles y características que no admiten el comando **Administrar como** se incluyen Servicios de Escritorio remoto (RDS) y Servidor de administración de dirección IP (IPAM). Si no se puede administrar el servidor RDS o IPAM remoto usando las mismas credenciales que se usan en el equipo en el que se ejecuta el Administrador de servidores, intenta agregar la cuenta que se usa normalmente para administrar estos servidores remotos al grupo Administradores del equipo que está ejecutando el Administrador de servidores. A continuación, inicia sesión en el equipo que ejecuta el Administrador de servidores con la cuenta que usa para administrar el servidor remoto que ejecuta rdS o IPAM.

## <a name="BKMK_add"></a>Agregar servidores para administrar
Puedes agregar servidores al Administrador de servidores para administrar usando uno de los tres métodos del cuadro de diálogo **Agregar servidores** .

-   **Active Directory Domain Services** agrega servidores para administrar que Active Directory se encuentra en el mismo dominio que el equipo local.

-   **Entrada de  Sistema de nombres de dominio (DNS)** Buscar servidores para administrar por nombre de equipo o dirección IP.

-   **Importar servidores múltiples** Especifica varios servidores para importarlos en un archivo que contiene servidores que aparecen por nombre de equipo o dirección IP.

#### <a name="to-add-servers-to-the-server-pool"></a>Para agregar servidores en el grupo de servidores

1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

    -   En la pantalla **Inicio** de Windows, haz clic en la ventana Administrador de servidores.

2.  En el menú **Administrar**, haz clic en **Agregar servidores**.

3.  Realice una de las siguientes acciones:

    -   En la pestaña **Active Directory**, selecciona los servidores que se encuentran en el dominio actual. Mientras selecciona, presione **Ctrl** para seleccionar varios servidores. Haz clic en el botón de flecha derecha para mover los servidores seleccionados a la lista **seleccionada**.

    -   En la pestaña **DNS** , escriba los primeros caracteres de un nombre de equipo o dirección IP y, a continuación, presione **Entrar** o haga clic en **Buscar**. Selecciona los servidores que desea agregar y, a continuación, haz clic en el botón de flecha derecha.

    -   En la ficha **Importar**, busca un archivo de texto que contenga los nombres DNS o direcciones IP de los equipos que deseas agregar, un nombre o dirección IP por línea.

4.  Cuando haya terminado de agregar nombres, haga clic en **Aceptar**.

### <a name="add-and-manage-servers-in-workgroups"></a>Agregar y administrar servidores en grupos de trabajo
Aunque se realice correctamente la adición de servidores que se encuentran en grupos de trabajo al Administrador de servidores, después de agregarse, la columna **Capacidad de administración** de la ventana **Servidores** en una página de rol o grupo que incluye un servidor de grupo de trabajo, puedes mostrar errores del tipo **Credenciales no válidas** que se producen al intentar conectarse a o recopilar datos del servidor remoto de grupo de trabajo.

Se pueden producir estos errores o unos similares en las condiciones siguientes.

-   El servidor administrado está en el mismo grupo de trabajo que el equipo que ejecuta el Administrador de servidores.

-   El servidor administrado está en un grupo de trabajo diferente del equipo que ejecuta el Administrador de servidores.

-   Uno de los equipos está en un grupo de trabajo, mientras que el otro está en un dominio.

-   El equipo que ejecuta el Administrador de servidores está en un grupo de trabajo y los servidores remotos administrados se encuentran en una subred diferente.

-   Ambos equipos están en dominios, pero no hay ninguna relación de confianza entre los dos dominios.

-   Ambos equipos están en dominios, pero la relación de confianza entre los dos dominios es unidireccional.

-   Se ha agregado el servidor que quieres administrar mediante su dirección IP.

##### <a name="to-add-remote-workgroup-servers-to-server-manager"></a>Para agregar servidores de grupo de trabajo remotos en el Administrador del servidor

1.  En el equipo que ejecuta el Administrador de servidores, agrega el nombre del servidor de grupo de trabajo a la lista **TrustedHosts**. Este es un requisito de la autenticación NTLM. Para agregar un equipo a una lista existente de hosts de confianza, agrega el parámetro `Concatenate` al comando. Por ejemplo, para agregar el equipo `Server01` a una lista existente de hosts de confianza, utilice el comando siguiente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determina si el servidor de grupo de trabajo que deseas administrar está en la misma subred que el equipo en el que ejecuta el Administrador de servidores.

    Si los dos equipos se encuentran en la misma subred, o si se establece el perfil de red del servidor de grupo de trabajo en **Privado** en el **Centro de redes y recursos compartidos**, ve al paso siguiente.

    Si no se encuentran en la misma subred, o si no se establece el perfil de grupo de trabajo del servidor de red en **Privado**, en el servidor de grupo de trabajo, cambia la configuración de **Administración remota de Windows (HTTP de entrada)** entrante en el Firewall de Windows para permitir conexiones de forma explícita desde equipos remotos mediante la adición de los nombres de equipos en la pestaña **equipos** del cuadro de diálogo **Propiedades** de la configuración.

3.  > [!IMPORTANT]
    > La ejecución de cmdlet en este paso invalida las medidas de Control de cuentas de usuario (UAC) que evitan la ejecución de procesos elevados en equipos del grupo de trabajo a menos que la cuenta predefinida de Administrador o la cuenta del Sistema esté ejecutando los procesos. El cmdlet permite que los miembros del grupo Administradores puedan manejar el servidor del grupo de trabajo sin iniciar sesión con la cuenta predefinida de Administrador. Permitir que otros usuarios administren el servidor del grupo de trabajo puede reducir la seguridad; sin embargo, esto es más seguro que proporcionar credenciales de la cuenta predefinida de administrador a varias personas para administrar el servidor del grupo de trabajo.

    Para invalidar las restricciones UAC en los procesos elevados en ejecución en los equipos del grupo de trabajo, cree una entrada del Registro llamada **LocalAccountTokenFilterPolicy** en el servidor de grupo de trabajo mediante la ejecución del siguiente cmdlet.

    ```
    New-ItemProperty -Name LocalAccountTokenFilterPolicy -path HKLM:\SOFTWARE\Microsoft\Windows\Currentversion\Policies\System -propertytype DWord -value 1
    ```

4.  En el equipo en el que ejecuta el Administrador de servidores, abre la página **Todos los servidores**.

5.  Si el equipo que ejecuta el Administrador de servidores y el servidor del grupo de trabajo de destino se encuentra en el mismo grupo de trabajo, salta al último paso. Si los dos equipos no se encuentran en el mismo grupo de trabajo, haga clic con el botón secundario en el grupo de trabajo de destino en el icono **Servidores** y, a continuación, haga clic en **Administrar como**.

6.  Inicie sesión en el servidor del grupo de trabajo con la cuenta predefinida de administrador del servidor del grupo de trabajo.

7.  Comprueba si el Administrador de servidores es capaz de conectarse al servidor de grupo de trabajo y recopilar datos desde él actualizando la página **Todos los servidores** y, a continuación, viendo el estado de capacidad de administración del servidor de grupo de trabajo.

##### <a name="to-add-remote-servers-when-server-manager-is-running-on-a-workgroup-computer"></a>Para agregar servidores remotos cuando el Administrador del servidor se está ejecutando en un equipo del grupo de trabajo

1.  En el equipo que ejecuta el Administrador de servidores, agrega servidores remotos a la lista **TrustedHosts** del equipo local en una sesión de Windows PowerShell. Para agregar un equipo a una lista existente de hosts de confianza, agrega el parámetro `Concatenate` al comando. Por ejemplo, para agregar el equipo `Server01` a una lista existente de hosts de confianza, utilice el comando siguiente.

    ```
    Set-Item wsman:\localhost\Client\TrustedHosts Server01 -Concatenate -force
    ```

2.  Determina si el servidor que desea administrar está en la misma subred que el equipo del grupo de trabajo en el que ejecuta el Administrador de servidores.

    Si los dos equipos se encuentran en la misma subred o si el perfil de red del equipo del grupo de trabajo se establece en **Privado** en el **Centro de redes y recursos compartidos**, ve al paso siguiente.

    Si no se encuentran en la misma subred o si el perfil de red del equipo de grupo de trabajo no está establecido en **Privado**, en el equipo del grupo de trabajo que está ejecutando el Administrador de servidores, cambia la configuración de **Administración remota de Windows (HTTP de entrada)** entrante en Firewall de Windows para permitir explícitamente las conexiones desde equipos remotos añadiendo los nombres de equipos en la pestaña **equipos** del cuadro de diálogo de **Propiedades** de la configuración.

3.  En el equipo en el que ejecuta el Administrador de servidores, abre la página **Todos los servidores**.

4.  Comprueba si el Administrador de servidores es capaz de conectarse a y recopilar datos desde el servidor remoto actualizando la página **Todos los servidores** y, a continuación, viendo el estado de capacidad de administración del servidor remoto. Si el icono **Servidores** aún muestra un error de capacidad de administración para el servidor remoto, vaya al siguiente paso.

5.  Cierra la sesión en el equipo en el que ejecuta el Administrador de servidores y, a continuación, vuelve a iniciarla con la cuenta predefinida de administrador. Repite el paso anterior, para comprobar que el Administrador de servidores es capaz de conectarse a y recopilar datos del servidor remoto.

Si has seguido los procedimientos descritos en esta sección y sigues teniendo problemas para administrar los equipos del grupo de trabajo o administrar los demás equipos de los equipos del grupo de trabajo, consulta [about_remote_Troubleshooting](https://technet.microsoft.com/library/dd347642.aspx) en el sitio Web de Microsoft.

### <a name="add-and-manage-servers-in-clusters"></a>Agregar y administrar servidores en clústeres
Puedes usar el Administrador de servidores para administrar servidores que se encuentran en conmutación de clústeres por error (también denominados clústeres de servidores o MSCS). Los servidores que se encuentran en conmutación de clústeres por error (si los nodos del clúster son físicos o virtuales) tienen algunos comportamientos únicos y limitaciones de administración en el Administrador de servidores.

-   Los servidores físicos y virtuales en clústeres se agregan automáticamente al Administrador de servidores cuando un servidor del clúster se agrega al mismo. De forma similar, cuando se quita un servidor en clúster desde el Administrador de servidores, se le pide quitar otros servidores del clúster.

-   El Administrador de servidores no muestra datos de los servidores virtuales agrupados en clúster, porque los datos son dinámicos y son idénticos a los datos del servidor en el que se hospeda el nodo virtual en clúster. Puedes seleccionar el servidor que hospeda el servidor virtual para ver sus datos.

-   Si agregas un servidor al Administrador de servidores utilizando el nombre del objeto del clúster virtual del servidor, el nombre del objeto virtual se muestra en el Administrador de servidores en lugar del nombre de servidor físico (esperado).

-   No puedes instalar roles y características en un servidor virtual en clúster.

## <a name="see-also"></a>Vea también
[Administrador del servidor](server-manager.md)
[crear y administrar grupos de servidores](create-and-manage-server-groups.md)
