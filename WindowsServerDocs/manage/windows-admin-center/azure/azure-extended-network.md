---
title: Amplíe sus subredes locales a Azure mediante la red extendida de Azure
description: Amplíe sus subredes locales a Azure mediante la red extendida de Azure
ms.technology: manage
ms.topic: article
author: grcusanz
ms.author: grcusanz
ms.date: 12/17/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 29e370da31b02a475f0fdd5d7914348189ca616b
ms.sourcegitcommit: 6a245fefdf958bfc0aeb69f7a887d11a07bdcd23
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570360"
---
# <a name="extend-your-on-premises-subnets-into-azure-using-azure-extended-network"></a>Amplíe sus subredes locales a Azure mediante la red extendida de Azure

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

## <a name="overview"></a>Información general

Azure Extended Network permite expandir una subred local en Azure para permitir que las máquinas virtuales locales mantengan sus direcciones IP privadas locales originales al migrar a Azure.

La red se extiende mediante un túnel VXLAN bidireccional entre dos máquinas virtuales de Windows Server 2019 que actúan como aplicaciones virtuales, una que se ejecuta de forma local y otra en Azure, cada una de las cuales también está conectada a la subred que se va a extender.
Cada subred que se va a extender requiere un par de dispositivos. Se pueden extender varias subredes mediante varios pares.

> [!NOTE]
> La red extendida de Azure solo debe usarse para equipos que no pueden cambiar su dirección IP al migrar a Azure. Siempre es mejor cambiar la dirección IP y conectarla a una subred que exista totalmente en Azure, si es una opción.

## <a name="planning"></a>Planificación

Para preparar el uso de la red extendida de Azure, debe identificar qué subred quiere expandir y, a continuación, realizar los pasos siguientes:

### <a name="capacity-planning"></a>Planificación de capacidad

Puede ampliar hasta 250 direcciones IP mediante la red extendida de Azure. Puede esperar un rendimiento agregado de aproximadamente 700 Mbps, con cierta variabilidad según la velocidad de la CPU de las aplicaciones virtuales de red extendidas de Azure.

### <a name="configuration-in-azure"></a>Configuración en Azure

Antes de usar el centro de administración de Windows, debe realizar los siguientes pasos a través de Azure Portal:

1. Cree una red virtual en Azure que contenga al menos dos subredes, además de las subredes necesarias para la conexión de la puerta de enlace. Una de las subredes que cree debe usar la misma subred CIDR que la subred local que desea extender. La subred debe ser única en el dominio de enrutamiento para que no se superponga con ninguna subredes local.

2. Configure una puerta de enlace de red virtual para que use una conexión de sitio a sitio o ExpressRoute para conectar la red virtual a la red local.

3. Cree una máquina virtual de Windows Server 2019 en Azure que sea capaz de ejecutar la virtualización anidada. Se trata de una de las dos aplicaciones virtuales. Conecte la interfaz de red principal a la subred enrutable y la segunda interfaz de red a la subred extendida.

4. Inicie la máquina virtual, habilite el rol de Hyper-V y reinicie. Por ejemplo:

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

5. Cree dos conmutadores virtuales externos en la máquina virtual y conéctese a cada una de las interfaces de red. Por ejemplo:

    ```powershell
    New-VMSwitch -Name "External" -AllowManagementOS $true -NetAdapterName "Ethernet"
    New-VMSwitch -Name "Extended" -AllowManagementOS $true -NetAdapterName "Ethernet 2"
    ```

### <a name="on-premises-configuration"></a>Configuración local

También debe realizar alguna configuración manual en la infraestructura local, incluida la creación de una máquina virtual para que actúe como el dispositivo virtual local:

1. Asegúrese de que las subredes están disponibles en la máquina física en la que va a implementar la máquina virtual local (aplicación virtual). Esto incluye la subred que desea extender y una segunda subred que sea única y no se superponga a las subredes de la red virtual de Azure.

2. Cree una máquina virtual de Windows Server 2019 en cualquier hipervisor que admita la virtualización anidada. Este es el dispositivo virtual local. Se recomienda crear esto como una máquina virtual de alta disponibilidad en un clúster. Conecte un adaptador de red virtual a la subred enrutable y un segundo adaptador de red virtual a la subred extendida.

3. Inicie la máquina virtual y, a continuación, ejecute este comando desde una sesión de PowerShell en la máquina virtual para habilitar el rol de Hyper-V y reinicie la máquina virtual:

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

4. Ejecute los siguientes comandos en una sesión de PowerShell en la máquina virtual para crear dos conmutadores virtuales externos en la máquina virtual y conectar uno a cada una de las interfaces de red:

    ```powershell
    New-VMSwitch -Name "External" -AllowManagementOS $true -NetAdapterName "Ethernet"
    New-VMSwitch -Name "Extended" -AllowManagementOS $true -NetAdapterName "Ethernet 2"
    ```

