---
title: Implementar una infraestructura de red definida por software mediante scripts
description: En este tema se explica cómo implementar una infraestructura de red definida por software de Microsoft (SDN) mediante scripts en Windows Server 2019 y 2016.
manager: grcusanz
ms.topic: how-to
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 706077c7ebd260f0a497568935fb94718408a655
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716471"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implementación de una infraestructura de red definida por software con scripts

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema, se implementa una infraestructura de red definida por software de Microsoft (SDN) mediante scripts. La infraestructura incluye un controlador de red de alta disponibilidad (HA), un/MUX de Load Balancer software de alta disponibilidad (SLB), redes virtuales y listas de Access Control asociadas (ACL). Además, otro script implementa una carga de trabajo de inquilinos para que pueda validar la infraestructura de SDN.

Si desea que las cargas de trabajo de inquilinos se comuniquen fuera de sus redes virtuales, puede configurar las reglas NAT de SLB, los túneles de puerta de enlace de sitio a sitio o el reenvío de capa 3 para enrutar entre cargas de trabajo virtuales y físicas.

También puede implementar una infraestructura de SDN mediante Virtual Machine Manager (VMM). Para obtener más información, consulte [configuración de una infraestructura de red definida por software (SDN) en el tejido de VMM](/system-center/vmm/deploy-sdn).

## <a name="pre-deployment"></a>Anterior a la implementación

