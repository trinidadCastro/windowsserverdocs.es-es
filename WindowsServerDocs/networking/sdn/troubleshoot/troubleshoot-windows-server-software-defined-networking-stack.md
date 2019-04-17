---
title: Solucionar problemas de la pila de red definido de Windows Server Software
description: Esta guía de Windows Server examina los errores comunes de Software definido de redes (SDN) y los escenarios de error y muestran un flujo de trabajo de solución de problemas que saca provecho de las herramientas de diagnósticos disponibles.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Solucionar problemas de la pila de red definido de Windows Server Software

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Esta guía examina los errores comunes de Software definido de redes (SDN) y los escenarios de error y describe un flujo de trabajo de solución de problemas que saca provecho de las herramientas de diagnósticos disponibles.  

Para obtener más información acerca del Software definido a redes de Microsoft, consulta [definido redes Software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de errores  
La siguiente lista representa la clase de los problemas más a menudo con la virtualización de red de Hyper-V (HNVv1) en Windows Server 2012 R2 de implementaciones de producción en el mercado y coincide en muchos aspectos con los mismos tipos de problemas en Windows Server 2016 HNVv2 con la nueva pila de red de definido de Software (SDN).  

La mayoría de errores pueden clasificarse en un pequeño conjunto de clases:   
* **Configuración no válida o no compatible**  
   Un usuario invoca la API NorthBound incorrectamente o con la directiva no válido.   

* **Error en la aplicación de la directiva**  
     Directiva de controlador de red no se ha entregado a un Host de Hyper-V, significativamente retraso o no está actualizada en todos los hosts de Hyper-V (por ejemplo, después de una migración en vivo).  
* **Error de software o desfase de configuración**  
 Problemas de ruta de acceso de datos resultantes en paquetes.  

* **Error externo relacionado con el hardware NIC / controladores o el subposición red fabric**  
 Descarga de tarea que (por ejemplo, VMQ) o subposición fabric de red incorrecta (por ejemplo, MTU)   

 Esta guía de solución de problemas examina cada una de estas categorías de error y recomienda los procedimientos recomendados y herramientas de diagnóstico identificar y corregir el error.  

## <a name="diagnostic-tools"></a>Herramientas de diagnóstico  

Antes de tratar los flujos de trabajo de solución de problemas para cada uno de estos tipos de errores, vamos a examinar las herramientas de diagnóstico disponibles.   
  
Para usar las herramientas de diagnóstico de controlador de red (ruta del control), primero debes instalar la característica RSAT-NetworkController e importar el ``NetworkControllerDiagnostics`` módulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar las herramientas de diagnóstico de diagnósticos HNV (ruta de datos), debes importar el ``HNVDiagnostics`` módulo:
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnóstico del controlador de red  
Estos cmdlets se documentan en TechNet en la [tema de Cmdlet de diagnósticos de controlador de red](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/). Ayudan a identificar problemas con la coherencia de la directiva de red en la ruta de control entre nodos de controlador de red y el controlador de red y los agentes de Host de CN ejecutan en el host de Hyper-V.

 La _depuración ServiceFabricNodeStatus_ y _Get NetworkControllerReplica_ cmdlets deben ejecutarse desde una de las máquinas virtuales de nodo de controlador de red. Todos los otros cmdlets CN diagnóstico se puede ejecutar desde cualquier host que tenga conectividad con el controlador de red y en ninguno en el grupo de seguridad de administración de controladores de red (Kerberos) o tiene acceso al certificado X.509 para administrar el controlador de red. 
   
### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host de Hyper-V  
Estos cmdlets se documentan en TechNet en la [tema de Cmdlet de diagnósticos de virtualización de red de Hyper-V (HNV)](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/). Ayudan a identificar los problemas de la ruta de datos entre las máquinas virtuales de inquilinos (oriental/oeste) y el tráfico de entrada a través de una VIP SLB (norte/sur). 

La _depuración VirtualMachineQueueOperation_, _Get CustomerRoute_, _Get PACAMapping_, _Get ProviderAddress_, _Get VMNetworkAdapterPortId_, _Get VMSwitchExternalPortId_, y _prueba EncapOverheadSettings_ son todas las pruebas locales que se pueden ejecutar desde cualquier host de Hyper-V. Los cmdlets de otros invocar pruebas de ruta de acceso de datos a través del controlador de red y, por tanto, necesita acceso al controlador de red como descried anteriormente.
 
### <a name="github"></a>GitHub
La [repositorio de GitHub de Microsoft o SDN](https://github.com/microsoft/sdn) tiene un número de scripts de muestra y los flujos de trabajo que desarrollar encima de estos cmdlets integrados. En particular, los scripts de diagnósticos pueden encontrarse en la [diagnósticos](https://github.com/Microsoft/sdn/diagnostics) carpeta. Ayúdanos a contribuir a estos scripts al envío de solicitudes de la extracción.

## <a name="troubleshooting-workflows-and-guides"></a>Flujos de trabajo y guías de solución de problemas  

### <a name="hoster-validate-system-health"></a>[Anfitrión] Validar el estado del sistema
Hay un recurso incrustado denominado _el estado de configuración_ en varios de los recursos de controlador de red. Estado de configuración proporciona información sobre el estado del sistema como la coherencia entre la configuración del controlador de red y el estado real (en ejecución) en los hosts de Hyper-V. 

Para comprobar el estado de configuración, ejecuta lo siguiente desde cualquier host de Hyper-V con conectividad al controlador de red.

>[!NOTE] 
>El valor de la *NetworkController* parámetro debe ser el nombre de dominio completo o dirección IP en función del nombre de asunto del X.509 > certificado creado para el controlador de red.
>
>La *credenciales* parámetro solo necesita especificarse si el controlador de red está usando la autenticación de Kerberos (normalmente en las implementaciones VMM). La credencial debe ser de un usuario que está en el grupo de seguridad de administración de controlador de red.

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

A continuación se muestra un mensaje de estado de la configuración de ejemplo:

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
> Hay un error en el sistema donde los recursos de interfaz de red para la NIC de VM SLB Mux tránsito están en un estado de error con el error "Modificador – Host no conectado al mando Virtual". Este error se puede omitir tranquilamente si la configuración IP en el recurso de VM NIC se establece en una dirección IP de grupo de IP de la red lógico durante el tránsito. Hay un error del segundo en el sistema donde están los recursos de interfaz de red para la NIC de máquina virtual de puerta de enlace HNV proveedor en un estado de error con el error "Virtual modificador – PortBlocked". Este error también se puede omitir tranquilamente si se establece la configuración IP en el recurso de VM NIC en null (por diseño).


La siguiente tabla muestra la lista de acciones de seguimiento, mensajes y códigos de error en función del estado de configuración observado.

  
| **Código**| **Mensaje**| **Acción**|  
|:--------:|:-----------:|----------:|  
| Desconocido| Error desconocido| |  
| HostUnreachable                       | El equipo host no es accesible | Comprobar la conectividad de red de administración entre Host y el controlador de red |  
| PAIpAddressExhausted                  | Las direcciones Ip PA agotada | Aumentar el tamaño de grupo de IP de la subred lógica HNV proveedor |  
| PAMacAddressExhausted                 | Las direcciones Mac PA agotada | Aumentar el intervalo de grupo de Mac |  
| PAAddressConfigurationFailure         | No se pudo instalar direcciones PA al host | Comprueba la conectividad de red de administración entre Host y el controlador de red. |  
| CertificateNotTrusted                 | Certificado no es de confianza  |Corregir los certificados usados para la comunicación con el host. |  
| CertificateNotAuthorized              | Certificado no autorizado | Corregir los certificados usados para la comunicación con el host. |  
| PolicyConfigurationFailureOnVfp       | Error al configurar las directivas VFP | Este es un error en tiempo de ejecución.  No hay soluciones definitiva. Recopilar registros. |  
| PolicyConfigurationFailure            | Error en insertarse directivas en los hosts, debido a errores de comunicación o a otros usuarios error en la NetworkController.| Ninguna acción definitiva.  Esto es debido a un error en estado de objetivo de procesamiento de los módulos de controlador de red. Recopilar registros. |  
| HostNotConnectedToController          | El Host no está aún conectado al controlador de red | Perfil de puerto no se aplica en el host o el host no es accesible desde el controlador de red. Validar que clave del registro de HostID coincide con el identificador de instancia del recurso de servidor |  
| MultipleVfpEnabledSwitches            | Hay varios VFp habilitado cambia en el equipo host  | Eliminar uno de los conmutadores, como el agente de Host del controlador de red solo admite un vSwitch con la extensión VFP habilitada |  
| PolicyConfigurationFailure            | No se pudo VNet directivas de inserción para un VmNic debido a errores de certificado o errores de conectividad  | Comprueba si se han implementado certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN de host). También comprueba la conectividad de host con el controlador de red |  
| PolicyConfigurationFailure            | No se pudo vSwitch directivas de inserción para un VmNic debido a errores de certificado o errores de conectividad  | Comprueba si se han implementado certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN de host). También comprueba la conectividad de host con el controlador de red|
| PolicyConfigurationFailure            | No se pudo directivas de Firewall de inserción para un VmNic debido a errores de certificado o errores de conectividad | Comprueba si se han implementado certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN de host). También comprueba la conectividad de host con el controlador de red|
| DistributedRouterConfigurationFailure | No se pudo configurar la configuración del enrutador distribuido en la vNic host                          | Error de la pila TCP/IP. Puede requerir limpiar el vNICs PA y recuperación ante desastres Host en el servidor en el que se informó este error |
| DhcpAddressAllocationFailure          | Error en la asignación de direcciones DHCP para una VMNic                                                    | Comprobar si el atributo de dirección IP estático está configurado en el recurso NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | No pudo conectarse a multiplexar debido a errores de red o certificados | Comprobar el código numérico proporcionado en el código de mensaje de error: Esto corresponde con el código de error de winsock. Errores de certificado tienen un detalle (por ejemplo, no se puede comprobar el certificado, certificado no autorizado, etcetera.) |  
| HostUnreachable                       | MUX está en mal estado (caso común es BGPRouter desconectado) | Sistemas del mismo nivel BGP en el RRAS (máquina virtual BGP) o el conmutador de la parte superior de bastidor (términos de referencia) es inaccesible o no interconexión correctamente. Comprobar la configuración de BGP en recursos equilibrado multiplexor de carga de Software y el sistema del mismo nivel BGP (términos de referencia o RRAS máquina virtual) |  
| HostNotConnectedToController          | Agente de host SLB no está conectado  | Comprueba que se está ejecutando el servicio Agente de Host de SLB; Consulte los registros de agente de host SLB (auto ejecutando) por motivos de por qué, en caso de que el certificado que se presenta el agente de host que ejecuta rechacen todos los SLBM (CN) estado mostrará información matizado  |  
| PortBlocked                           | El puerto VFP está bloqueado, debido a la falta de VNET / directivas ACL | Comprobar si hay otros errores, lo que podría provocar que las directivas no esté configurado. |  
| Sobrecargado                            | Está sobrecargado Loadbalancer MUX  | Problema de rendimiento con MUX |  
| RoutePublicationFailure               | Loadbalancer MUX no está conectado a un enrutador BGP | Comprueba si la Multiplexación tiene conectividad con los enrutadores BGP y ese BGP interconexión está configurado correctamente |  
| VirtualServerUnreachable              | Loadbalancer MUX no está conectado al administrador SLB | Comprobar la conectividad entre SLBM y MUX |  
| QosConfigurationFailure               | No se pudo configurar las directivas de QOS | Ver si hay suficiente ancho de banda disponible para todas las máquinas virtuales si se usa la reserva de QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Comprobar la conectividad de red entre el controlador de red y el Host de Hyper-V (servicio de agente de Host CN)
Ejecutar el *netstat* siguiente comando para validar que hay tres conexiones ESTABLISHED entre el agente de Host CN y los nodos de controlador de red y un socket de escucha en el Host de Hyper-V
- ESCUCHA en TCP:6640 puerto en el Host de Hyper-V (servicio de agente de Host CN)
- Dirección IP en el puerto 6640 CN nodo IP en puertos efímeros (> 32000) del host de dos conexiones establecidas de Hyper-V
- Una conexión establecida de IP de host de Hyper-V en el puerto efímera a la dirección IP del resto de controlador de red en el puerto 6640

