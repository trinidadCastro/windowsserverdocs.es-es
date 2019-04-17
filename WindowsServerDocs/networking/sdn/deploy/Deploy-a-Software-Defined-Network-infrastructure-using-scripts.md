---
title: Implementar una infraestructura de red definido de Software mediante Scripts
description: En este tema se explica cómo implementar una infraestructura de red de definido de Software de Microsoft (SDN) mediante scripts en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implementar una infraestructura de red definido de Software mediante Scripts

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema se explica cómo implementar una infraestructura de red de definido de Software de Microsoft (SDN) mediante scripts. La infraestructura incluye un controlador de red (HA) altamente disponible, una alta disponibilidad Software carga equilibrado (SLB) / MUX, redes virtuales y se asocian listas de Control de acceso (ACL). Además, otro script implementa una carga de trabajo de inquilino para validar su infraestructura SDN.  
  
Si quieres que las cargas de trabajo de inquilino comunicarse fuera de sus redes virtuales, puede configurar las reglas SLB NAT, túneles de puerta de enlace de sitio a sitio o reenvío de capa 3 para enrutar entre cargas de trabajo físicas y virtuales.  
  
También puedes implementar una infraestructura SDN con Virtual Machine Manager (VMM). Para obtener más información, consulta [configurar una infraestructura de red de definido de Software (SDN) en la estructura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  
  
## <a name="pre-deployment"></a>Anteriores a la implementación  
  
> [!IMPORTANT]  
> Antes de comenzar la implementación, debes planear y configurar la infraestructura de red física y hosts. Para obtener más información, consulta [planear una infraestructura de red definido de Software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Todos los host de Hyper-V deben tener instalado Windows Server 2016.  
  
## <a name="deployment-steps"></a>Pasos de implementación  
Inicio mediante la configuración del host de Hyper-V conmutador virtual de Hyper-V (servidores físicos) y asignación de direcciones IP. Puede usar cualquier tipo de almacenamiento que sea compatible con Hyper-V, compartida o local.  
### <a name="install-host-networking"></a>Instalar el host de red  
1. Instala a los controladores más recientes de red disponibles para el hardware NIC.  
2. Instalar el rol de Hyper-V en todos los hosts (para obtener más información, consulta [comenzar con Hyper-V en Windows Server 2016](https://technet.microsoft.com/en-us/library/mt126159.aspx).   
  
   Desde el símbolo de Windows PowerShellcommand con privilegios elevados:  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    Un archivo. Crear el conmutador virtual de Hyper-V (usa el mismo nombre de conmutador para todos los hosts. Por ejemplo: **sdnSwitch**). Configurar al menos un adaptador de red, o bien, si usas el conmutador incrustado agrupación, configurar al menos dos adaptadores de red. La propagación de entrada máxima se produce cuando se usan dos NIC.  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >Puedes omitir los pasos 3 y 4 si tienes independiente NIC de administración.

3. Consulte el tema de planeación ([planear una infraestructura de red definido de Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el Administrador de red para obtener el identificador de VLAN de la VLAN de administración. Adjuntar el vNIC de administración del conmutador Virtual recién creado a la VLAN de administración. Si el entorno no usa VLAN etiquetas, se puede omitir este paso.  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. Consulte el tema de planeación ([planear una infraestructura de red definido de Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el Administrador de red para usar DHCP bien o asignaciones estáticas de IP para asignar una dirección IP para el vNIC de administración de vSwitch recién creado. El siguiente ejemplo muestra cómo crear una dirección IP estática y asígnale el vNIC de administración de vSwitch:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [Opcional] Implementar una máquina virtual en el host de servicios de dominio de Active Directory ([instalar Active Directory Domain Services (nivel 100)](https://technet.microsoft.com/library/hh472162.aspx) y un servidor DNS.  
   
    Un archivo. Conectar la máquina virtual de Active Directory y DNS Server a la VLAN administración:
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b. Instalar DNS y servicios de dominio de Active Directory.  
      >[!NOTE]
      >El controlador de red admite Kerberos y X.509 certificados para la autenticación. Esta guía usa estos dos mecanismos de autenticación para finalidades distintas (aunque solo uno es necesario).  
        
6. Únete a todos los hosts de Hyper-V para el dominio. Asegúrate de que la entrada del servidor DNS para el adaptador de red que tiene una dirección IP asignada a los puntos de la red de administración a un servidor DNS que pueden resolver el nombre de dominio. Por ejemplo:

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   Un archivo. Haz clic en **inicio**, haz clic en **sistema**y, a continuación, haz clic en **cambiar la configuración de**.  
   b. Haz clic en **cambio**.  
   c. Haz clic en **dominio** y especifica el nombre de dominio.  
   d. Haz clic en **Aceptar**.  
   e. Escribe las credenciales de nombre y la contraseña de usuario cuando se te solicite.  
   f. Reiniciar el servidor.  
  
### <a name="validation"></a>Validación  
Usa los siguientes pasos para validar que el host de red está correctamente configurado.  
1. Asegúrate de que el modificador de la máquina virtual se creó correctamente:  
      
    ``Get-VMSwitch "<switch name>"``  
2. Comprueba que la vNIC administración en el conmutador de máquina virtual está conectado a la VLAN administración:  
    >[!NOTE]
    >Relevante solamente si el tráfico de administración e inquilino comparte la NIC mismo.    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. Comprueba que todos los host de Hyper-V (y recursos de administración externo, por ejemplo: servidores DNS) son accesibles mediante el comando "ping" usar su dirección IP de administración o el nombre de dominio completo (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. Ejecuta el siguiente comando en el host de implementación y especifica el FQDN de cada host de Hyper-V para garantizar las credenciales de Kerberos usadas proporciona acceso a todos los servidores.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Notas y los requisitos de instalación de Nano  
Si usas Nano como los hosts de Hyper-V (servidores físicos) para la implementación, requisitos adicionales son las siguientes acciones:  
1. Todos los nodos de Nano necesitan tener instalado con el paquete de idioma el paquete DSC:  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. Los scripts SDN Express se deben ejecutar desde un host no Nano (ediciones Server Core de Windows o Windows Server con la interfaz gráfica de usuario). No se admiten los flujos de trabajo de PowerShell en Nano.  
3.  Invocar la API NorthBound del controlador de red usando PowerShell o contenedores de REST CN (que dependen de Invoke-WebRequest e Invoke-RestMethod) debe realizarse desde un host no Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Ejecutar Scripts SDN Express  
  
1.  Los archivos de instalación se encuentran en GitHub. Descargar el archivo zip desde el [repositorio de GitHub SDN Microsoft](https://github.com/Microsoft/SDN.git). En la página de repositorio SDN Microsoft, haz clic en **Clone o descarga** y, a continuación, haz clic en **descargar ZIP**.  
  
2.  Designar un equipo como el equipo de implementación.  Este equipo debe ejecutar Windows Server 2016. Expande el archivo zip y copia el **SDNExpress** carpeta en el equipo de implementación `C:\`carpeta.  
  
3.  Compartir la `C:\SDNExpress`carpeta como "**SDNExpress**" con permiso para **todos** a **lectura y escritura**.  
  
4.  Navegar a la `C:\SDNExpress`carpeta.

 Verás las siguientes carpetas:  

|Nombre de carpeta|Descripción|  
|---------------|---------------|  
|AgentConf|Contiene las copias nuevas de esquemas OVSDB utilizado por el agente de Host SDN en cada host de Windows Server 2016 Hyper-V para la directiva de red del programa.|  
|Certificados|Ubicación compartida temporal para el archivo de certificado NC.|  
|Imágenes|Vacío, coloca la imagen de Windows Server 2016 vhdx aquí|  
|Herramientas|Herramientas de depuración y solución de problemas.  Copia a los hosts y máquinas virtuales.  Te recomendamos que realices el Monitor de red o Wireshark aquí para que esté disponible si es necesario.|  
|Scripts|Scripts de implementación.<br /><br />-   **SDNExpress.ps1**<br />    Implementa y configura al fabric, incluidas las máquinas virtuales de controlador de red, máquinas virtuales de SLB Mux, grupos de puerta de enlace y las máquinas de virtuales de puerta de enlace HNV correspondientes a los grupos.<br />-   **FabricConfig.psd1**<br />    Una plantilla de archivo de configuración para el script SDNExpress.  Se personalizará para su entorno.<br />-   **SDNExpressTenant.ps1**<br />    Implementa una carga de trabajo del inquilino de muestra en una red virtual con una VIP de equilibrio de carga.<br />    También se aprovisiona una o más conexiones de red (IPSec S2S VPN, GRE, L3) en las puertas de enlace de borde de proveedor de servicio que están conectados a la carga de trabajo de inquilino creado anteriormente. Las puertas de enlace de IPSec y GRE están disponibles para la conectividad a través de la dirección IP de VIP correspondiente y la puerta de enlace de reenvío L3 sobre el correspondiente conjunto de direcciones.<br />    Este script puede usarse para eliminar la configuración correspondiente con la opción Deshacer también.<br />-   **TenantConfig.psd1**<br />    Un archivo de configuración de plantilla para la configuración de la puerta de enlace de S2S y carga de trabajo del inquilino.<br />-   **SDNExpressUndo.ps1**<br />    Limpia el entorno fabric y se restablece a un estado inicial.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Aprovisiona uno o varios entornos de sitio de empresa con una puerta de enlace de acceso remoto y (opcionalmente a) una máquina de virtual enterprise correspondiente por sitio. Las puertas de enlace de empresa de IPSec o GRE se conecta a la dirección IP VIP correspondiente de la puerta de enlace de proveedor de servicio para establecer los túneles S2S. La puerta de enlace de reenvío L3 se conecta a través de la dirección IP de sistema del mismo nivel correspondiente. <br />            Este script puede usarse para eliminar la configuración correspondiente con la opción Deshacer también.<br />-   **EnterpriseConfig.psd1**<br />    Un archivo de configuración de plantilla para la puerta de enlace de sitio a sitio de empresa y la configuración de la máquina virtual de cliente.|  
|TenantApps|Archivos usados para implementar las cargas de trabajo del inquilino de ejemplo.|  
  
5.  Comprobar el archivo de Windows Server 2016 VHDX está en la **imágenes** carpeta.  
  
6. Personalizar el archivo SDNExpress\scripts\FabricConfig.psd1 cambiando la **<< reemplazar >>** etiquetas con los valores específicos para ajustarse a la infraestructura de laboratorio, incluidos los nombres de host, nombres de dominio, nombres de usuario y contraseñas y la información de red para las redes se enumeran en el tema de planeación de red.  
7. Crear un registro de Host A en DNS para NetworkControllerRestName (FQDN) y NetworkControllerRestIP.  
8. Ejecuta el script como un usuario con credenciales de administrador de dominio:  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  Para deshacer todas las operaciones, ejecuta el siguiente comando:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Validación  
Suponiendo que el script SDN Express se ejecuta completamente sin informar de los errores, puedes realizar el paso siguiente para asegurarte de los recursos de fabric se han implementado correctamente y están disponibles para la implementación del inquilino.  

- Usa [herramientas de diagnóstico](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) para garantizar que no existen errores en todos los recursos fabric en el controlador de red.  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implementar una carga de trabajo del inquilino de muestra con el equilibrado de carga de software  
    
Ahora que se han implementado recursos fabric, puedes validar tu SDN implementación-to-end mediante la implementación de una carga de trabajo del inquilino de muestra. Esta carga de trabajo de inquilino consta de dos subredes virtuales (nivel de base de datos y web) protegidas a través de las reglas de la lista de Control de acceso (ACL) usando el firewall SDN distribuido. La subred virtual del nivel de la web es accesible mediante el SLB/MUX con una dirección IP Virtual (VIP). El script automáticamente implementa dos niveles de máquinas virtuales de web y la máquina virtual de nivel de una base de datos y conecta a las subredes virtuales.  
  
1.  Personalizar el archivo SDNExpress\scripts\TenantConfig.psd1 cambiando la **<< reemplazar >>** etiquetas con valores específicos (por ejemplo: la imagen VHD, nombre REST de controlador de red, vSwitch nombre, etc. tal y como se define en el archivo de FabricConfig.psd1)  
2.  Ejecuta el script. Por ejemplo:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  Para deshacer la configuración, ejecute el mismo script con el **deshacer** parámetro. Por ejemplo:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validación  
Para validar que la implementación de inquilino fue correcta, haz lo siguiente:
1.  Iniciar sesión en la máquina virtual de nivel de base de datos e intente hacer ping a la dirección IP de uno de los web niveles de máquinas virtuales (asegúrese de que Firewall de Windows está desactivado en máquinas virtuales de nivel de web).  
2.  Consulta los recursos de inquilino de controlador de red para todos los errores. Ejecuta lo siguiente desde cualquier host de Hyper-V con conectividad de capa 3 en el controlador de red:  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. Para comprobar que el equilibrado de carga se ejecuta correctamente, ejecuta lo siguiente desde cualquier host de Hyper-V:
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   Donde `<VIP IP address>`es el nivel de web dirección IP VIP has configurado en el archivo TenantConfig.psd1. Busca el `VIPIP`variable en TenantConfig.psd1.

   Ejecutar este veces varias para ver el equilibrado carga cambiar entre el DIP disponibles. También puede observar este comportamiento con un navegador web. Vaya a `<VIP IP address>/unique.htm`. Cerrar al explorador y abrir una nueva instancia y buscar de nuevo. Verás la página azul y la página verde alternativa, excepto cuando el explorador almacena en caché la página antes de que el tiempo de espera de la memoria caché.