### <a name="additional-prerequisites"></a>Requisitos previos adicionales

Si tiene un firewall entre la red local y Azure, debe configurarlo para permitir el enrutamiento asimétrico. Esto puede incluir pasos para deshabilitar la selección aleatoria del número de secuencia y habilitar la omisión del estado de TCP. Consulte la documentación del proveedor de Firewall para obtener información sobre estos pasos.

## <a name="deploy"></a>Implementar

La implementación se controla a través del centro de administración de Windows.

### <a name="install-and-configure-windows-admin-center"></a>Instalar y configurar el centro de administración de Windows

1. [Descargue e instale el centro de administración de Windows](../understand/windows-admin-center.md) en cualquier equipo capaz de ejecutar el centro de administración de Windows, excepto las dos aplicaciones virtuales que creó anteriormente.

2. En el centro de administración de Windows, seleccione **configuración** (en la esquina superior derecha de la página) > **extensiones**. Después, seleccione **extensiones** :

    ![Captura de pantalla que muestra la pestaña extensiones disponibles de configuración](../media/azure-extended-network/installed-extensions.png)

3. En la pestaña **extensiones disponibles** , seleccione **red extendida de Azure** y, después, seleccione **instalar**.

    Después de unos segundos, debería ver un mensaje que indica que la instalación se ha realizado correctamente.

4. [Conecte el centro de administración de Windows a Azure](azure-integration.md), si aún no lo ha hecho. Si omite este paso ahora, le pediremos que lo haga más adelante en el proceso.

5. En el centro de administración de Windows, vaya a **todas las conexiones**  >  **Agregar** y, a continuación, seleccione **Agregar** en el icono de **Windows Server** . Escriba el nombre del servidor (y las credenciales si es necesario) para el dispositivo virtual local.

    ![Captura de pantalla del centro de administración de Windows que muestra la herramienta de red extendida de Azure en Administrador del servidor en el dispositivo virtual local](../media/azure-extended-network/azure-extended-network.png).

6. Haga clic en **red extendida de Azure** para comenzar. La primera vez que se le presentará una introducción y un botón de configuración:

    ![Imagen](../media/azure-extended-network/azure-extended-network-intro.png)

### <a name="deploy-azure-extended-network"></a>Implementación de la red extendida de Azure

1. Haga clic en **configurar** para iniciar la configuración.

2. Haga clic en **siguiente** para continuar más allá de la información general.

3. En el panel **cargar paquete** , tendrá que descargar el paquete del agente de red extendido de Azure y cargarlo en el dispositivo virtual. Siga las instrucciones del panel.

    > [!IMPORTANT]
    > Desplácese hacia abajo si es necesario y haga clic en **cargar** antes de hacer clic en **siguiente: Extended-Network el programa de instalación**.

4. Seleccione el CIDR de subred de la red local que desea extender. La lista de subredes se lee desde el dispositivo virtual. Si no ha conectado el dispositivo virtual al conjunto de subredes correcto, no verá el CIDR de subred deseado en esta lista.

5. Haga clic en **siguiente** después de seleccionar la subred CIDR.

6. Seleccione la suscripción, el grupo de recursos y la red virtual que va a extender:

    ![Red de Azure](../media/azure-extended-network/azure-network.png)

    La región (ubicación de Azure) y la subred se seleccionan automáticamente. Seleccione siguiente: Extended-Network configuración de puerta de enlace para continuar.

7. Ahora se configurarán las aplicaciones virtuales. La puerta de enlace local debe tener su información rellenada automáticamente:

    ![puerta de enlace de red local](../media/azure-extended-network/on-premises-network-gateway.png)

    Si parece correcto, puede hacer clic en **siguiente**.

8. Para el dispositivo virtual de Azure, deberá seleccionar el grupo de recursos y la máquina virtual que se usarán:

    ![Puerta de enlace de red de Azure](../media/azure-extended-network/azure-network-gateway.png)

9. Después de seleccionar la máquina virtual, también tendrá que seleccionar la opción CIDR de la subred de Azure Extended-Network Gateway. A continuación, haga clic en **siguiente: implementar**.

10. Revise la información de Resumen y, a continuación, haga clic en **implementar** para iniciar el proceso de implementación. La implementación tardará aproximadamente 5-10 minutos. Una vez completada la implementación, verá el siguiente panel para administrar las direcciones IP extendidas y el Estado debería decir **correcto** :

    ![Instalación completada](../media/azure-extended-network/installation-complete.png)

## <a name="manage"></a>Administrar

Tendrá que configurar todas las direcciones IP a las que desea que se pueda tener acceso a través de la red extendida. Puede configurar hasta 250 direcciones para extender.
Para extender una dirección