>[!NOTE]
>Solo puede haber dos conexiones establecidas en un host de Hyper-V si no hay ningún máquinas virtuales de inquilino implementados en ese host en particular.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Servicios de agente de Host
El controlador de red se comunica con dos servicios de agente de host en los hosts de Hyper-V: agente de Host SLB y CN Host. Es posible que uno o ambos servicios no se está ejecutando. Comprobar su estado y reiniciar si no se está ejecutando.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Comprobar el estado del controlador de red
Si hay no tres ESTABLISHED conexiones o si el controlador de red que dejan de responder, comprueba que todos los nodos y los módulos de servicio están en funcionamiento mediante el uso de los siguientes cmdlets. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Los módulos de servicio de controlador de red son:
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

Comprueba que está ReplicaStatus **listo** y HealthState es **Aceptar**.

En una producción implementación es con un controlador de red de varios nodos, también puedes consultar qué nodo es principal en cada servicio y su estado de réplica individuales.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Comprueba que el estado de réplica esté listo para cada servicio.
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Buscar correspondiente de host y certificados entre cada Host Hyper-V y el controlador de red 
En un Host de Hyper-V, ejecuta los siguientes comandos para comprobar que la HostID corresponde al identificador de instancia de un recurso de servidor en el controlador de red

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

*Corrección* si con SDNExpress scripts o la implementación manual, actualiza la clave de HostId en el registro para que coincida con el identificador de instancia de lo recursos del servidor. Reinicia al agente de Host del controlador de red en el host de Hyper-V (servidor físico) si usar VMM, eliminar el servidor de Hyper-V de VMM y quitar la clave del registro de HostId. A continuación, volver a agregar el servidor a través de VMM.


