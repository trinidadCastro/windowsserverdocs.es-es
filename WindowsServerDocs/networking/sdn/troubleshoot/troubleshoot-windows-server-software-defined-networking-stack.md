---
title: Solución de problemas de la pila de redes definidas por software de Windows Server
description: Esta guía de Windows Server examina los errores comunes de redes definidas por software (SDN) y los escenarios de error, y describe un flujo de trabajo de solución de problemas que aprovecha las herramientas de diagnóstico disponibles.
manager: ravirao
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: 22dcfb318a0e60bd1694496288f3e63b2780d643
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355500"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Solución de problemas de la pila de redes definidas por software de Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta guía se examinan los errores comunes de redes definidas por software (SDN) y los escenarios de error, y se describe un flujo de trabajo de solución de problemas que aprovecha las herramientas de diagnóstico disponibles.  

Para obtener más información acerca de las redes definidas por software de Microsoft, consulte [redes definidas por software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de error  
En la lista siguiente se representa la clase de problemas que se suele observar con la virtualización de red de Hyper-V (HNVv1) en Windows Server 2012 R2 de las implementaciones de producción en el mercado y coincide de muchas maneras con los mismos tipos de problemas que se han detectado en Windows Server 2016 HNVv2 con la nueva pila de redes definidas por software (SDN).  

La mayoría de los errores se pueden clasificar en un pequeño conjunto de clases:   
* **Configuración no válida o no admitida**  
   Un usuario invoca la API NorthBound de forma incorrecta o con una directiva no válida.   

* **Error en la aplicación de Directiva**  
     La Directiva de la controladora de red no se ha entregado a un host de Hyper-V, se ha retrasado o no actualizado en todos los hosts de Hyper-V (por ejemplo, después de un Migración en vivo).  
* **Error de software o de desplazamiento de configuración**  
  Problemas de rutas de acceso de datos que provocan la pérdida de paquetes.  

* **Error externo relacionado con el hardware o los controladores de la NIC o con el tejido de red proporcionaban**  
  Descargamientos de la tarea (como VMQ) o proporcionaban Network fabric mal configurados (por ejemplo, MTU)   

  En esta guía de solución de problemas se examina cada una de estas categorías de error y se recomiendan las prácticas recomendadas y las herramientas de diagnóstico disponibles para identificar y corregir el error.  

## <a name="diagnostic-tools"></a>Herramientas de diagnóstico  

Antes de explicar los flujos de trabajo de solución de problemas de cada uno de estos tipos de errores, vamos a examinar las herramientas de diagnóstico disponibles.   

Para usar las herramientas de diagnóstico de la controladora de red (ruta de acceso de control), primero debe instalar la característica RSAT-NetworkController e importar el módulo ``NetworkControllerDiagnostics``:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar las herramientas de diagnóstico de diagnósticos de HNV (ruta de datos), debe importar el módulo ``HNVDiagnostics``:

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnóstico de controladora de red  
Estos cmdlets están documentados en TechNet en el [tema cmdlet de diagnóstico de la controladora de red](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Ayudan a identificar problemas con la coherencia de la Directiva de red en la ruta de acceso de control entre los nodos de controladora de red y entre el controlador de red y los agentes de host de NC que se ejecutan en los hosts de Hyper-V.

 Los cmdlets _Debug-ServiceFabricNodeStatus_ y _Get-NetworkControllerReplica_ deben ejecutarse desde una de las máquinas virtuales del nodo de la controladora de red. Todos los demás cmdlets de diagnóstico de NC se pueden ejecutar desde cualquier host que tenga conectividad con el controlador de red y se encuentra en el grupo de seguridad de administración de la controladora de red (Kerberos) o tiene acceso al certificado X. 509 para administrar la controladora de red. 

### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host de Hyper-V  
Estos cmdlets están documentados en TechNet en el [tema del cmdlet de diagnóstico de virtualización de red de Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Ayudan a identificar problemas en la ruta de acceso de datos entre las máquinas virtuales de los inquilinos (este y oeste) y el tráfico de entrada a través de una VIP de SLB (norte/sur). 

_Debug-VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_y  _Test-EncapOverheadSettings_ son todas las pruebas locales que se pueden ejecutar desde cualquier host de Hyper-V. Los otros cmdlets invocan las pruebas de ruta de acceso a datos a través de la controladora de red y, por tanto, necesitan tener acceso al controlador de red como descried anterior.

### <a name="github"></a>GitHub
El [repositorio de github de Microsoft/Sdn](https://github.com/microsoft/sdn) tiene una serie de scripts y flujos de trabajo de ejemplo que se basan en estos cmdlets integrados. En concreto, los scripts de diagnóstico se pueden encontrar en la carpeta [Diagnostics](https://github.com/Microsoft/sdn/diagnostics) . Ayúdenos a obtener estos scripts mediante el envío de solicitudes de incorporación de cambios.

## <a name="troubleshooting-workflows-and-guides"></a>Solución de problemas de flujos de trabajo y guías  

### <a name="hoster-validate-system-health"></a>Hosting Validar el estado del sistema
Hay un recurso incrustado denominado _Estado de configuración_ en varios de los recursos de la controladora de red. Estado de configuración proporciona información sobre el estado del sistema, incluida la coherencia entre la configuración de la controladora de red y el estado real (en ejecución) en los hosts de Hyper-V. 

Para comprobar el estado de la configuración, ejecute lo siguiente desde cualquier host de Hyper-V con conectividad a la controladora de red.

>[!NOTE] 
>El valor del parámetro *NetworkController* debe ser el FQDN o la dirección IP según el nombre de sujeto del certificado X. 509 > creado para la controladora de red.
>
>Solo es necesario especificar el parámetro *Credential* si la controladora de red utiliza la autenticación Kerberos (típica en las implementaciones de VMM). La credencial debe ser para un usuario que esté en el grupo de seguridad de administración de la controladora de red.

```none
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

A continuación se muestra un mensaje de estado de configuración de ejemplo:

```none
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> Hay un error en el sistema en el que los recursos de la interfaz de red para la NIC de la máquina virtual de transporte de MUX de SLB están en un estado de error con el error "conmutador virtual: host no conectado al controlador". Este error se puede omitir sin ningún riesgo si la configuración de IP en el recurso de NIC de máquina virtual está establecida en una dirección IP del grupo de direcciones IP de la red lógica de tránsito. Hay un segundo error en el sistema en el que los recursos de la interfaz de red para las NIC de la máquina virtual de puerta de enlace HNV del proveedor se encuentran en un estado de error con el error "Virtual Switch-PortBlocked". Este error también se puede omitir de forma segura si la configuración de IP en el recurso de NIC de la máquina virtual se establece en null (por diseño).


En la tabla siguiente se muestra la lista de códigos de error, mensajes y acciones de seguimiento que se deben realizar en función del estado de configuración observado.


| **Codifica**| **Mensaje**| **Acción**|  
|--------|-----------|----------|  
| Unknown| Error desconocido| |  
| HostUnreachable                       | No se puede tener acceso al equipo host | Comprobar la conectividad de red de administración entre la controladora de red y el host |  
| PAIpAddressExhausted                  | Direcciones IP de PA agotadas | Aumento del tamaño del grupo de direcciones IP de la subred lógica del proveedor de HNV |  
| PAMacAddressExhausted                 | Direcciones MAC de PA agotadas | Aumentar el intervalo de grupo de direcciones MAC |  
| PAAddressConfigurationFailure         | No se pudieron sondear las direcciones PA en el host | Compruebe la conectividad de red de administración entre la controladora de red y el host. |  
| CertificateNotTrusted                 | El certificado no es de confianza  |Corrija los certificados usados para la comunicación con el host. |  
| CertificateNotAuthorized              | Certificado no autorizado | Corrija los certificados usados para la comunicación con el host. |  
| PolicyConfigurationFailureOnVfp       | Error al configurar las directivas de VFP | Se trata de un error de tiempo de ejecución.  No hay ninguna solución alternativa. Recopilar registros. |  
| PolicyConfigurationFailure            | Error al insertar directivas en los hosts debido a errores de comunicación u otros errores en el NetworkController.| Ninguna acción definitiva.  Esto se debe a un error en el procesamiento del estado del objetivo en los módulos de la controladora de red. Recopilar registros. |  
| HostNotConnectedToController          | El host todavía no está conectado a la controladora de red | No se ha aplicado el perfil de puerto en el host o el host no es accesible desde el controlador de red. Validar que la clave del registro HostID coincide con el ID. de instancia del recurso de servidor |  
| MultipleVfpEnabledSwitches            | Hay varios conmutadores habilitados para VFp en el host  | Elimine uno de los conmutadores, ya que el agente de host de la controladora de red solo admite un vSwitch con la extensión VFP habilitada |  
| PolicyConfigurationFailure            | No se pudieron enviar las directivas de red virtual para un VmNic debido a errores de certificado o errores de conectividad  | Compruebe si se han implementado los certificados adecuados (el nombre de sujeto del certificado debe coincidir con el FQDN del host). Comprobar también la Conectividad del host con la controladora de red |  
| PolicyConfigurationFailure            | No se pudieron enviar las directivas vSwitch para un VmNic debido a errores de certificado o errores de conectividad  | Compruebe si se han implementado los certificados adecuados (el nombre de sujeto del certificado debe coincidir con el FQDN del host). Comprobar también la Conectividad del host con la controladora de red|
| PolicyConfigurationFailure            | No se pudieron enviar las directivas de Firewall para un VmNic debido a errores de certificado o errores de conectividad | Compruebe si se han implementado los certificados adecuados (el nombre de sujeto del certificado debe coincidir con el FQDN del host). Comprobar también la Conectividad del host con la controladora de red|
| DistributedRouterConfigurationFailure | No se pudo configurar la configuración del enrutador distribuido en el host vNic                          | Error de pila de TCPIP. Es posible que sea necesario limpiar el host de PA y DR VNIC en el servidor en el que se ha comunicado este error |
| DhcpAddressAllocationFailure          | Error de asignación de dirección DHCP para un VMNic                                                    | Compruebe si el atributo de dirección IP estática está configurado en el recurso de NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | No se pudo conectar con MUX debido a errores de red o de certificado | Compruebe el código numérico proporcionado en el código del mensaje de error: se corresponde con el código de error de Winsock. Los errores de certificado son granulares (por ejemplo, no se puede comprobar el certificado, certificado no autorizado, etc.). |  
| HostUnreachable                       | El estado de MUX es incorrecto (el caso común es BGPRouter disconnected) | No se puede tener acceso al elemento BGP del mismo nivel en el conmutador de la parte superior del rack (de la máquina virtual BGP) o de parte superior del rack (ToR). Comprobar la configuración de BGP en el recurso de multiplexor de software Load Balancer y el BGP del mismo nivel (ToR o máquina virtual de RRAS) |  
| HostNotConnectedToController          | El agente de host de SLB no está conectado  | Compruebe que se está ejecutando el servicio del agente de host SLB; Consulte los registros del agente de host de SLB (ejecución automática) por motivos por los que, en caso de que SLBM (NC) rechace el certificado presentado por el estado de ejecución del agente de host, mostrará información de matices  |  
| PortBlocked                           | El puerto VFP está bloqueado debido a la falta de directivas de red virtual/ACL | Compruebe si hay otros errores, lo que puede provocar que no se configuren las directivas. |  
| Sobrecargado                            | El MUX loadbalancer está sobrecargado  | Problema de rendimiento con MUX |  
| RoutePublicationFailure               | El MUX loadbalancer no está conectado a un enrutador BGP | Compruebe si el MUX tiene conectividad con los enrutadores BGP y que el emparejamiento BGP está configurado correctamente. |  
| VirtualServerUnreachable              | El MUX loadbalancer no está conectado al administrador de SLB | Comprobación de la conectividad entre SLBM y MUX |  
| QosConfigurationFailure               | No se pudieron configurar las directivas de QOS | Ver si hay suficiente ancho de banda disponible para todas las máquinas virtuales si se usa la reserva de QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Comprobar la conectividad de red entre la controladora de red y el host de Hyper-V (servicio del agente de host de CN)
Ejecute el comando *netstat* siguiente para validar que hay tres conexiones establecidas entre el agente de host de NC y los nodos de la controladora de red y un socket de escucha en el host de Hyper-V.
- ESCUCHANDO en el puerto TCP: 6640 en el host de Hyper-V (servicio del agente de host de CN)
- Dos conexiones establecidas desde la IP del host de Hyper-V en el puerto 6640 a la IP del nodo NC en los puertos efímeros (> 32000)
- Una conexión establecida desde la IP del host de Hyper-V en el puerto efímero a la IP de REST de la controladora de red en el puerto 6640

>[!NOTE]
>Solo puede haber dos conexiones establecidas en un host de Hyper-V si no hay máquinas virtuales de inquilino implementadas en ese host concreto.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Comprobar los servicios del agente de host
La controladora de red se comunica con dos servicios de agente de host en los hosts de Hyper-V: Agente de host de SLB y agente de host de NC. Es posible que uno o ambos servicios no se estén ejecutando. Compruebe su estado y reinicie si no se están ejecutando.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Comprobar el estado de la controladora de red
Si no hay tres conexiones establecidas o si el controlador de red parece que no responde, compruebe que todos los nodos y módulos de servicio están en funcionamiento con los siguientes cmdlets. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Los módulos de servicio de la controladora de red son:
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService

Compruebe que ReplicaStatus está **listo** y que HealthState es **correcto**.

En una implementación de producción es con una controladora de red de varios nodos, también puede comprobar en qué nodo es principal cada servicio y su estado de réplica individual.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Compruebe que el estado de la réplica está listo para cada servicio.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Comprobar los HostIDs y certificados correspondientes entre la controladora de red y cada host de Hyper-V 
En un host de Hyper-V, ejecute los siguientes comandos para comprobar que HostID corresponde al identificador de instancia de un recurso de servidor de la controladora de red.

```none
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

*Corrección* Si usa scripts de SDNExpress o implementación manual, actualice la clave HostId en el registro para que coincida con el identificador de instancia del recurso de servidor. Reinicie el agente de host de la controladora de red en el host de Hyper-V (servidor físico) si usa VMM, elimine el servidor de Hyper-V de VMM y quite la clave del registro HostId. A continuación, vuelva a agregar el servidor a través de VMM.


Compruebe que las huellas digitales de los certificados X. 509 que usa el host de Hyper-V (el nombre de host serán el nombre de sujeto del certificado) para la comunicación (SouthBound) entre el host de Hyper-V (servicio del agente de host de CN) y los nodos de la controladora de red son los mismos. Compruebe también que el certificado de REST de la controladora de red tiene el nombre de sujeto de *CN = <FQDN or IP>* .

```  
# On Hyper-V Host
dir cert:\\localmachine\my  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```  

También puede comprobar los siguientes parámetros de cada certificado para asegurarse de que el nombre del sujeto es el que se espera (nombre de host o FQDN o IP de REST de NC), que el certificado no ha expirado y que todas las entidades de certificación de la cadena de certificados están incluidas en la raíz de confianza. entidad certificación.

- Nombre del firmante  
- Fecha de expiración  
- De confianza de la entidad de certificación raíz  

*Corrección* Si hay varios certificados con el mismo nombre de sujeto en el host de Hyper-V, el agente de host de la controladora de red elegirá uno para presentar el controlador de red de forma aleatoria. Es posible que no coincida con la huella digital del recurso de servidor conocido para la controladora de red. En este caso, elimine uno de los certificados con el mismo nombre de sujeto en el host de Hyper-V y, a continuación, vuelva a iniciar el servicio del agente de host de la controladora de red. Si todavía no se puede realizar una conexión, elimine el otro certificado con el mismo nombre de sujeto en el host de Hyper-V y elimine el recurso de servidor correspondiente en VMM. A continuación, vuelva a crear el recurso de servidor en VMM, que generará un nuevo certificado X. 509 e instálelo en el host de Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Comprobar el estado de configuración de SLB
El estado de configuración de SLB se puede determinar como parte de la salida del cmdlet Debug-NetworkController. Este cmdlet también generará el conjunto actual de recursos de la controladora de red en archivos JSON, todas las configuraciones de IP de cada host de Hyper-V (servidor) y la Directiva de red local de las tablas de base de datos del agente de host. 

De forma predeterminada, se recopilarán seguimientos adicionales. Para no recopilar seguimientos, agregue el parámetro-IncludeTraces: $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>La ubicación de salida predeterminada será el directorio < working_directory > \NCDiagnostics\. El directorio de salida predeterminado se puede cambiar mediante el parámetro `-OutputDirectory`. 

La información de estado de configuración de SLB se puede encontrar en el archivo _Diagnostics-slbstateResults. JSON_ de este directorio.

Este archivo JSON se puede dividir en las siguientes secciones:
 * Tejido
   * SlbmVips: en esta sección se muestra la dirección IP de la dirección VIP del administrador SLB usada por la controladora de red para coodinate la configuración y el estado entre los agentes de host de SLB MUX y SLB.
   * MuxState: en esta sección se enumera un valor para cada MUX de SLB implementado, lo que proporciona el estado del MUX
   * Configuración del enrutador: en esta sección se enumera el número de sistema autónomo (ASN) del enrutador ascendente (ASN), la dirección IP de tránsito y el identificador. También se mostrará el ASN MUX de SLB y la IP de tránsito.
   * Información de host conectado: en esta sección se enumeran las direcciones IP de administración de todos los hosts de Hyper-V disponibles para ejecutar cargas de trabajo con equilibrio de carga.
   * Intervalos de VIP: en esta sección se enumeran los intervalos de grupos IP VIP públicos y privados. La VIP de SLBM se incluirá como una dirección IP asignada de uno de estos intervalos. 
   * Rutas de MUX: en esta sección se enumera un valor para cada MUX de SLB implementado que contiene todos los anuncios de ruta para ese MUX determinado.
 * Inquilino
   * VipConsolidatedState: en esta sección se muestra el estado de conectividad de cada VIP de inquilino, incluido el prefijo de ruta anunciado, el host de Hyper-V y los puntos de conexión DIP.

> [!NOTE]
> El estado de SLB se puede determinar directamente mediante el script [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) disponible en el [repositorio de github de SDN de Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validación de puerta de enlace

**Desde controladora de red:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Desde la máquina virtual de puerta de enlace:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Del conmutador de la parte superior del rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Enrutador BGP de Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Además, a partir de los problemas que hemos encontrado hasta ahora (especialmente en las implementaciones basadas en SDNExpress), la razón más común para que el compartimiento de inquilinos no se configure en las máquinas virtuales de GW parece ser el hecho de que la capacidad de GW en FabricConfig. psd1 es menos que las personas intentan asignarse a las conexiones de red (Túneles S2S) en TenantConfig. psd1. Esto se puede comprobar fácilmente si se comparan los resultados de los siguientes comandos:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>Hosting Validar plano de datos
Una vez implementada la controladora de red, se han creado las redes virtuales de inquilinos y las subredes, y las máquinas virtuales se han conectado a las subredes virtuales, el anfitrión puede realizar otras pruebas de nivel de tejido para comprobar la conectividad de los inquilinos.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Comprobar la conectividad de red lógica del proveedor de HNV
Después de que la primera máquina virtual invitada que se ejecuta en un host de Hyper-V se haya conectado a una red virtual de inquilino, la controladora de red asignará dos direcciones IP de proveedor de HNV (direcciones IP de PA) al host de Hyper-V. Estas direcciones IP procederán del grupo de direcciones IP de la red lógica del proveedor de HNV y se administrarán mediante la controladora de red.  Para averiguar cuáles son las dos direcciones IP de HNV

```none
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

Estas direcciones IP de proveedor de HNV (PA IP) se asignan a los adaptadores Ethernet creados en un compartimiento de red TCPIP independiente y tienen el nombre de adaptador _VLANX_ , donde X es la VLAN asignada a la red lógica del proveedor de HNV (transporte).

La conectividad entre dos hosts de Hyper-V que usa la red lógica del proveedor de HNV puede realizarse mediante un ping con un parámetro de compartimiento adicional (-c Y), donde Y es el compartimiento de red TCPIP en el que se crean los PAhostVNICs. Este compartimiento se puede determinar mediante la ejecución de:

```none
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> Los adaptadores de vNIC host de PA no se usan en la ruta de acceso de datos y, por tanto, no tienen una dirección IP asignada al adaptador de vEthernet (PAhostVNic).

Por ejemplo, supongamos que los hosts de Hyper-V 1 y 2 tienen direcciones IP de proveedor HNV (PA) de:

|-Host de Hyper-V-|-Dirección IP PA 1|-Dirección IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

podemos hacer ping entre los dos mediante el siguiente comando para comprobar la conectividad de red lógica del proveedor de HNV.

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*Corrección* Si el ping del proveedor HNV no funciona, Compruebe la conectividad de red física, incluida la configuración de VLAN. Las NIC físicas de cada host de Hyper-V deben estar en modo de tronco sin ninguna VLAN específica asignada. El host de administración vNIC debe aislarse en la VLAN de la red lógica de administración.

```none
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...        
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...
```

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Comprobar la compatibilidad de MTU y de tramas Jumbo en la red lógica del proveedor de HNV

Otro problema común en la red lógica del proveedor de HNV es que los puertos de red físicos o la tarjeta Ethernet no tienen una MTU suficientemente grande configurada para controlar la sobrecarga de la encapsulación VXLAN (o NVGRE). 
>[!NOTE]
> Algunas tarjetas y controladores Ethernet admiten la nueva palabra clave * EncapOverhead, que establecerá automáticamente el agente de host de la controladora de red en un valor de 160. Este valor se agregará al valor de la palabra clave * JumboPacket cuya suma se usa como MTU anunciada.
> por ejemplo, * EncapOverhead = 160 y * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para probar si la red lógica del proveedor de HNV admite o no el tamaño de MTU más grande, use el cmdlet _Test-LogicalNetworkSupportsJumboPacket_ :
```none
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.

# TODO: Success Results aftering updating MTU on physical switch ports
```

*Corrección*
* Ajuste el tamaño de MTU en los puertos de conmutador físico para que sea al menos 1674B (incluido el encabezado y el finalizador de Ethernet de 14B)
* Si la tarjeta NIC no admite la palabra clave EncapOverhead, ajuste la palabra clave JumboPacket para que sea al menos 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Comprobar la conectividad de NIC de VM de inquilino
Cada NIC de máquina virtual asignada a una máquina virtual invitada tiene una asignación de CA-PA entre la dirección de cliente privada (CA) y el espacio de direcciones de proveedor HNV (PA). Estas asignaciones se mantienen en las tablas del servidor de OVSDB en cada host de Hyper-V y se pueden encontrar ejecutando el siguiente cmdlet.

```none
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66
```
>[!NOTE]
> Si las asignaciones de CA-PA que espera no son salidas para una máquina virtual de inquilino determinada, compruebe los recursos de configuración de IP y la NIC de la máquina virtual en el controlador de red mediante el cmdlet _Get-NetworkControllerNetworkInterface_ . Además, compruebe las conexiones establecidas entre el agente de host de NC y los nodos de controlador de red.

Con esta información, el anfitrión puede iniciar ping de VM de inquilino desde el controlador de red mediante el cmdlet _Test-VirtualNetworkConnection_ .

## <a name="specific-troubleshooting-scenarios"></a>Escenarios de solución de problemas específicos

En las secciones siguientes se proporcionan instrucciones para la solución de problemas de escenarios específicos.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>No hay conectividad de red entre dos máquinas virtuales de inquilino

1.  Inquilino Asegúrese de que el Firewall de Windows en las máquinas virtuales de inquilino no está bloqueando el tráfico.  
2.  Inquilino Compruebe que las direcciones IP se han asignado a la máquina virtual del inquilino mediante la ejecución de _ipconfig_. 
3.  Hosting Ejecute **Test-VirtualNetworkConnection** desde el host de Hyper-V para validar la conectividad entre las dos máquinas virtuales de inquilino en cuestión. 

>[!NOTE]
>El ID. de sesión hace referencia al identificador de subred virtual. En el caso de VXLAN, es el identificador de red de VXLAN (VNI). Puede encontrar este valor mediante la ejecución del cmdlet **Get-PACAMapping** .

#### <a name="example"></a>Ejemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Cree un ping de CA entre "Green web VM 1" con SenderCA IP de 192.168.1.4 en el host "sa18n30-2.sa18.nttest.microsoft.com" con la IP de MGMT de 10.127.132.153 en ListenerCA IP de 192.168.1.5 asociadas a la subred virtual (4114).

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Iniciando prueba de ping de espacio de la entidad de certificación iniciando la sesión de seguimiento ping a 192.168.1.5 correctamente desde la dirección 192.168.1.4 RTT = 0 ms


Información de enrutamiento de CA:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Información de enrutamiento de PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip> ...

4. Inquilino Compruebe que no se ha especificado ninguna directiva de Firewall distribuido en las interfaces de red de VM o subred virtual que bloquearían el tráfico.    

Consulte la API de REST de la controladora de red que se encuentra en el entorno de demostración en sa18n30nc en el dominio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Vea la configuración de IP y las subredes virtuales que hacen referencia a esta ACL

1. Hosting Ejecute ``Get-ProviderAddress`` en ambos hosts de Hyper-V que hospeden las dos máquinas virtuales de inquilino en cuestión y, a continuación, ejecute ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` desde el host de Hyper-V para validar la conectividad en la red lógica del proveedor de HNV
2.  Hosting Asegúrese de que la configuración de MTU sea correcta en los hosts de Hyper-V y en cualquier dispositivo de conmutación de capa 2 entre los hosts de Hyper-V. Ejecute ``Test-EncapOverheadValue`` en todos los hosts de Hyper-V en cuestión. Compruebe también que todos los conmutadores de nivel 2 entre tienen la MTU establecida en menos de 1674 bytes para tener en cuenta la sobrecarga máxima de 160 bytes.  
3.  Hosting Si las direcciones IP de PA no están presentes y/o se interrumpe la conectividad de la entidad de certificación, asegúrese de que se ha recibido la Directiva de red. Ejecute ``Get-PACAMapping`` para ver si se han establecido correctamente las asignaciones de reglas de encapsulación y CA-PA necesarias para la creación de redes virtuales de superposición.  
4.  Hosting Compruebe que el agente de host de la controladora de red esté conectado a la controladora de red. Ejecute ``netstat -anp tcp |findstr 6640`` para ver si el   
5.  Hosting Compruebe que el identificador de host en HKLM/coincide con el ID. de instancia de los recursos de servidor que hospedan las máquinas virtuales de inquilino.  
6. Hosting Compruebe que el identificador de Perfil de Puerto coincide con el identificador de instancia de las interfaces de red de máquina virtual de las máquinas virtuales de inquilino.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro, seguimiento y diagnósticos avanzados

En las secciones siguientes se proporciona información sobre diagnósticos avanzados, registro y seguimiento.

### <a name="network-controller-centralized-logging"></a>Registro centralizado de controladora de red 

La controladora de red puede recopilar automáticamente registros del depurador y almacenarlos en una ubicación centralizada. La recopilación de registros se puede habilitar al implementar la controladora de red por primera vez o en cualquier momento posterior. Los registros se recopilan de la controladora de red y los elementos de red administrados por la controladora de red: máquinas host, equilibradores de carga de software (SLB) y máquinas de puerta de enlace. 

Estos registros incluyen registros de depuración para el clúster de controladora de red, la aplicación de controladora de red, los registros de puerta de enlace, SLB, la red virtual y el Firewall distribuido. Cada vez que se agrega un nuevo host/SLB/Gateway a la controladora de red, se inicia el registro en esos equipos. Del mismo modo, cuando se quita un host/SLB/Gateway de la controladora de red, el registro se detiene en esos equipos.

#### <a name="enable-logging"></a>Habilitar registro

El registro se habilita automáticamente al instalar el clúster de la controladora de red mediante el cmdlet **install-NetworkControllerCluster** . De forma predeterminada, los registros se recopilan localmente en los nodos de la controladora de red en *%SystemDrive%\SDNDiagnostics*. Se **recomienda encarecidamente** cambiar esta ubicación para que sea un recurso compartido de archivos remoto (no local). 

Los registros de clúster de la controladora de red se almacenan en *%ProgramData%\Windows Fabric\log\Traces*. Puede especificar una ubicación centralizada para la recopilación de registros con el parámetro **DiagnosticLogLocation** con la recomendación de que este también sea un recurso compartido de archivos remoto. 

Si desea restringir el acceso a esta ubicación, puede proporcionar las credenciales de acceso con el parámetro **LogLocationCredential** . Si proporciona las credenciales para tener acceso a la ubicación del registro, también debe proporcionar el parámetro **CredentialEncryptionCertificate** , que se usa para cifrar las credenciales almacenadas localmente en los nodos de la controladora de red.  

Con la configuración predeterminada, se recomienda que tenga al menos 75 GB de espacio disponible en la ubicación central y 25 GB en los nodos locales (si no usa una ubicación central) para un clúster de controlador de red de tres nodos.

#### <a name="change-logging-settings"></a>Cambiar la configuración de registro

Puede cambiar la configuración de registro en cualquier momento mediante el cmdlet ``Set-NetworkControllerDiagnostic``. Se puede cambiar la siguiente configuración:

- **Ubicación de registro centralizada**.  Puede cambiar la ubicación para almacenar todos los registros, con el parámetro ``DiagnosticLogLocation``.  
- **Credenciales para tener acceso**a la ubicación del registro.  Puede cambiar las credenciales para tener acceso a la ubicación del registro, con el parámetro ``LogLocationCredential``.  
- **Desplácese al registro local**.  Si ha proporcionado una ubicación centralizada para almacenar los registros, puede volver al registro localmente en los nodos de la controladora de red con el parámetro ``UseLocalLogLocation`` (no recomendado debido a los grandes requisitos de espacio en disco).  
- **Ámbito de registro**.  De forma predeterminada, se recopilan todos los registros. Puede cambiar el ámbito para recopilar solo los registros de clúster de la controladora de red.  
- **Nivel de registro**.  El nivel de registro predeterminado es informativo. Puede cambiarlo a error, ADVERTENCIA o detallado.  
- **Hora de caducidad del registro**.  Los registros se almacenan de manera circular. De forma predeterminada, tendrá 3 días de registro de datos, independientemente de que use el registro local o el registro centralizado. Puede cambiar este límite de tiempo con el parámetro **LogTimeLimitInDays** .  
- **Tamaño**de la antigüedad del registro.  De forma predeterminada, tendrá un máximo de 75 GB de datos de registro si usa el registro centralizado y 25 GB si usa el registro local. Puede cambiar este límite con el parámetro **LogSizeLimitInMBs** .

#### <a name="collecting-logs-and-traces"></a>Recopilación de registros y seguimientos

Las implementaciones de VMM usan el registro centralizado para la controladora de red de forma predeterminada. La ubicación del recurso compartido de archivos para estos registros se especifica al implementar la plantilla de servicio de la controladora de red.

Si no se ha especificado la ubicación de un archivo, se usará el registro local en cada nodo de la controladora de red con registros guardados en C:\Windows\tracing\SDNDiagnostics. Estos registros se guardan con la siguiente jerarquía:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Tene

La controladora de red utiliza (Azure) Service Fabric. Los registros de Service Fabric pueden ser necesarios para solucionar determinados problemas. Estos registros se pueden encontrar en cada nodo de la controladora de red en C:\ProgramData\Microsoft\Service fabric.

Si un usuario ha ejecutado el cmdlet _Debug-NetworkController_ , habrá registros adicionales disponibles en cada host de Hyper-V que se haya especificado con un recurso de servidor en el controlador de red. Estos registros (y seguimientos si están habilitados) se mantienen en C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnóstico de SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errores de tejido de SLBM (acciones de proveedor de servicio de hospedaje)

1.  Compruebe que el administrador de Load Balancer de software (SLBM) funciona y que los niveles de orquestación pueden comunicarse entre sí: Agentes de host de SLB-> SLB MUX y SLBM-> SLB. Ejecute [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) desde cualquier nodo con acceso al punto de conexión de REST de la controladora de red.  
2.  Valide *SDNSLBMPerfCounters* en Perfmon en una de las máquinas virtuales de nodo de la controladora de red (preferiblemente el nodo principal de la controladora de red-Get-NetworkControllerReplica):
    1.  ¿Está el motor de Load Balancer (LB) conectado a SLBM? (*Total de configuraciones de LBEngine de SLBM* > 0)  
    2.  ¿Se conoce SLBM como mínimo de sus propios puntos de conexión? (*Número total de puntos de conexión VIP* > = 2)  
    3.  ¿Los hosts de Hyper-V (DIP) están conectados a SLBM? (*Clientes de HP conectados* = = número de servidores)   
    4.  ¿Se conecta SLBM a MUX? (*MUX conectado* == *MUX correcto en SLBM* == *MUX informes correctos* = # SLB MUX VM).  
3.  Asegúrese de que el enrutador BGP configurado está emparejando correctamente con el MUX de SLB  
    1.  Si usa RRAS con acceso remoto (es decir, máquina virtual BGP):  
        1.  Get-eliminara debe mostrar connected  
        2.  Get-BgpRouteInformation debe mostrar al menos una ruta para la VIP Self de SLBM  
    2.  Si usa el conmutador físico de parte superior del rack (ToR) como BGP del mismo nivel, consulte la documentación.  
        1.  Por ejemplo: # show BGP Instance  
4.  Validación de los contadores *SlbMuxPerfCounters* y *SLBMUX* en Perfmon en la máquina virtual de SLB MUX
5.  Comprobar el estado de configuración y los intervalos de VIP en el recurso de software Load Balancer Manager  
    1.  Get-NetworkControllerLoadBalancerConfiguration-ConnectionUri < https://<FQDN or IP> | ConvertTo-JSON-Depth 8 (comprobar los intervalos de VIP en grupos de direcciones IP y garantizar que la auto-VIP de SLBM (*LoadBalanacerManagerIPAddress*) y cualquier VIP orientada a inquilinos estén dentro de estos intervalos)  
        1. Get-NetworkControllerIpPool-NetworkId "< identificador de recurso de red lógica de VIP pública/privada >"-subnetid "< identificador de recurso de subred lógica de VIP pública/privada >"-ResourceId "<IP Pool Resource Id>"-ConnectionUri $uri | ConvertTo-JSON-Depth 8 
    2.  Debug-NetworkControllerConfigurationState-  

Si se produce un error en cualquiera de las comprobaciones anteriores, el estado de SLB del inquilino también estará en modo de error.  

**Corrección**   
En función de la siguiente información de diagnóstico presentada, corrija lo siguiente:  
* Asegurarse de que los multiplexores SLB estén conectados  
  * Corregir problemas de certificados  
  * Corregir problemas de conectividad de la red  
* Asegurarse de que la información de emparejamiento BGP está configurada correctamente  
* Asegúrese de que el identificador de host del registro coincide con el ID. de instancia de servidor en el recurso de servidor (apéndice de referencia para el código de error *HostNotConnected* )  
* Recopilar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errores de inquilino de SLBM (acciones de inquilino y proveedor de servicios de hospedaje)

1.  Hosting Compruebe *Debug-NetworkControllerConfigurationState* para ver si hay algún recurso loadbalancer en estado de error. Intente realizar la mitigación siguiendo la tabla elementos de acción del apéndice.   
    1.  Comprobar que hay un punto de conexión de VIP presente y las rutas de publicidad  
    2.  Comprobar cuántos puntos de conexión DIP se han detectado para el punto de conexión de VIP  
2.  Inquilino Validar Load Balancer recursos se han especificado correctamente  
    1.  Validar que los puntos de conexión DIP registrados en SLBM hospedan máquinas virtuales de inquilino que corresponden a las configuraciones de IP del grupo de direcciones de back-end de LoadBalancer  
3.  Hosting Si los puntos de conexión DIP no se detectan ni se conectan:   
    1.  Check *Debug-NetworkControllerConfigurationState*  
        1.  Validar que el agente de host de SLB y SLB está conectado correctamente al Coordinador de eventos de la controladora de red mediante ``netstat -anp tcp |findstr 6640)``  
    2.  Compruebe *HostId* en la clave del servicio *nchostagent* (referencia del código de error *HostNotConnected* en el apéndice) coincide con el ID. de instancia del recurso del servidor correspondiente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Comprobar el ID. de Perfil de puerto de la máquina virtual coincide con el ID. de instancia del recurso NIC de máquina virtual   
4.  [Proveedor de hospedaje] Recopilar registros   

#### <a name="slb-mux-tracing"></a>Seguimiento del MUX de SLB

La información del software Load Balancer MUX también se puede determinar a través de Visor de eventos. 
1. Haga clic en "Mostrar registros analíticos y de depuración" en el menú Ver Visor de eventos
2. Vaya a "registros de aplicaciones y servicios" > el seguimiento de Microsoft > Windows > SlbMuxDriver > en Visor de eventos 
3. Haga clic con el botón derecho en él y seleccione "habilitar registro".

>[!NOTE]
>Se recomienda que solo tenga este registro habilitado durante un breve período de tiempo mientras intenta reproducir un problema.

### <a name="vfp-and-vswitch-tracing"></a>Seguimiento de VFP y vSwitch

Desde cualquier host de Hyper-V que hospeda una máquina virtual invitada conectada a una red virtual de inquilino, puede recopilar un seguimiento de VFP para determinar dónde pueden encontrarse los problemas.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