1. Haga clic en **agregar direcciones IPv4** :

    ![Agregar direcciones IPv4](../media/azure-extended-network/add-ipv4-addresses.png)

2. Verá el control flotante **Agregar nuevas direcciones IPv4** a la derecha:

    ![Panel agregar direcciones IPv4](../media/azure-extended-network/add-ipv4-addresses-panel.png)

3. Use el botón **Agregar** para agregar manualmente una dirección. Las direcciones que agregue a las instalaciones locales serán accesibles para las direcciones de Azure que agregue a la lista de direcciones de Azure y viceversa.

4. La red extendida de Azure examina la red para detectar las direcciones IP y rellena las listas de sugerencias basadas en este análisis. Para ampliar estas direcciones, debe usar la lista desplegable y activar la casilla junto a la dirección detectada. No se detectarán todas las direcciones. Opcionalmente, puede usar el botón **Agregar** para agregar manualmente las direcciones que no se detectan automáticamente.

    ![Agregar panel de direcciones IPv4 con información](../media/azure-extended-network/add-ipv4-addresses-panel-filled.png)

5. Haga clic en **submit (enviar** ) cuando haya terminado. Verá que el estado cambia a **actualizando** , después **progresando** y, finalmente, a **Aceptar** una vez completada la configuración.

    Ahora se han ampliado las direcciones. Use el botón agregar direcciones IPv4 para agregar direcciones adicionales en cualquier momento. Si una dirección IP ya no se usa en ninguno de los extremos de la red extendida, active la casilla situada junto a ella y seleccione quitar direcciones IPv4.

Si ya no desea usar la red extendida de Azure, haga clic en el botón **quitar red extendida de Azure** . Se desinstalará el agente de las dos aplicaciones virtuales y se quitarán las direcciones IP extendidas. La red dejará de extenderse. Tendrá que volver a ejecutar el programa de instalación después de quitarlo, si desea empezar a usar la red extendida de nuevo.

## <a name="troubleshooting"></a>Solución de problemas

Si recibe un error durante la implementación de la red extendida de Azure, siga estos pasos:

1. Compruebe que las dos aplicaciones virtuales usan Windows Server 2019.

2. Compruebe que no está ejecutando el centro de administración de Windows en una de las aplicaciones virtuales. Se recomienda ejecutar el centro de administración de Windows desde una red local.

3. Asegúrese de que puede tener acceso remoto a la máquina virtual local desde la puerta de enlace del centro de administración de Windows mediante `enter-pssession` .

4. Si hay un firewall entre Azure y el entorno local, confirme que está configurado para permitir el tráfico UDP en el puerto seleccionado (el valor predeterminado es 4789). Use una herramienta como ctsTraffic para configurar un agente de escucha y un remitente. Compruebe que el tráfico se puede enviar en ambas direcciones en el puerto especificado.

5. Use pktmon para comprobar que los paquetes se envían y reciben según lo previsto. Ejecute pktmon en cada aplicación virtual:

    ```powershell
    Pktmon start –etw
    ```

6. Ejecute la configuración de red extendida de Azure y luego detenga el seguimiento:

    ```powershell
    Pktmon stop
    Netsh trace convert input=<path to pktmon etl file>
    ```

7. Abra el archivo de texto que se produce a partir de cada aplicación virtual y busque el tráfico UDP en el puerto especificado (el valor predeterminado es 4789). Si ve tráfico enviado desde el dispositivo virtual local, pero no recibido por el dispositivo virtual de Azure, debe comprobar el enrutamiento y el Firewall entre los dispositivos. Si ve tráfico enviado desde el entorno local a Azure, debería ver que el dispositivo virtual de Azure envía un paquete en respuesta. Si el dispositivo virtual local nunca recibe ese paquete, debe comprobar que el enrutamiento sea correcto y que no haya un firewall que bloquee el tráfico entre ellos.

### <a name="diagnosing-the-data-path-after-initial-configuration"></a>Diagnosticar la ruta de acceso de datos después de la configuración inicial

Una vez configurada la red extendida de Azure, los problemas adicionales que pueda encontrar suelen deberse a que los firewalls bloquean el tráfico o a que se superó la MTU si el error es intermitente.

1. Compruebe que ambos dispositivos virtuales estén en funcionamiento.

2. Compruebe que el agente de red extendido de Azure se está ejecutando en cada uno de los dispositivos virtuales:

    ```powershell
    get-service extnwagent
    ```

3. Si el estado no es en ejecución, puede iniciarlo con:

    ```powershell
    start-service extnwagent
    ```

4. Use packetmon como se describe en el paso 5 anterior mientras se envía tráfico entre dos máquinas virtuales de la red extendida para comprobar que las aplicaciones virtuales reciben el tráfico, que se envía a través del puerto UDP configurado y, a continuación, lo recibe y reenvía el otro dispositivo virtual.