Comprueba que las huellas digitales de los certificados X.509 utilizados por el host de Hyper-V (el nombre de host será nombre del firmante del certificado) para la comunicación entre el Host de Hyper-V (servicio de agente de Host CN) y los nodos de controlador de red (SouthBound) son los mismos. También comprueba que el certificado del resto del controlador de red tiene el nombre de sujeto *CN =<FQDN or IP>*.

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

También puedes consultar los siguientes parámetros de cada certificado para asegurarte de que el nombre del sujeto es lo que es lo esperado (nombre de host o CN REST FQDN o IP), aún no ha caducado el certificado, y que todas las entidades de certificación en la cadena de certificados se incluyen en la autoridad raíz de confianza.

- Nombre del firmante  
- Fecha de expiración  
- Entidad de certificación raíz de confianza  

*Corrección* si varios certificados tienen el mismo nombre de asunto en el host de Hyper-V, el agente de Host del controlador de red elige al azar uno para presentar al controlador de red. Esto puede no coincidir con la huella digital del recurso de servidor que se sabe que el controlador de red. En este caso, eliminar uno de los certificados con el mismo nombre de asunto en el host de Hyper-V y, a continuación, vuelve a iniciar el servicio Agente de Host de controlador de red. Si aún no pueden hacerse una conexión, elimina el otro certificado con el mismo nombre de asunto en el Host de Hyper-V y eliminar el recurso de servidor correspondiente en VMM. A continuación, volver a crear el recurso de servidor en VMM que se genera un nuevo certificado X.509 e instalarlo en el host de Hyper-V.
  