> [!IMPORTANT]
> Antes de comenzar la implementación, debe planear y configurar los hosts y la infraestructura de red física. Para más información, consulta [Planeación de una infraestructura de red definida por software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

Todos los hosts de Hyper-V deben tener instalado Windows Server 2019 o 2016.

## <a name="deployment-steps"></a>Pasos de implementación
Para empezar, configure el conmutador virtual de Hyper-v (servidores físicos) y la asignación de direcciones IP. Se puede usar cualquier tipo de almacenamiento que sea compatible con Hyper-V, compartido o local.

### <a name="install-host-networking"></a>Instalar redes de host

1. Instale los controladores de red más recientes disponibles para el hardware de la NIC.

2. Instale el rol de Hyper-V en todos los hosts (para obtener más información, consulte [Introducción a Hyper-v en Windows Server 2016](../../../virtualization/hyper-v/get-started/get-started-with-hyper-v-on-windows.md).

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```

3. Cree el conmutador virtual de Hyper-V.<p>Use el mismo nombre de conmutador para todos los hosts, por ejemplo, **sdnSwitch**. Configure al menos un adaptador de red o, si usa SET, configure al menos dos adaptadores de red. La propagación de entrada máxima se produce cuando se usan dos NIC.

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```

   > [!TIP]
   > Puede omitir los pasos 4 y 5 si tiene NIC de administración independientes.

3. Consulte el tema de planeación ([planear una infraestructura de red definida por software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el administrador de la red para obtener el identificador de VLAN de la VLAN de administración. Conecte el vNIC de administración del conmutador virtual recién creado a la VLAN de administración. Este paso se puede omitir si el entorno no usa etiquetas VLAN.

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```

4. Consulte el tema de planeación ([planear una infraestructura de red definida por software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el administrador de red para usar las asignaciones de IP estáticas o DHCP para asignar una dirección IP a los VNIC de administración del vSwitch recién creado. En el ejemplo siguiente se muestra cómo crear una dirección IP estática y asignarla al vNIC de administración del vSwitch:

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```

5. Opta Implementar una máquina virtual para hospedar Active Directory Domain Services ([instalar Active Directory Domain Services (nivel 100)](../../../identity/ad-ds/deploy/install-active-directory-domain-services--level-100-.md) y un servidor DNS.

    a. Conecte la máquina virtual del servidor de Active Directory/DNS a la VLAN de administración:

    ```PowerShell
    Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True
    ```

   b. Instale Active Directory Domain Services y DNS.

   > [!NOTE]
   > La controladora de red admite certificados de Kerberos y X. 509 para la autenticación. En esta guía se usan los dos mecanismos de autenticación para propósitos diferentes (aunque solo se requiere uno).

6. Unir todos los hosts de Hyper-V al dominio. Asegúrese de que la entrada del servidor DNS para el adaptador de red que tiene asignada una dirección IP a la red de administración apunta a un servidor DNS que pueda resolver el nombre de dominio.

   ```PowerShell
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>
   ```

   a. Haga clic con el botón secundario en **Inicio**, seleccione **sistema** y, a continuación, haga clic en **Cambiar configuración**.
   b. Haga clic en **Cambiar**.
   c. Haga clic en **dominio** y especifique el nombre de dominio.  "" "" d. Haga clic en **OK**.
   e. Escriba las credenciales de nombre de usuario y contraseña cuando se le solicite.
   f. Reinicie el servidor.

### <a name="validation"></a>Validación

Siga los pasos siguientes para validar que la red de host está configurada correctamente.

1. Asegúrese de que el conmutador de máquina virtual se ha creado correctamente:

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```

2. Compruebe que el vNIC de administración en el conmutador de máquina virtual está conectado a la VLAN de administración:

   > [!NOTE]
   > Solo es relevante si el tráfico de administración y de inquilinos comparte la misma NIC.

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Validar todos los hosts de Hyper-V y los recursos de administración externos, por ejemplo, los servidores DNS.<p>Asegúrese de que son accesibles a través de ping mediante su dirección IP de administración y/o el nombre de dominio completo (FQDN).

   ```
   ping <Hyper-V Host IP>
   ping <Hyper-V Host FQDN>
   ```

4. Ejecute el siguiente comando en el host de implementación y especifique el FQDN de cada host de Hyper-V para asegurarse de que las credenciales de Kerberos usadas proporcionan acceso a todos los servidores.

   ```
   winrm id -r:<Hyper-V Host FQDN>
   ```

### <a name="run-sdn-express-scripts"></a>Ejecutar scripts de SDN Express

1. Vaya al [repositorio de github de SDN de Microsoft](https://github.com/Microsoft/SDN.git) para obtener los archivos de instalación.

2. Descargue los archivos de instalación del repositorio en el equipo de implementación designado. Haga clic en **clonar o descargar** y, a continuación, haga clic en **Descargar zip**.

   > [!NOTE]
   > El equipo de implementación designado debe ejecutar Windows Server 2016 o versiones posteriores.

3. Expanda el archivo zip y copie la carpeta **SDNExpress** en la carpeta del equipo de implementación `C:\` .

4. Comparta la `C:\SDNExpress` carpeta como "**SDNExpress**" con permiso para que **todos los usuarios** **lean y escriban**.

5. Vaya a la carpeta `C:\SDNExpress`.<p>Verá las siguientes carpetas:

   | Nombre de carpeta | Descripción |
   |--|--|
   | AgentConf | Contiene copias actualizadas de los esquemas de OVSDB que usa el agente de host de SDN en cada host de Hyper-V de Windows Server 2016 para programar directivas de red. |
   | Certificados | Ubicación compartida temporal para el archivo de certificado de NC. |
   | Imágenes | Vacío, coloque la imagen de vhdx de Windows Server 2016 aquí |
   | Herramientas | Utilidades para la solución de problemas y la depuración.  Se copia en los hosts y las máquinas virtuales.  Se recomienda colocar Monitor de red o Wireshark aquí para que esté disponible si es necesario. |
   | Scripts | Scripts de implementación.<p> - **SDNExpress.ps1**<br>Implementa y configura el tejido, incluidas las máquinas virtuales de la controladora de red, las máquinas virtuales del MUX de SLB, los grupos de puerta de enlace y las máquinas virtuales de puerta de enlace de HNV correspondientes a los grupos.<br /> - **FabricConfig.psd1**<br>Una plantilla de archivo de configuración para el script SDNExpress. Lo personalizará para su entorno.<br /> - **SDNExpressTenant.ps1**<br>Implementa una carga de trabajo de inquilino de ejemplo en una red virtual con una VIP de carga equilibrada.<br>También aprovisiona una o más conexiones de red (IPSec S2S VPN, GRE y L3) en las puertas de enlace perimetrales del proveedor de servicios que están conectadas a la carga de trabajo de inquilino creada anteriormente. Las puertas de enlace de IPSec y GRE están disponibles para la conectividad a través de la dirección IP VIP correspondiente y la puerta de enlace de reenvío L3 en el grupo de direcciones correspondiente.<br>Este script se puede utilizar también para eliminar la configuración correspondiente con una opción de deshacer.<br /> - **TenantConfig.psd1**<br>Un archivo de configuración de plantilla para la carga de trabajo del inquilino y la configuración de puerta de enlace S2S.<br /> - **SDNExpressUndo.ps1**<br>Limpia el entorno de tejido y lo restablece a un estado de inicio.<br /> - **SDNExpressEnterpriseExample.ps1**<br>Aprovisiona uno o más entornos de sitio de empresa con una puerta de enlace de acceso remoto y, opcionalmente, una máquina virtual de empresa correspondiente por sitio. Las puertas de enlace empresariales de IPSec o GRE se conectan a la dirección IP VIP correspondiente de la puerta de enlace del proveedor de servicios para establecer los túneles S2S. La puerta de enlace de reenvío L3 se conecta a través de la dirección IP del mismo nivel correspondiente. <br> Este script se puede utilizar también para eliminar la configuración correspondiente con una opción de deshacer.<br /> - **EnterpriseConfig.psd1**<br>Un archivo de configuración de plantilla para la puerta de enlace de sitio a sitio de empresa y la configuración de máquina virtual de cliente. |
   | TenantApps | Archivos usados para implementar cargas de trabajo de inquilino de ejemplo. |

6. Compruebe que el archivo VHDX de Windows Server 2016 está en la carpeta **images** .

7. Personalice el archivo SDNExpress\scripts\FabricConfig.psd1 cambiando el **<< reemplazar >>** etiquetas con valores específicos para que se adapten a la infraestructura del laboratorio, incluidos los nombres de host, los nombres de dominio, los nombres de usuario y las contraseñas, y la información de red de las redes que aparecen en el tema Planificación de la red.

8. Cree un registro de host A en DNS para NetworkControllerRestName (FQDN) y NetworkControllerRestIP.

9. Ejecute el script como un usuario con credenciales de administrador de dominio:

   ```
   SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose
   ```

10. Para deshacer todas las operaciones, ejecute el siguiente comando:

   ```
    SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose
   ```


#### <a name="validation"></a>Validación

Suponiendo que el script de SDN Express se ejecutó hasta completarse sin informar de ningún error, puede realizar el siguiente paso para asegurarse de que los recursos del tejido se han implementado correctamente y están disponibles para la implementación de inquilinos.

Use [herramientas de diagnóstico](../troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md) para asegurarse de que no hay ningún error en los recursos de tejido de la controladora de red.

   ```
   Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>
   ```


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implementación de una carga de trabajo de inquilino de ejemplo con el equilibrador de carga de software

Ahora que se han implementado los recursos del tejido, puede validar la implementación de SDN de un extremo a otro mediante la implementación de una carga de trabajo de inquilino de ejemplo. Esta carga de trabajo de inquilino consta de dos subredes virtuales (nivel de base de datos y capa Web) protegidas a través de reglas de lista de Access Control (ACL) mediante el Firewall distribuido de SDN. La subred virtual del nivel Web es accesible a través de SLB/MUX mediante una dirección IP virtual (VIP). El script implementa automáticamente dos máquinas virtuales de nivel Web y una máquina virtual de nivel de base de datos y las conecta a las subredes virtuales.

1. Personalice el archivo SDNExpress\scripts\TenantConfig.psd1 cambiando el **<< reemplazar >>** etiquetas con valores específicos (por ejemplo: nombre de la imagen de VHD, nombre de REST de la controladora de red, nombre de vSwitch, etc., como se ha definido anteriormente en el archivo FabricConfig.psd1)

2. Ejecute el script. Por ejemplo:

   ```
   SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose
   ```

3. Para deshacer la configuración, ejecute el mismo script con el parámetro **Undo** . Por ejemplo:

   ```
   SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose
   ```

#### <a name="validation"></a>Validación

Para validar que la implementación de inquilinos se realizó correctamente, haga lo siguiente:

1. Inicie sesión en la máquina virtual de nivel de base de datos e intente hacer ping a la dirección IP de una de las máquinas virtuales de nivel Web (Asegúrese de que el Firewall de Windows está desactivado en las máquinas virtuales de nivel Web).

2. Compruebe si hay errores en los recursos de inquilino de la controladora de red. Ejecute lo siguiente desde cualquier host de Hyper-V con conectividad de nivel 3 a la controladora de red:

   ```
   Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>
   ```

3. Para comprobar que el equilibrador de carga se está ejecutando correctamente, ejecute lo siguiente desde cualquier host de Hyper-V:

   ```
   wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   ```

   donde `<VIP IP address>` es la dirección IP VIP de nivel Web que configuró en el archivo TenantConfig.psd1.

   > [!TIP]
   > Busque la `VIPIP` variable en TenantConfig.psd1.

   Ejecútelo varias veces para ver el conmutador del equilibrador de carga entre las DIP disponibles. También puede observar este comportamiento mediante un explorador Web. Vaya a `<VIP IP address>/unique.htm`. Cierre el explorador y abra una nueva instancia de y vuelva a examinar. Verá la página azul y la página verde alternativa, excepto cuando el explorador almacena en caché la página antes de que se agote el tiempo de espera de la caché.
