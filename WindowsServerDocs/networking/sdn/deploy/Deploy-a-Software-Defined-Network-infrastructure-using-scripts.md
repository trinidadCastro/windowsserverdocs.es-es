---
title: Implementar una infraestructura de red definida por Software con Scripts
description: En este tema se explica cómo implementar una infraestructura de red definida por Software (SDN) de Microsoft con scripts de Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: dabfe3de4cc307723ff7e614fb73e3903e74aeb2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844626"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Implementación de una infraestructura de red definida por software con scripts

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, implemente una infraestructura de red definida por Software (SDN) de Microsoft con scripts. La infraestructura incluye un controlador de red (HA) alta disponibilidad, una alta disponibilidad de Software de equilibrador de carga (SLB) / MUX, redes virtuales y los asociados de listas de Control de acceso (ACL). Además, otro script implementa una carga de trabajo de inquilino para validar la infraestructura de SDN.  
  
Si desea que las cargas de trabajo de inquilinos para la comunicación fuera de sus redes virtuales, puede configurar reglas NAT de SLB, túneles de puerta de enlace de sitio a sitio o reenvío de capa 3 para enrutar mensajes entre las cargas de trabajo físicas y virtuales.  
  
También puede implementar una infraestructura SDN mediante Virtual Machine Manager (VMM). Para obtener más información, consulte [configurar una infraestructura de red definida por Software (SDN) en el tejido de VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  

  
## <a name="pre-deployment"></a>Anterior a la implementación  
  
> [!IMPORTANT]  
> Antes de comenzar la implementación, debe planear y configurar los hosts y la infraestructura de red física. Para más información, consulta [Planificación de una infraestructura de red definida por software](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Todos los hosts de Hyper-V deben tener instalado Windows Server 2016.  
  
## <a name="deployment-steps"></a>Pasos de implementación  
Empiece por configurar el conmutador virtual de Hyper-V (servidores físicos) y la asignación de direcciones IP del host de Hyper-V. Se puede usar cualquier tipo de almacenamiento es compatible con Hyper-V, local o compartida.  

### <a name="install-host-networking"></a>Instalación de redes de host  

1. Instalar a los controladores de red más recientes disponibles para el hardware NIC.  
2. Instalar el rol de Hyper-V en todos los hosts (para obtener más información, consulte [Introducción a Hyper-V en Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   
  
   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  
    
3. Cree el conmutador virtual de Hyper-V.<p>Usar el mismo nombre de conmutador para todos los hosts, por ejemplo, **sdnSwitch**. Configure al menos un adaptador de red, o bien, si usa el conjunto, configure al menos dos adaptadores de red. La propagación máxima entrante se produce cuando se usa dos NIC.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Si tiene NIC de administración independiente, puede omitir los pasos 4 y 5.

3. Consulte el tema planificación ([planear una infraestructura de red definida por Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el Administrador de red para obtener el identificador de VLAN de la VLAN de administración. Conectar la vNIC de administración del conmutador Virtual recién creada a la VLAN de administración. Este paso puede omitirse si el entorno no usa las etiquetas VLAN.  
   
   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  
 
4. Consulte el tema planificación ([planear una infraestructura de red definida por Software](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) y trabajar con el Administrador de red para usar DHCP bien o asignaciones de IP estáticas para asignar una dirección IP a la vNIC de administración recién creada vSwitch. El ejemplo siguiente muestra cómo crear una dirección IP estática y asignarla a la vNIC de administración del conmutador virtual:  
 
   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  
      
5. [Opcional] Implementar una máquina virtual para hospedar los servicios de dominio de Active Directory ([instalar Active Directory Domain Services (nivel 100)](https://technet.microsoft.com/library/hh472162.aspx) y un servidor DNS.  
   
    a. Conectar la máquina virtual de servidor de Active Directory/DNS a la VLAN de administración:
    
       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Instalar servicios de dominio de Active Directory y DNS.  

   >[!NOTE]
   >La controladora de red admite certificados de Kerberos y X.509 para la autenticación. Esta guía usa ambos mecanismos de autenticación para distintos fines (aunque sólo uno es necesario).  
        
6. Únase a todos los hosts de Hyper-V en el dominio. Asegúrese de que la entrada del servidor DNS para el adaptador de red que tiene una dirección IP asignada a los puntos de administración de red a un servidor DNS que pueda resolver el nombre de dominio. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```
   
   a. Haga clic en **iniciar**, haga clic en **sistema**y, a continuación, haga clic en **cambiar la configuración**.  
   b. Haga clic en **Cambiar**.  
   c. Haga clic en **dominio** y especifique el nombre de dominio.  
   d. Haga clic en **Aceptar**.  
   e. Escriba las credenciales de nombre y la contraseña de usuario cuando se le solicite.  
   f. Reinicie el servidor.  
  
### <a name="validation"></a>Resultados  
Use los pasos siguientes para validar que alojan funciones de red es configurar correctamente.  

1. Asegúrese de que el conmutador de máquina virtual se creó correctamente:  
      
   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Compruebe que la vNIC de administración en el conmutador de máquina virtual está conectada a la VLAN de administración:  

   >[!NOTE]
   >Sólo es relevante si tráfico de inquilinos y administración comparte la misma NIC.    
      
   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Validar todos los hosts de Hyper-V y los recursos de administración externo, por ejemplo, los servidores DNS.<p>Asegúrese de que son accesibles a través del ping mediante su dirección IP de administración o el nombre de dominio completo (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Ejecute el siguiente comando en el host de implementación y especifique el FQDN de cada host de Hyper-V para garantizar las credenciales de Kerberos usadas proporciona acceso a todos los servidores.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Notas y los requisitos de instalación de Nano  

Si usas Nano como los hosts de Hyper-V (servidores físicos) para la implementación, los siguientes son requisitos adicionales:  

1. Todos los nodos de Nano deben tener el paquete de DSC que se instala con el paquete de idioma:  
   
    - Microsoft-NanoServer-DSC-Package.cab  
    - Microsoft-NanoServer-DSC-Package_en-us.cab
   
    ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Los scripts de SDN Express se deben ejecutar desde un host que no sea Nano (Windows Server Core o Windows Server con la interfaz gráfica de usuario). No se admiten los flujos de trabajo de PowerShell en Nano.  

3.  Invocar la API NorthBound de controladora de red con PowerShell o contenedores de REST de controladora de red (que dependen de Invoke-WebRequest y Invoke-RestMethod) debe realizarse desde un host que no sea Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Ejecutar scripts de SDN Express  
  
1. Vaya a la [repositorio de GitHub de SDN de Microsoft](https://github.com/Microsoft/SDN.git) para los archivos de instalación.

2. Descargue los archivos de instalación desde el repositorio en el equipo de implementación designado. Haga clic en **clonar o descargar** y, a continuación, haga clic en **Download ZIP**.  
 
   >[!NOTE]
   >El equipo de implementación designado debe ejecutar Windows Server 2016 o posterior.
 
3. Expanda el archivo zip y copie el **SDNExpress** carpeta en el equipo de implementación `C:\` carpeta.  
  
4. Recurso compartido de la `C:\SDNExpress` carpeta como "**SDNExpress**" con permiso para **todo el mundo** a **lectura/escritura**.  
  
5. Navegue hasta la `C:\SDNExpress` carpeta.<p>Consulte las siguientes carpetas:  

   |Nombre de carpeta|Descripción|  
   |---------------|---------------|  
   |AgentConf|Contiene copias nuevas de esquemas OVSDB utilizados por el agente de Host de SDN en cada host de Windows Server 2016 Hyper-V para la directiva de red del programa.|  
   |Certificados|Ubicación compartida temporal para el archivo de certificado de la controladora de red.|  
   |Imágenes|Está vacío, coloque la imagen de vhdx de Windows Server 2016 aquí|  
   |Herramientas|Utilidades para solucionar problemas y depuración.  Si se copian en los hosts y máquinas virtuales.  Se recomienda que colocar el Monitor de red o Wireshark aquí para que esté disponible si es necesario.|  
   |Scripts|Scripts de implementación.<br /><br />-   **SDNExpress.ps1**<br />    Implementa y configura al tejido, incluidas las máquinas virtuales de controlador de red, las máquinas virtuales de SLB/Mux, grupos de puerta de enlace y las máquinas virtuales de puerta de enlace HNV correspondientes a los grupos.<br />-   **FabricConfig.psd1**<br />    Una plantilla de archivo de configuración para la secuencia de comandos SDNExpress.  Va a personalizar esto para su entorno.<br />-   **SDNExpressTenant.ps1**<br />    Implementa una carga de trabajo de inquilinos de ejemplo en una red virtual con una VIP con equilibrio de carga.<br />    También proporciona una o varias conexiones de red (VPN S2S de IPSec, GRE, L3) en las puertas de enlace de borde de proveedor de servicio que están conectadas a la carga de trabajo de inquilino creada anteriormente. Las puertas de enlace de IPSec y GRE están disponibles para la conectividad a través de la dirección IP de VIP correspondiente y la puerta de enlace de reenvío L3 a través del grupo de direcciones correspondiente.<br />    Este script puede utilizarse para eliminar la configuración correspondiente también una opción de deshacer.<br />-   **TenantConfig.psd1**<br />    Un archivo de configuración de plantilla para la carga de trabajo de inquilino y la configuración de puerta de enlace S2S.<br />-   **SDNExpressUndo.ps1**<br />    Limpia el entorno de fabric y la restablece a un estado inicial.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Aprovisiona uno o más entornos de sitio de empresa con una puerta de enlace de acceso remoto y (opcionalmente) una máquina de virtual de enterprise correspondiente por sitio. Las puertas de enlace de empresa de IPSec o GRE se conecta a la dirección IP de IP virtual correspondiente de la puerta de enlace del proveedor de servicio para establecer los túneles S2S. La puerta de enlace de reenvío L3 se conecta a través de la dirección de IP del mismo nivel correspondiente. <br />            Este script puede utilizarse para eliminar la configuración correspondiente también una opción de deshacer.<br />-   **EnterpriseConfig.psd1**<br />    Un archivo de configuración de plantilla para la puerta de enlace de sitio a sitio empresarial y la configuración de máquina virtual del cliente.|  
   |TenantApps|Archivos utilizados para implementar cargas de trabajo de inquilinos de ejemplo.|  
   ---
  
6. Compruebe el archivo VHDX de Windows Server 2016 se encuentra en la **imágenes** carpeta.  
  
7. Personalizar el archivo SDNExpress\scripts\FabricConfig.psd1 cambiando el **<< reemplazar >>** etiquetas con valores específicos para ajustarse a su infraestructura de laboratorio, incluidos los nombres de host, los nombres de dominio, los nombres de usuario y contraseñas, y información de red para las redes mostradas en el tema de planificación de red.  

8. Crear un registro Host A de DNS para el NetworkControllerRestName (FQDN) y NetworkControllerRestIP.  

9. Ejecute el script como un usuario con credenciales de administrador de dominio:  
      
   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
10. Para todas las operaciones de deshacer, ejecute el siguiente comando:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Resultados  

Suponiendo que el script de SDN Express se ejecutó hasta completarse sin informar de los errores, puede realizar el paso siguiente para asegurarse de los recursos de tejido se ha implementado correctamente y están disponibles para la implementación del inquilino.  

Use [herramientas de diagnóstico](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) para asegurarse de que no hay ningún error en todos los recursos de tejido de la controladora de red.  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Implementar una carga de trabajo de inquilinos de ejemplo con el equilibrador de carga de software  
    
Ahora que se han implementado los recursos del tejido, puede validar la SDN implementación-to-end mediante la implementación de una carga de trabajo de inquilinos de ejemplo. Esta carga de trabajo de inquilino se compone de dos subredes virtuales (nivel web y nivel de base de datos) protegidas mediante reglas de lista de Control de acceso (ACL) mediante el firewall distribuido de SDN. Subred virtual del nivel web es accesible a través de SLB/MUX con una dirección IP Virtual (VIP). El script implementa dos máquinas virtuales del nivel web y la máquina virtual de nivel de una base de datos y conecta a las subredes virtuales automáticamente.  
  
1.  Personalizar el archivo SDNExpress\scripts\TenantConfig.psd1 cambiando el **<< reemplazar >>** etiquetas con valores específicos (por ejemplo: Nombre de la imagen de disco duro virtual, el nombre REST de controladora de red, vSwitch nombre, etc. definidos anteriormente en el archivo FabricConfig.psd1)  

2.  Ejecute el script. Por ejemplo:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Para deshacer la configuración, ejecute el mismo script con el **deshacer** parámetro. Por ejemplo:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Resultados  

Para validar que la implementación de inquilinos fue correcta, realice lo siguiente:

1. Inicie sesión en la máquina virtual de nivel de base de datos e intente hacer ping a la dirección IP de una de las máquinas virtuales de nivel web (asegúrese de que Firewall de Windows está desactivado en las máquinas virtuales de capa web).  

2. Compruebe los recursos del inquilino de controlador de red para los errores. Ejecute lo siguiente desde cualquier host de Hyper-V con la conectividad de nivel 3 para la controladora de red:  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Para comprobar que el equilibrador de carga se está ejecutando, ejecute lo siguiente desde cualquier host de Hyper-V:
    
   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``
   
   donde `<VIP IP address>` es la dirección IP de IP virtual configurada en el archivo TenantConfig.psd1 del nivel web. 

   >[!TIP]
   >Busque el `VIPIP` variable TenantConfig.psd1.

   Ejecute este veces varios para ver el equilibrador de carga cambiar entre los DIP disponibles. También puede observar este comportamiento mediante un explorador web. Vaya a `<VIP IP address>/unique.htm`. Cierre al explorador y abra una nueva instancia y busque de nuevo. Verá la página azul y la página verde alternativa, excepto cuando el explorador almacena en caché la página antes de la memoria caché expira.

---