#### <a name="check-the-slb-configuration-state"></a>Comprobar el estado de la configuración de SLB
Puede determinar el estado de la configuración de SLB como parte de la salida del cmdlet NetworkController de depuración. Este cmdlet también enviará el conjunto actual de recursos de controlador de red en archivos JSON, todas las configuraciones de IP de cada host de Hyper-V (servidor) y la directiva de red local de tablas de base de datos de agente de Host. 

De manera predeterminada, se recopilarán seguimientos adicionales. Para no recopilar seguimientos, agregar IncludeTraces-: $false parámetro.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>La ubicación de salida predeterminada será el directorio de \NCDiagnostics\ < working_directory >. El directorio de resultados predeterminada puede cambiarse mediante la `-OutputDirectory` parámetro. 

La información de estado de la configuración de SLB puede encontrarse en la _diagnósticos slbstateResults.Json_ archivo en este directorio.

Este archivo JSON puede dividirse en las siguientes secciones:
 * Estructura
   * SlbmVips - en esta sección se muestran la dirección IP de la dirección VIP SLB administrador que se usa el controlador de red a la configuración de coodinate y salud entre SLB Muxes y agentes de Host SLB.
   * MuxState - esta sección enumera un valor para cada Mux SLB implementado ofrecer el estado de la multiplexación
   * Configuración del enrutador - esta sección enumera la dirección ascendente del enrutador (BGP del mismo nivel) número de sistema autónomo (ASN), la dirección IP de tránsito y el identificador. También enumerará los SLB Muxes ASN y IP durante el tránsito.
   * Conectado Host información - en esta sección se IP de administración de la lista de direcciones todos los hosts de Hyper-V para ejecutar cargas de trabajo de equilibrio de carga.
   * VIP intervalos - esta sección enumerará los intervalos IP VIP del grupo públicos y privados. La VIP SLBM se incluirán como una dirección IP asignada de uno de estos intervalos. 
   * Rutas MUX - esta sección enumera un valor para cada Mux SLB implementado que contiene todos los anuncios de rutas para ese mux particular.
 * Inquilino
   * VipConsolidatedState - esta sección enumera el estado de conectividad para cada VIP inquilino como prefijo de ruta anunciado, Host de Hyper-V y extremos DIP.
    
> [!NOTE]
> Se puede comprobar el estado de SLB directamente mediante el [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script está disponible en la [repositorio GitHub SDN de Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validación de puerta de enlace

**Desde el controlador de red:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Desde la puerta de enlace de máquina virtual:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Desde la parte superior del conmutador (términos de referencia):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP enrutador**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Además, de los problemas que hemos visto hasta ahora (especialmente en las implementaciones en función de SDNExpress), la razón más común para compartimento inquilino no obtener configurado en máquinas virtuales GW parece que el hecho de que la capacidad de GW en FabricConfig.psd1 es menor en relación con lo que a tus amigos intentan asignar a las conexiones de red (S2S túneles) TenantConfig.psd1. Esto puede revisarse fácilmente mediante la comparación de resultados de los siguientes comandos:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Anfitrión] Validar datos plano
Después de que el controlador de red se haya implementado, se han creado las subredes y las redes virtuales inquilino y máquinas virtuales se adjuntaron con las subredes virtuales, el anfitrión para comprobar la conectividad de inquilino pueden realizar pruebas de nivel de fabric adicionales.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Comprobar la conectividad de red lógica de proveedor HNV
Después del primer invitado de la máquina virtual que se ejecuta en un host de Hyper-V se ha conectado a una red virtual del inquilino, el controlador de red asignará dos HNV de direcciones IP de proveedor (direcciones IP de PA) en el Host de Hyper-V. Estos IPs provienen del grupo de IP de la red lógica HNV proveedor y administrarse mediante el controlador de red.  Para saber cuáles son estas dos direcciones IP de HNV de

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

Estas direcciones IP de proveedor de HNV (PA IPs) se asignan a los adaptadores Ethernet creado en un compartimento de red TCPIP independiente y tiene un nombre de adaptador de _VLANX_ donde X está la VLAN asignada a la red lógica de proveedor HNV (transporte).

Conectividad entre dos hosts de Hyper-V con el proveedor de HNV red lógica puede realizarse mediante un comando "ping" con un compartimento adicional (-c Y) donde Y es el compartimento de red TCPIP en la que se creó el PAhostVNICs de parámetro. Este compartimento se puede determinar mediante la ejecución de:

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
> Los adaptadores de vNIC PA Host no se usan en la ruta de acceso a datos y por lo tanto, no tienen una dirección IP asignada a la "adaptador vEthernet (PAhostVNic)".

Por ejemplo, supongamos que los host de Hyper-V 1 y 2 tienen direcciones IP de proveedor HNV (PA) de:

|Host hyper-V-|-Dirección IP de PA 1|-Dirección IP de PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

te podemos hacer ping entre las dos mediante el siguiente comando para comprobar la conectividad de red lógica HNV proveedor.

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

*Corrección* ping proveedor HNV si no funciona, comprobar la conectividad de red física, incluida la configuración de VLAN. El NIC físicas en cada host de Hyper-V deben estar en modo de tronco con ningún VLAN específico asignado. La vNIC de Host de administración deben aislar dentro de VLAN de la red lógica de administración.

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Comprobar la compatibilidad de MTU y el marco de gigante en HNV proveedor de red

Otro problema común en la red de proveedor HNV lógica es que los puertos de red física o una tarjeta Ethernet no tienen una MTU lo suficientemente grande como configurada para controlar la sobrecarga de encapsulación VXLAN (o NVGRE). 
>[!NOTE]
> Algunas tarjetas Ethernet y controladores que admite el nuevo * palabra clave EncapOverhead que se establecerá automáticamente el agente de Host del controlador de red con un valor de 160. Este valor, a continuación, se agregará al valor de la * palabra clave de JumboPacket cuyo resumen se usa como la MTU anunciada.
> Por ejemplo, * EncapOverhead = 160 y * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para comprobar si la red lógica de proveedor HNV es compatible con la mayor MTU tamaño-to-end, usa el _prueba LogicalNetworkSupportsJumboPacket_ cmdlet:
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
* Ajustar el tamaño MTU en los puertos conmutador físico para ser al menos 1674B (incluido el encabezado de 14 ter Ethernet y tráiler)
* Si la tarjeta NIC no admite la palabra clave EncapOverhead, ajustar la palabra clave para que sea al menos JumboPacket 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Comprobar la conectividad de inquilino VM NIC
Cada NIC de máquina virtual asignado a un máquina virtual de invitado tiene una asignación de CA-PA entre la dirección de cliente privada (CA) y el espacio de direcciones de proveedor de HNV (PA). Estas asignaciones se mantienen en las tablas de servidor OVSDB en cada host de Hyper-V y pueden encontrarse ejecutando el siguiente cmdlet.

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
> Si no se envían las asignaciones de CA-PA esperas para un determinado inquilino VM, ponte en contacto los recursos de VM NIC y la configuración IP en el controlador de red mediante el _Get NetworkControllerNetworkInterface_ cmdlet. Además, comprueba las conexiones establecidas entre los nodos de agente de Host de CN y el controlador de red.

Con esta información, ahora se puede iniciar un comando "ping" VM de inquilino el anfitrión del controlador de red mediante el _prueba VirtualNetworkConnection_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Escenarios específicos de solución de problemas

Las siguientes secciones proporcionan instrucciones para escenarios específicos de la solución de problemas.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Sin conectividad de red entre dos máquinas virtuales de inquilino.

1.  [Inquilino] Asegúrate de que Firewall de Windows en máquinas virtuales de inquilino no esté bloqueando el tráfico.  
2.  [Inquilino] Comprueba que las direcciones IP asignadas a la máquina virtual de inquilino ejecutando _ipconfig_. 
3.  [Anfitrión] Ejecutar **prueba VirtualNetworkConnection** desde el host de Hyper-V para validar la conectividad entre las máquinas virtuales de dos inquilino en cuestión. 

>[!NOTE]
>La VSID hace referencia al identificador de subred Virtual. En el caso de VXLAN, este es el identificador de red VXLAN (VNI). Puedes encontrar este valor ejecutando el **Get PACAMapping** cmdlet.

#### <a name="example"></a>Ejemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Crear CA-ping entre "Verde Web VM 1" con SenderCA IP de 192.168.1.4 en Host "sa18n30-2.sa18.nttest.microsoft.com" con Mgmt IP de 10.127.132.153 ListenerCA IP de 192.168.1.5 asociados a subred Virtual (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Prueba de inicio CA espacio ping inicio de sesión de seguimiento Ping a 192.168.1.5 realizada desde direcciones 192.168.1.4 Rtt = 0 ms


Información de enrutamiento de entidad emisora de certificados:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Información de enrutamiento PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [Inquilino] Comprueba que no hay ninguna directiva distribuida firewall especificada en la subred virtual o interfaces de red de la máquina virtual que se bloquearán tráfico.    

Consulta la API de REST de controlador de red se encuentra en el entorno de demostración en sa18n30nc en el dominio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Observa la configuración IP y subredes Virtual que se hace referencia a esta ACL

1. [Anfitrión] Ejecutar ``Get-ProviderAddress`` en ambas Hyper-V hosts que hospeda los dos máquinas virtuales en cuestión de inquilinos y, a continuación, ejecutar ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` desde el host de Hyper-V para validar la conectividad en la red lógica de proveedor HNV
2.  [Anfitrión] Asegúrate de que la configuración de MTU es correctos en los hosts de Hyper-V y cualquier nivel 2 cambiar los dispositivos entre los Hosts de Hyper-V. Ejecutar ``Test-EncapOverheadValue`` en todos los hosts de Hyper-V en cuestión. También comprueba que todos los conmutadores de nivel 2 en el medio tienen MTU establecida en bytes 1674 menos en cuenta la sobrecarga máxima de 160 bytes.  
3.  [Anfitrión] Si las direcciones IP PA no están presentes o se interrumpió la conectividad de entidad emisora de certificados, comprueba si se ha recibido la directiva de red. Ejecutar ``Get-PACAMapping`` para ver si las reglas de encapsulación y asignaciones de CA-PA necesarias para crear redes virtuales de superposición se establecen correctamente.  
4.  [Anfitrión] Comprueba que el agente de Host del controlador de red está conectado al controlador de red. Ejecutar ``netstat -anp tcp |findstr 6640`` para ver si el   
5.  [Anfitrión] Comprueba que el Host del identificador en HKLM / coincide con el identificador de instancia de los recursos de servidor que hospeda las máquinas virtuales de inquilino.  
6. [Anfitrión] Comprueba que el identificador de perfil de puerto coincide con el identificador de instancia de las Interfaces de red de la máquina virtual de las máquinas virtuales de inquilino.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro, el seguimiento y diagnóstico avanzado

Las secciones siguientes proporcionan información de diagnóstico avanzada, registro y seguimiento.

### <a name="network-controller-centralized-logging"></a>Registro del controlador centralizado de red 
 
El controlador de red automáticamente puede recopilar registros del depurador y almacenarlos en una ubicación centralizada. Colección de registro puede habilitarse al implementar el controlador de red para la primera vez o en cualquier momento más adelante. Los registros se recopilan desde el controlador de red y elementos que administrados por el controlador de red de red: hospedar máquinas, equilibradores de carga de software (SLB) y equipos de puerta de enlace. 

Estos registros incluyen los registros de depuración para el clúster de controlador de red, la aplicación de controlador de red, los registros de puerta de enlace, SLB, redes virtuales y el firewall distribuido. Cuando se agrega un nuevo host/SLB/puerta de enlace en el controlador de red, se inicia el registro en esos equipos. Del mismo modo, cuando se quita un host o SLB o la puerta de enlace desde el controlador de red, se detiene el registro en esos equipos.

#### <a name="enable-logging"></a>Habilitar el registro

Registro se habilita automáticamente cuando se instala el clúster de controlador de red utilizando el **instalación NetworkControllerCluster** cmdlet. De manera predeterminada, los registros se recopilan localmente en los nodos del controlador de red en *%systemdrive%\SDNDiagnostics*. Es **recomienda** que cambiar esta ubicación para que sea un recurso compartido de archivos remotos (no local). 

Los registros de clúster de controlador de red se almacenan en *%programData%\Windows Fabric\log\Traces*. Puedes especificar una ubicación centralizada para la colección de registro con la **DiagnosticLogLocation** parámetro con la recomendación que también es ser un recurso compartido de archivos. 

Si quieres restringir el acceso a esta ubicación, puedes proporcionar las credenciales de acceso con el **LogLocationCredential** parámetro. Si proporcionas las credenciales para acceder a la ubicación del registro, también debes proporcionar el **CredentialEncryptionCertificate** parámetro, que se usa para cifrar las credenciales almacenadas localmente en los nodos del controlador de red.  

Con la configuración predeterminada, se recomienda que tiene al menos 75 GB de espacio libre en la ubicación central y 25 GB en los nodos locales (si no usas una ubicación central) para un clúster de controlador de red de 3 nodos.

#### <a name="change-logging-settings"></a>Cambiar la configuración de registro

Puede cambiar la configuración de registro en cualquier momento mediante la ``Set-NetworkControllerDiagnostic`` cmdlet. Se puede cambiar la configuración siguiente:

- **Ubicación del registro centralizada**.  Puedes cambiar la ubicación para almacenar todos los registros, con la ``DiagnosticLogLocation`` parámetro.  
- **Las credenciales para acceder a la ubicación del registro**.  Puedes cambiar las credenciales para acceder a la ubicación del registro, con el ``LogLocationCredential`` parámetro.  
- **Ir al inicio de sesión local**.  Si has proporcionado una ubicación centralizada para almacenar los registros, puede volver a iniciar sesión localmente en los nodos del controlador de red con el ``UseLocalLogLocation`` parámetro (no recomendado debido a requisitos de espacio en disco grande).  
- **Ámbito de registro**.  De manera predeterminada, todos los registros se recopilan. Puedes cambiar el ámbito para recopilar registros de clúster de controlador de red solo.  
- **Nivel de registro**.  El nivel de registro predeterminado es informativa. Puedes cambiar a Error, advertencia o detallado.  
- **Registro de tiempo de caducidad**.  Los registros se almacenan de forma circular. Tendrá tres días de datos de registro de manera predeterminada, si usas iniciar sesión local o centralizada. Puedes cambiar este límite de tiempo con **LogTimeLimitInDays** parámetro.  
- **Registro de tamaño de antigüedad**.  De manera predeterminada, tendrás un 75 GB máximo de datos de registro si usa el registro centralizado y 25 GB si usas el registro local. Puedes cambiar este límite con la **LogSizeLimitInMBs** parámetro.

#### <a name="collecting-logs-and-traces"></a>Seguimientos y recopilar registros

Las implementaciones de VMM usan registro centralizado para el controlador de red de manera predeterminada. La ubicación del recurso compartido de estos registros se especifica al implementar la plantilla de servicio de controlador de red.

Si una ubicación de archivo no se ha especificado, local registro se usará en cada nodo del controlador de red con los registros guardados en C:\Windows\tracing\SDNDiagnostics. Estos registros se guardan con la jerarquía de la siguiente:

- Volcados de sucesos
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Seguimientos

El controlador de red usa a (Azure) Service Fabric. Service Fabric registros pueden ser necesarios al solucionar algunos problemas. Estos registros se pueden encontrar en cada nodo del controlador de red en C:\ProgramData\Microsoft\Service Fabric.

Si un usuario ha ejecutado el _depuración NetworkController_ cmdlet, registros adicionales estará disponibles en los hosts de Hyper-V que se ha especificado con un recurso de servidor en el controlador de red. Estos registros (y trazas si habilitado) se mantienen bajo C:\NCDiagnostics

### <a name="slb-diagnostics"></a>SLB diagnósticos

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errores de SLBM Fabric (acciones de proveedor de servicio de hospedaje)

1.  Comprobar que funciona Software carga equilibrado Manager (SLBM) y que las capas de la organización pueden comunicarse entre sí: SLBM -> SLB Mux y SLBM -> SLB los agentes de Host. Ejecutar [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) desde cualquier nodo con acceso al extremo de REST de controlador de red.  
2.  Validar la *SDNSLBMPerfCounters* en PerfMon en uno del nodo del controlador de red máquinas virtuales (preferiblemente el controlador de red nodo principal - Get NetworkControllerReplica):
    1.  ¿Está conectado el motor de carga equilibrado (LB) a SLBM? (*SLBM LBEngine configuraciones Total* > 0)  
    2.  ¿SLBM al menos sabe acerca de sus propio extremos? (*VIP extremos Total* > = 2)  
    3.  ¿Son hosts de Hyper-V (DIP) conectados a SLBM? (*Los clientes de HP conectados* == num servidores)   
    4.  ¿Está conectado SLBM a Muxes? (*Muxes conectado* == *Muxes correcto en SLBM* == *Muxes informes saludable* = # SLB Muxes virtuales).  
3.  Asegúrate de que el enrutador BGP configurado correctamente es interconexión con el MUX SLB  
    1.  Si utiliza RRAS con acceso remoto (es decir, máquina virtual BGP):  
        1.  Get-BgpPeer debe mostrar conectado  
        2.  Get-BgpRouteInformation debe mostrar al menos una ruta para el SLBM self VIP  
    2.  Si utiliza física parte superior de bastidor (términos de referencia) conmutador como BGP del mismo nivel, consulte la documentación  
        1.  Por ejemplo: # mostrar bgp instancia  
4.  Validar la *SlbMuxPerfCounters* y *SLBMUX* contadores de rendimiento en la máquina virtual Mux de SLB
5.  Comprobar el estado de configuración y los intervalos de VIP en recursos de administrador de equilibrado de carga de Software  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| ConvertTo json-profundidad 8 (comprobar intervalos VIP en grupos de IP y garantizar SLBM self-VIP (*LoadBalanacerManagerIPAddress*) y cualquier VIPs inquilino frontal estos intervalos)  
        1. Get-NetworkControllerIpPool - Id. de red "< pública y privada VIP lógico recursos Id. de red >" - SubnetId "< pública y privada VIP lógico subred identificador de recurso >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | convertto json-profundidad 8 
    2.  Depuración-NetworkControllerConfigurationState-  

Si alguna de las comprobaciones por encima de los errores, el estado SLB inquilino también estará en un modo de error.  

**Corrección**   
En función de la siguiente información de diagnóstico presentada, corregir las siguientes acciones:  
* Asegúrate de que están conectados SLB multiplexores  
  * Solucionar problemas de certificado  
  * Solucionar problemas de conectividad de red  
* Asegúrese de información de interconexión BGP está configurado correctamente  
* Asegúrate de identificador de instancia de servidor en el recurso de servidor coincida con el identificador de Host en el registro (hacer referencia al apéndice para *HostNotConnected* código de error)  
* Recopilar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errores de inquilino SLBM (Hosting servicio proveedor e inquilino acciones)

1.  [Anfitrión] Comprobar *depuración NetworkControllerConfigurationState* para ver si los recursos de la dirección en un estado de error. Intenta mitigar siguiendo los elementos de acción tabla en el apéndice.   
    1.  Comprueba que el extremo de una VIP esté presente y rutas de publicidad  
    2.  Comprobar cuántos extremos DIP se descubrieron del extremo VIP  
2.  [Inquilino] Validar carga equilibrado recursos se especifican correctamente  
    1.  Validar DIP extremos que se registran en SLBM aloja máquinas virtuales de inquilino que corresponden a las configuraciones de IP del grupo de dirección LoadBalancer Back-end  
3.  [Anfitrión] Si los extremos DIP no se detectan ni conectados:   
    1.  Comprobar *NetworkControllerConfigurationState de depuración*  
        1.  Validar ese CN y SLB Host agente está correctamente conectado mediante Coordinador de eventos de controlador de red ``netstat -anp tcp |findstr 6640)``  
    2.  Comprobar *HostId* en *nchostagent* clave de registro de servicio (referencia *HostNotConnected* código de error en el apéndice) coincide con el identificador de la instancia del recurso de servidor correspondiente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Comprueba el identificador de instancia del recurso correspondiente máquina virtual NIC coincide con el identificador de perfil de puerto para el puerto de máquina virtual   
4.  [Proveedor de hospedaje] Recopilar registros   

#### <a name="slb-mux-tracing"></a>SLB Mux seguimiento

Información de la Muxes equilibrado de carga de Software también puede determinarse a través del Visor de eventos. 
1. Haz clic en "Mostrar registros analíticos y de depuración" en el menú Ver del Visor de eventos
2. Ve a "Registros de aplicaciones y servicios" > Microsoft > Windows > SlbMuxDriver > en el Visor de eventos de seguimiento 
3. Haga clic con el botón secundario en él y selecciona "Habilitar registro"

>[!NOTE]
>Se recomienda que solo tiene este registro habilitado durante un rato mientras está intentando reproducir un problema

### <a name="vfp-and-vswitch-tracing"></a>VFP y vSwitch seguimiento

Desde cualquier Hyper-V host que hospeda un Invitado VM conectado a una red virtual del inquilino, puede recopilar un seguimiento VFP para determinar dónde es posible que se encuentran problemas.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
