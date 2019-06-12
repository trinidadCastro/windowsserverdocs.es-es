---
title: Solución de problemas de la pila de redes definidas por software de Windows Server
description: Esta guía de Windows Server examina los escenarios de error y errores comunes de redes definidas por Software (SDN) y describe un flujo de trabajo de solución de problemas que aprovecha las herramientas de diagnóstico disponibles.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: eeb0c335e4afd3c6835a04421a15073aeab6cdc6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446244"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Solución de problemas de la pila de redes definidas por software de Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía examina los errores comunes de redes definidas por Software (SDN) y los escenarios de error y describe un flujo de trabajo de solución de problemas que aprovecha las herramientas de diagnóstico disponibles.  

Para obtener más información acerca de redes definidas por Software de Microsoft, consulte [redes definidas por Software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de errores  
La siguiente lista representa la clase de problemas más frecuentes con virtualización de red de Hyper-V (HNVv1) en Windows Server 2012 R2 de las implementaciones de producción en el mercado y que coincida en muchos aspectos con los mismos tipos de problemas se han observado en Windows Server 2016 HNVv2 con la nueva pila de red definida por Software (SDN).  

Mayoría de los errores se puede clasificar en un pequeño conjunto de clases:   
* **Configuración no válida o no compatible**  
   Un usuario llama a la API NorthBound incorrectamente o con la directiva no válido.   

* **Error en la aplicación de directiva**  
     Directiva de controladora de red no se entregó a un Host de Hyper-V, significativamente diferida o no está actualizada en todos los hosts de Hyper-V (por ejemplo, después de una migración en vivo).  
* **Error de software o de desfase de configuración**  
  Problemas de ruta de acceso de datos resultante en paquetes descartados.  

* **Error externo relacionado con el hardware NIC / controladores o el subposición tejido de red**  
  Un comportamiento erróneo de las descargas de tarea (por ejemplo, VMQ) o el tejido de red subposición mal configurada (por ejemplo, la MTU)   

  Esta guía de solución de problemas examina cada una de estas categorías de error y recomienda mejores prácticas y herramientas de diagnóstico identificar y corregir el error.  

## <a name="diagnostic-tools"></a>Herramientas de diagnóstico  

Antes de tratar los flujos de trabajo de solución de problemas para cada uno de estos tipos de errores, vamos a examinar las herramientas de diagnóstico disponibles.   

Para usar las herramientas de diagnóstico de la controladora de red (ruta de control), primero debe instalar la característica de RSAT NetworkController e importar el ``NetworkControllerDiagnostics`` módulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar las herramientas de diagnóstico de diagnóstico de HNV (ruta de datos), debe importar el ``HNVDiagnostics`` módulo:

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnóstico del controlador de red  
Estos cmdlets se documentan en TechNet en el [tema del Cmdlet de diagnóstico de red controlador](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Ayudan a identificar los problemas de coherencia de la directiva de red en la ruta de control entre los nodos de controladora de red y entre la controladora de red y los agentes de Host de la controladora de red que se ejecutan en los hosts de Hyper-V.

 El _depuración ServiceFabricNodeStatus_ y _Get NetworkControllerReplica_ cmdlets deben ejecutarse desde una de las máquinas virtuales de nodo de controladora de red. Todos los demás cmdlets de diagnóstico de controladora de red se puede ejecutar desde cualquier host que tenga conectividad con la controladora de red y se encuentra en un grupo de seguridad de administración de la controladora de red (Kerberos) o tiene acceso al certificado X.509 para administrar la controladora de red. 

### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host de Hyper-V  
Estos cmdlets se documentan en TechNet en el [tema de Cmdlet de diagnóstico de virtualización de red de Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Ayudan a identificar los problemas de la ruta de datos entre las máquinas virtuales de inquilino (este/oeste) y el tráfico de entrada a través de una dirección VIP de SLB (norte/sur). 

El _depuración VirtualMachineQueueOperation_, _Get CustomerRoute_, _Get PACAMapping_, _Get ProviderAddress_, _Get VMNetworkAdapterPortId_, _Get VMSwitchExternalPortId_, y _prueba EncapOverheadSettings_ son todas las pruebas locales que se pueden ejecutar desde cualquier host de Hyper-V. Otros cmdlets de la invocación de pruebas de la ruta de acceso de datos a través de la controladora de red y, por tanto, necesitan tener acceso a la controladora de red como descried anteriormente.

### <a name="github"></a>GitHub
El [repositorio de GitHub Microsoft/SDN](https://github.com/microsoft/sdn) tiene un número de secuencias de comandos de ejemplo y los flujos de trabajo que se basan en estos cmdlets en el cuadro. En concreto, las secuencias de comandos de diagnóstico pueden encontrarse en el [diagnósticos](https://github.com/Microsoft/sdn/diagnostics) carpeta. Ayúdenos a contribuir a estas secuencias de comandos mediante el envío de solicitudes de incorporación de cambios.

## <a name="troubleshooting-workflows-and-guides"></a>Solución de problemas de flujos de trabajo y guías  

### <a name="hoster-validate-system-health"></a>[Hosting] Validar el estado del sistema
Hay un recurso incrustado denominado _el estado de configuración_ en varios de los recursos de la controladora de red. Estado de configuración proporciona información sobre el estado del sistema incluida la coherencia entre la configuración de la controladora de red y el estado real (en ejecución) en los hosts de Hyper-V. 

Para comprobar el estado de configuración, ejecute lo siguiente desde cualquier host de Hyper-V con la conectividad a la controladora de red.

>[!NOTE] 
>El valor de la *NetworkController* parámetro debe ser la dirección IP o FQDN, según el nombre de sujeto de X.509 > certificado creado para la controladora de red.
>
>El *credencial* parámetro solo debe especificarse si la controladora de red utiliza la autenticación de Kerberos (típica en las implementaciones de VMM). La credencial debe ser un usuario que se encuentra en el grupo de seguridad de administración de controlador de red.

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
> Hay un error en el sistema donde los recursos de la interfaz de red para la NIC de máquina virtual de SLB/Mux tránsito están en un estado de error con el error "Virtual Switch – no conectado a controladora de Host". Este error puede omitirse si se establece la configuración de IP en el recurso de NIC de VM en una dirección IP del grupo de direcciones IP de la red lógica de tránsito. Hay un segundo error en el sistema donde están los recursos de la interfaz de red para las NIC de VM del proveedor de HNV de puerta de enlace en un estado de error con el error "Virtual Switch – PortBlocked". Este error puede también se puede pasar por alto si la configuración de IP en el recurso de NIC de VM se establece en null (por diseño).


En la tabla siguiente se muestra la lista de códigos de error, mensajes y acciones de seguimiento que tomar en función del estado de configuración observado.


| **Código**| **Mensaje**| **Acción**|  
|--------|-----------|----------|  
| Unknown| Error desconocido| |  
| HostUnreachable                       | El equipo host no está accesible | Compruebe la conectividad de red de administración entre el Host y la controladora de red |  
| PAIpAddressExhausted                  | Las direcciones Ip PA agotada | Aumentar el tamaño del grupo de IP de subred lógica de proveedor de HNV |  
| PAMacAddressExhausted                 | Las direcciones Mac de PA agotada | Aumentar el intervalo de grupo de Mac |  
| PAAddressConfigurationFailure         | No se pudo asociar las direcciones de PA al host | Compruebe la conectividad de red de administración entre la controladora de red y el Host. |  
| CertificateNotTrusted                 | Certificado no es de confianza  |Corregir los certificados usados para la comunicación con el host. |  
| CertificateNotAuthorized              | Certificado no autorizado | Corregir los certificados usados para la comunicación con el host. |  
| PolicyConfigurationFailureOnVfp       | Error al configurar las directivas de VFP | Se trata de un error en tiempo de ejecución.  No hay soluciones definitiva. Recopilación de registros. |  
| PolicyConfigurationFailure            | Error en la inserción de directivas en los hosts, debido a errores de comunicación u otros errores en el NetworkController.| No hay acciones definitivas.  Esto es debido a un error en el estado de objetivo de procesamiento en los módulos de controlador de red. Recopilación de registros. |  
| HostNotConnectedToController          | El Host no está conectado aún a la controladora de red | Perfil de puerto no se aplica en el host o el host no es accesible desde la controladora de red. Validar que HostID clave de registro coincide con el identificador de instancia del recurso de servidor |  
| MultipleVfpEnabledSwitches            | Hay varios VFp habilitado modificadores en el host  | Eliminar uno de los modificadores, puesto que el agente de Host del controlador de red solo admite un vSwitch con la extensión VFP habilitada |  
| PolicyConfigurationFailure            | No se pudo insertar las directivas de red virtual para una VmNic debido a errores de certificado o errores de conectividad  | Compruebe si se han implementado los certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN del host). También compruebe la conectividad de host con la controladora de red |  
| PolicyConfigurationFailure            | No se pudo insertar un vSwitch directivas para una VmNic debido a errores de certificado o errores de conectividad  | Compruebe si se han implementado los certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN del host). También compruebe la conectividad de host con la controladora de red|
| PolicyConfigurationFailure            | No se pudo insertar las directivas de Firewall para una VmNic debido a errores de certificado o errores de conectividad | Compruebe si se han implementado los certificados apropiados (nombre de sujeto del certificado debe coincidir con el FQDN del host). También compruebe la conectividad de host con la controladora de red|
| DistributedRouterConfigurationFailure | No se pudo establecer la configuración del enrutador distribuido en la vNic de host                          | Error de la pila de TCP/IP. Puede requerir limpiar las VNIC de PA y Host de recuperación ante desastres en el servidor en el que se notificó este error |
| DhcpAddressAllocationFailure          | Error de asignación de direcciones DHCP para una VMNic                                                    | Compruebe si el atributo de dirección IP estático se configura en el recurso NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | No se pudo conectar a Mux debido a errores de red o certificado | Compruebe el código numérico proporcionado en el código de mensaje de error: Esto corresponde al código de error de winsock. Errores de certificado son granulares (por ejemplo, no se puede comprobar el certificado, certificado no autorizado, etcetera.) |  
| HostUnreachable                       | MUX está en estado incorrecto (caso común es BGPRouter desconectado) | BGP del mismo nivel en el conmutador Top of Rack (ToR) o la (máquina virtual BGP) de RRAS es inaccesible o no emparejamiento correctamente. Compruebe la configuración de BGP en el recurso de equilibrador de carga de Software multiplexor y BGP del mismo nivel (ToR o RRAS máquina virtual) |  
| HostNotConnectedToController          | Agente de host SLB no está conectado  | Compruebe que se está ejecutando el servicio del agente de Host de SLB; Consulte los registros de agente de host SLB (auto ejecuta) por motivos de por qué, en caso de SLBM (NC) rechazó el certificado presentado por el agente de host que se ejecuta estado mostrará información matizado  |  
| PortBlocked                           | El puerto VFP está bloqueado, debido a la falta de red virtual o las directivas de ACL | Compruebe si hay otros errores, lo que podrían producir las directivas que no se ha configurado. |  
| Sobrecargado                            | LoadBalancer MUX está sobrecargado  | Problema de rendimiento con MUX |  
| RoutePublicationFailure               | LoadBalancer MUX no está conectado a un enrutador BGP | Compruebe si el MUX tiene conectividad con los enrutadores BGP y ese par BGP está configurada correctamente |  
| VirtualServerUnreachable              | LoadBalancer MUX no está conectado a Administrador de SLB | Compruebe la conectividad entre SLBM y MUX |  
| QosConfigurationFailure               | No se pudo configurar las directivas de QOS | Ver si hay suficiente ancho de banda disponible para todas las máquinas virtuales si se usa la reserva de QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Compruebe la conectividad de red entre la controladora de red y el Host de Hyper-V (servicio de agente de Host de la controladora de red)
Ejecute el *netstat* siguiente comando para validar que hay tres conexiones establecidas entre el agente de Host de la controladora de red y los nodos de controladora de red y un socket de escucha en el Host de Hyper-V
- ESCUCHA en puerto TCP:6640 en el Host de Hyper-V (servicio de agente de Host de controladora de red)
- Dirección IP en el puerto 6640 a la dirección IP del nodo de controladora de red en los puertos efímeros (32000 >) del host de dos conexiones establecidas de Hyper-V
- Una conexión establecida de dirección IP del host de Hyper-V en los puertos efímeros para IP de REST de controladora de red en el puerto 6640

>[!NOTE]
>Solo puede haber dos conexiones establecidas en un host de Hyper-V si no hay ninguna máquina virtual de inquilino implementada en dicho host particular.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Compruebe los servicios de agente de Host
La controladora de red se comunica con dos servicios del agente de host en los hosts de Hyper-V: Agente de Host SLB y agente de Host de la controladora de red. Es posible que uno o ambos de estos servicios no se está ejecutando. Compruebe su estado y reinicie si no se está ejecutando.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Comprobar el estado de la controladora de red
Si hay conexiones de ESTABLISHED no tres, o si la controladora de red aparece no responde, compruebe que todos los nodos y los módulos de servicio están en marcha mediante los siguientes cmdlets. 

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

Compruebe que está ReplicaStatus **listo** y HealthState es **Aceptar**.

En una producción implementación es con una controladora de red de varios nodos, también puede comprobar qué nodo es principal en cada servicio y su estado de réplica individual.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Compruebe que el estado de réplica está preparada para cada servicio.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Comprobación de certificados entre la controladora de red y en cada Host de Hyper-V y de host correspondientes 
En un Host de Hyper-V, ejecute los comandos siguientes para comprobar que el HostID corresponde al identificador de instancia de un recurso de servidor en la controladora de red

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

*Corrección* si mediante SDNExpress secuencias de comandos o la implementación manual, actualice la HostId clave en el registro para que coincida con el identificador de instancia del recurso de servidor. Reinicie al agente de Host del controlador de red en el host de Hyper-V (servidor físico) si usa VMM, elimine el servidor de Hyper-V en VMM y quitar la clave del registro de HostId. A continuación, vuelva a agregar el servidor a través de VMM.


Compruebe que las huellas digitales de los certificados X.509 usados por el host de Hyper-V (el nombre de host será nombre de sujeto del certificado) para la comunicación entre el Host de Hyper-V (servicio de agente de Host de la controladora de red) y los nodos de controladora de red (SouthBound) son los mismos. Compruebe también que el certificado REST de la controladora de red tiene el nombre de asunto *CN =<FQDN or IP>* .

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

También puede comprobar los siguientes parámetros de cada certificado para asegurarse de que el nombre de sujeto es lo que es lo esperado (nombre de host o FQDN de REST de controladora de red o IP), el certificado no ha expirado todavía, y que se incluyen todas las entidades emisoras de certificados en la cadena de certificados en la raíz de confianza autoridad.

- Nombre del firmante  
- Fecha de expiración  
- Entidad de certificación raíz de confianza  

*Corrección* si varios certificados tienen el mismo nombre de sujeto en el host de Hyper-V, el agente de Host del controlador de red elegirá aleatoriamente uno para presentar a la controladora de red. Esto puede no coincidir con la huella digital del recurso de servidor sabe que la controladora de red. En este caso, elimine uno de los certificados con el mismo nombre de sujeto en el host de Hyper-V y, a continuación, vuelva a iniciar el servicio del agente de Host de controlador de red. Si todavía no pueden realizarse una conexión, elimine el otro certificado con el mismo nombre de sujeto en el Host de Hyper-V y el recurso de servidor correspondiente en VMM. A continuación, volver a crear el recurso en VMM que se genere un nuevo certificado X.509 e instálelo en el host de Hyper-V server.


#### <a name="check-the-slb-configuration-state"></a>Comprobar el estado de configuración de SLB
Como parte de la salida al cmdlet Debug-NetworkController se puede determinar el estado de configuración de SLB. Este cmdlet también dará como resultado el conjunto actual de los recursos de la controladora de red en archivos JSON, todas las configuraciones de IP de cada host de Hyper-V (servidor) y la directiva de red local desde las tablas de base de datos del agente de Host. 

De forma predeterminada, se recopilará seguimientos adicionales. Para no recopilar seguimientos, agregue IncludeTraces-: parámetro $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>La ubicación de salida predeterminado será el directorio de \NCDiagnostics\ < working_directory >. El directorio de salida predeterminado puede cambiarse mediante la `-OutputDirectory` parámetro. 

La información de estado de configuración de SLB puede encontrarse en el _diagnósticos slbstateResults.Json_ archivo en este directorio.

Este archivo JSON puede dividirse en las secciones siguientes:
 * Tejido
   * SlbmVips: en esta sección se muestra la dirección IP de la dirección VIP del Administrador de SLB, que es utilizada por la controladora de red a coodinate configuración y el estado entre el SLB/MUX y agentes de Host de SLB.
   * MuxState - esta sección mostrará un valor para cada SLB Mux implementada que proporciona el estado del MUX
   * Configuración del enrutador: en esta sección mostrará una lista de nivel superior del enrutador (BGP del mismo nivel) número de sistema autónomo (ASN), dirección IP de tránsito e identificador. También mostrará el ASN de MUX de SLB y la IP de tránsito.
   * Conectado a la información de Host: en esta sección se lista la dirección IP de administración de direcciones todos los hosts de Hyper-V disponibles para ejecutar cargas de trabajo con equilibrio de carga.
   * Intervalos de dirección VIP: esta sección enumera los intervalos de grupos de IP de VIP públicos y privados. La dirección VIP de SLBM se incluirá como una dirección IP asignada desde uno de estos intervalos. 
   * Rutas MUX - en esta sección mostrará un valor para cada SLB Mux implementada que contiene todos los anuncios de rutas que MUX determinado.
 * Inquilino
   * VipConsolidatedState - esta sección enumera el estado de conectividad para cada VIP de inquilino, incluido el prefijo de ruta anunciada, Host de Hyper-V y los puntos de conexión DIP.

> [!NOTE]
> Se puede comprobar el estado de SLB directamente mediante el [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script está disponible en el [repositorio de GitHub de SDN de Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validación de la puerta de enlace

**Controladora de red:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Desde la máquina virtual de la puerta de enlace:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Desde la parte superior del conmutador de Rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Enrutador BGP de Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Además, desde los problemas que hemos visto hasta ahora (sobre todo en las implementaciones en función de SDNExpress), parecen ser el hecho de que la capacidad de puerta de enlace en FabricConfig.psd1 es menor en comparación con lo que la razón más común para el compartimiento de inquilino no obtener configurado en máquinas virtuales de puerta de enlace personas intentan asignar a las conexiones de red (túneles S2S) en TenantConfig.psd1. Puede comprobar fácilmente mediante la comparación de las salidas de los siguientes comandos:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Hosting] Validar el plano de datos
Una vez se ha implementado la controladora de red, se han creado redes virtuales de inquilinos y las subredes y máquinas virtuales asociadas a las subredes virtuales, se pueden realizar pruebas de nivel de tejido adicional por el proveedor de hospedaje para comprobar la conectividad del inquilino.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Compruebe la conectividad de red lógica del proveedor HNV
Después de la primera Invitado VM que se ejecutan en un host de Hyper-V se ha conectado a una red virtual de inquilino, la controladora de red asignará dos direcciones de IP de proveedor de HNV (direcciones IP de PA) para el Host de Hyper-V. Estas direcciones IP procederán de grupo de la red lógica del proveedor de HNV de direcciones IP y administrarse mediante la controladora de red.  Para averiguar cuáles son estas dos direcciones IP de HNV de

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

Estas direcciones IP de proveedor de HNV (PA IPs) se asignan a los adaptadores Ethernet creado en un compartimento de red TCP/IP independiente y tiene un nombre de adaptador de _VLANX_ donde X es la VLAN asignada a la red lógica del proveedor de HNV (transporte).

Conectividad entre dos hosts de Hyper-V mediante el proveedor de HNV red lógica puede hacerse mediante un ping con un compartimiento adicional (-c Y) donde x es el compartimento de red TCP/IP en la que se creó el PAhostVNICs del parámetro. Este compartimiento puede determinarse mediante la ejecución:

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
> Los adaptadores de vNIC de Host de PA no se usan en la ruta de acceso a datos y por lo que no tiene una dirección IP asignada a la "adaptador vEthernet (PAhostVNic)".

Por ejemplo, supongamos que los hosts de Hyper-V 1 y 2 tienen direcciones IP de HNV de proveedor (PA) de:

|De Host hyper-V-|-Dirección IP PA 1|-Dirección IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

nos podemos hacer ping entre ellas con el siguiente comando para comprobar la conectividad de red lógica del proveedor de HNV.

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

*Corrección* ping de proveedor de HNV si no funciona, compruebe la conectividad de red física, incluida la configuración de VLAN. Las NIC físicas en cada host de Hyper-V deben estar en modo de tronco con ninguna VLAN específica asignada. La vNIC de Host de administración deben aislarse a VLAN de la red lógica de administración.

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

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Comprobar compatibilidad de MTU y gigantes marco en la red lógica del proveedor de HNV

Otro problema común en la red lógica del proveedor de HNV es que los puertos de red físico o una tarjeta Ethernet no tiene una MTU lo suficientemente grande como configurada para controlar la sobrecarga de encapsulación VXLAN (o NVGRE). 
>[!NOTE]
> Algunas tarjetas Ethernet y controladores de admitan los nuevos * palabra clave EncapOverhead que se establecerá automáticamente el agente de Host del controlador de red en un valor de 160. Este valor, a continuación, se agregará al valor de la * palabra clave de JumboPacket cuya suma se utiliza como la MTU anunciada.
> Por ejemplo, * EncapOverhead = 160 y * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para probar si la red lógica del proveedor de HNV es compatible con la mayor MTU tamaño-to-end, use el _prueba LogicalNetworkSupportsJumboPacket_ cmdlet:
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
* Ajustar el tamaño MTU en los puertos de conmutador físico sea como mínimo 1674B (incluidos 14B Ethernet encabezado y finalizador)
* Si la tarjeta NIC no es compatible con la palabra clave EncapOverhead, ajuste la palabra clave para que sea al menos JumboPacket 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Compruebe la conectividad NIC de máquina virtual de inquilino
Cada NIC de máquina virtual asignada a una máquina virtual invitada tiene una asignación entre CA y PA entre la dirección privada de cliente (CA) y el espacio de direcciones de proveedor de HNV (PA). Estas asignaciones se mantienen en las tablas de servidor OVSDB en cada host de Hyper-V y pueden encontrarse ejecutando el siguiente cmdlet.

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
> Si no se devuelven las asignaciones entre CA y PA que espera para una máquina virtual del inquilino determinado, compruebe los recursos de VM NIC y la configuración IP de la controladora de red usando el _Get NetworkControllerNetworkInterface_ cmdlet. Además, compruebe las conexiones establecidas entre los nodos de agente de Host de la controladora de red y la controladora de red.

Con esta información, ahora se puede iniciar un ping de la máquina virtual de inquilino mediante el proveedor de hospedaje de la controladora de red mediante el _prueba VirtualNetworkConnection_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Escenarios de solución de problemas específicos

Las secciones siguientes proporcionan instrucciones para solucionar problemas de escenarios concretos.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>No hay conectividad de red entre dos máquinas virtuales de inquilinos

1.  [Inquilino] Asegúrese de que Firewall de Windows en máquinas virtuales de inquilinos no está bloqueando el tráfico.  
2.  [Inquilino] Compruebe que las direcciones IP asignadas a la máquina virtual de inquilino mediante la ejecución de _ipconfig_. 
3.  [Hosting] Ejecute **prueba VirtualNetworkConnection** desde el host de Hyper-V para validar la conectividad entre las máquinas virtuales de dos inquilino en cuestión. 

>[!NOTE]
>El VSID se refiere a que el identificador de subred Virtual. En el caso de VXLAN, este es el identificador de red VXLAN (VNI). Puede encontrar este valor mediante la ejecución de la **Get PACAMapping** cmdlet.

#### <a name="example"></a>Ejemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Crear entidad de certificación-ping entre "Verde Web VM 1" con SenderCA IP de 192.168.1.4 en Host "sa18n30-2.sa18.nttest.microsoft.com" con Mgmt IP de 10.127.132.153 ListenerCA IP de 192.168.1.5 asociados a subred Virtual (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Iniciando prueba de ping de espacio CA inicio de sesión de seguimiento Ping a 192.168.1.5 se realizó correctamente desde la dirección 192.168.1.4 Rtt = 0 ms


Información de enrutamiento de la entidad de certificación:

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

4. [Inquilino] Compruebe que no hay ninguna directiva de firewall distribuido que especificó en la subred virtual o las interfaces de red de máquina virtual que deben bloquear el tráfico.    

Consultar la API de REST de controladora de red se encuentra en el entorno de demostración en sa18n30nc en el dominio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examine la configuración de IP y subredes virtuales que hacen referencia a esta ACL

1. [Hosting] Ejecute ``Get-ProviderAddress`` en Hyper-V hosts aloja las dos máquinas virtuales en cuestión de inquilinos y, a continuación, ejecute ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` desde el host de Hyper-V para validar la conectividad en la red lógica del proveedor de HNV
2.  [Hosting] Asegúrese de que la configuración de MTU es correctas en los hosts de Hyper-V y en cualquier nivel 2 dispositivos entre los Hosts de Hyper-V de conmutación. Ejecute ``Test-EncapOverheadValue`` en todos los hosts de Hyper-V en cuestión. Asegúrese también de que todos los conmutadores de capa 2 entre dispone MTU establecida en bytes 1674 mínimos para tener en cuenta para la sobrecarga de máxima de bytes de 160.  
3.  [Hosting] Si las direcciones IP PA no están presentes o se interrumpe la conectividad de la entidad emisora de certificados, compruebe que se ha recibido la directiva de red. Ejecute ``Get-PACAMapping`` para ver si las reglas de encapsulación y asignaciones de CA-PA necesarias para crear redes virtuales de superposición se establecen correctamente.  
4.  [Hosting] Compruebe que el agente de Host del controlador de red está conectado a la controladora de red. Ejecute ``netstat -anp tcp |findstr 6640`` para ver si el   
5.  [Hosting] Compruebe que el Host del identificador en HKLM / coincide con el identificador de instancia de los recursos de servidor que hospeda las máquinas virtuales de inquilino.  
6. [Hosting] Compruebe que el identificador de perfil de puerto coincide con el identificador de instancia de las Interfaces de red de máquina virtual de las máquinas virtuales de inquilino.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro, seguimiento y diagnósticos avanzados

Las secciones siguientes proporcionan información de diagnóstico avanzada, registro y seguimiento.

### <a name="network-controller-centralized-logging"></a>Controladora de red centralizadas de registro 

La controladora de red automáticamente puede recopilar registros del depurador y almacenarlos en una ubicación centralizada. Recopilación de registros puede habilitarse al implementar la controladora de red para la primera vez o en cualquier momento más adelante. Los registros se recopilan de la controladora de red y los elementos administrados por controladora de red de red: equipos, los equilibradores de carga de software (SLB) y máquinas de puerta de enlace de host. 

Estos registros incluyen registros de depuración para el clúster de controladora de red, la aplicación de la controladora de red, los registros de puerta de enlace, SLB, una red virtual y el firewall distribuido. Cada vez que se agrega un nuevo host/SLB/puerta de enlace a la controladora de red, se inicia el registro en esas máquinas. De forma similar, cuando se quita un host/SLB/puerta de enlace de la controladora de red, se detiene el registro en esas máquinas.

#### <a name="enable-logging"></a>Habilitar registro

El registro se habilita automáticamente cuando se instala el clúster de controladora de red con el **Install NetworkControllerCluster** cmdlet. De forma predeterminada, los registros se recopilan localmente en los nodos de controladora de red en *%systemdrive%\SDNDiagnostics*. Es **recomienda** que cambiar esta ubicación para que sea un recurso compartido de archivos remoto (no local). 

Los registros de clúster de la controladora de red se almacenan en *%programData%\Windows Fabric\log\Traces*. Puede especificar una ubicación centralizada para la recopilación de registros con el **DiagnosticLogLocation** parámetro con la recomendación que se trata también ser un recurso compartido de archivos remoto. 

Si desea restringir el acceso a esta ubicación, puede proporcionar las credenciales de acceso con el **LogLocationCredential** parámetro. Si proporciona las credenciales para tener acceso a la ubicación del registro, también debe proporcionar el **CredentialEncryptionCertificate** parámetro, que se utiliza para cifrar las credenciales almacenadas localmente en los nodos de controladora de red.  

Con la configuración predeterminada, se recomienda que tenga al menos de 75 GB de espacio libre en la ubicación central y 25 GB en los nodos locales (si no usa una ubicación central) para un clúster de la controladora de red de 3 nodos.

#### <a name="change-logging-settings"></a>Cambiar la configuración de registro

Puede cambiar la configuración del registro en cualquier momento mediante el ``Set-NetworkControllerDiagnostic`` cmdlet. Se puede cambiar la configuración siguiente:

- **Ubicación de registro centralizada**.  Puede cambiar la ubicación para almacenar todos los registros con el ``DiagnosticLogLocation`` parámetro.  
- **Las credenciales para tener acceso a la ubicación del registro**.  Puede cambiar las credenciales para tener acceso a la ubicación del registro, con el ``LogLocationCredential`` parámetro.  
- **Mover al registro local**.  Si ha especificado una ubicación centralizada para almacenar los registros, puede regresar a la sesión de forma local en los nodos de controladora de red con el ``UseLocalLogLocation`` parámetro (no recomendado debido a requisitos de espacio en disco de gran tamaño).  
- **Ámbito de registro**.  De forma predeterminada, todos los registros se recopilan. Puede cambiar el ámbito para recopilar registros de clúster de controladora de red únicos.  
- **Nivel de registro**.  El nivel de registro predeterminado es Informational. Se puede cambiar a Error, advertencia o detallado.  
- **Tiempo de caducidad del registro**.  Los registros se almacenan de manera circular. Tendrá tres días de datos de registro de forma predeterminada, si se usa el registro local o registro centralizado. Puede cambiar este límite de tiempo con **LogTimeLimitInDays** parámetro.  
- **Tamaño de la antigüedad del registro**.  De forma predeterminada, tendrá un máximo 75 GB de datos de registro si usa el registro centralizado y 25 GB si se utiliza el registro local. Puede cambiar este límite con el **LogSizeLimitInMBs** parámetro.

#### <a name="collecting-logs-and-traces"></a>Los seguimientos y recopilación de registros

Las implementaciones de VMM usan registro centralizado para la controladora de red de forma predeterminada. Al implementar la plantilla de servicio de controladora de red, se especifica la ubicación de recurso compartido de archivos para estos registros.

Si una ubicación de archivo no se ha especificado, local se utilizará el registro en cada nodo de la controladora de red con los registros guardados en C:\Windows\tracing\SDNDiagnostics. Estos registros se guardan con la siguiente jerarquía:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Seguimientos

La controladora de red usa (Azure) de Service Fabric. Registros de Service Fabric pueden ser necesarios al solucionar ciertos problemas. Estos registros pueden encontrarse en cada nodo de la controladora de red en el tejido C:\ProgramData\Microsoft\Service.

Si un usuario se ha ejecutado el _depuración NetworkController_ cmdlet, registros adicionales estarán disponibles en cada host de Hyper-V que se ha especificado con un recurso de servidor en la controladora de red. Estos registros (y los seguimientos si habilitado) se mantienen en C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnósticos SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errores del tejido de SLBM (acciones de proveedor de servicio de hospedaje)

1.  Compruebe que el Administrador de equilibrador de carga de Software (SLBM) está funcionando y que las capas de orquestación pueden comunicarse entre sí: SLBM -> SLB/Mux y SLBM -> a agentes de Host SLB. Ejecute [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) desde cualquier nodo con acceso al extremo de REST de controladora de red.  
2.  Validar la *SDNSLBMPerfCounters* en Monitor de rendimiento en una de las máquinas virtuales (preferentemente la controladora de red nodo principal - Get-NetworkControllerReplica) de nodos de controladora de red:
    1.  ¿Está conectado el motor de Load Balancer (LB) a SLBM? (*SLBM LBEngine configuraciones Total* > 0)  
    2.  ¿SLBM al menos sabe acerca de sus propios puntos de conexión? (*Total de puntos de conexión VIP* > = 2)  
    3.  ¿Están los hosts de Hyper-V (DIP) conectados a SLBM? (*Clientes HP conectados* == num servidores)   
    4.  ¿Está conectado SLBM a MUX? (*MUX conectado* == *MUX correcto en el SLBM* == *MUX un estado correcto* = # las máquinas virtuales de SLB MUX).  
3.  Asegúrese de que el enrutador BGP configurado correctamente es el emparejamiento del SLB MUX  
    1.  Si el uso de RRAS con acceso remoto (es decir, máquina virtual BGP):  
        1.  Get-BgpPeer debe aparecer como conectado  
        2.  Get-BgpRouteInformation debe mostrar al menos una ruta para SLBM self VIP  
    2.  Si utiliza físico parte superior del Rack (ToR) conmutador como par BGP, consulte la documentación  
        1.  Por ejemplo: # mostrar instancia de bgp  
4.  Validar la *SlbMuxPerfCounters* y *SLB/MUX* contadores en Monitor de rendimiento en la máquina virtual Mux de SLB
5.  Compruebe el estado de configuración y los intervalos de dirección VIP en Manager de equilibrador de carga de Software Resource  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| convertto-json-profundidad 8 (Compruebe los intervalos de dirección VIP en grupos de direcciones IP y asegúrese de self-VIP de SLBM (*LoadBalanacerManagerIPAddress*) y cualquier VIP de orientación del inquilino se encuentran dentro de estos intervalos)  
        1. "< Pública-privada VIP lógico Resource Id. de red >" Get-NetworkControllerIpPool - NetworkId - SubnetId "< pública-privada VIP lógico Resource ID de subred >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | convertto-json-8 de profundidad 
    2.  Depuración-NetworkControllerConfigurationState-  

Si alguna de las comprobaciones anteriores presenta algún error, el estado SLB de inquilino también estará en un modo de error.  

**Corrección**   
Según la información de diagnóstico siguiente presentan, corregir lo siguiente:  
* Asegúrese de que SLB multiplexores está conectados  
  * Solucionar problemas de certificados  
  * Corregir problemas de conectividad de la red  
* Asegúrese de información sobre el emparejamiento BGP está configurado correctamente  
* Asegúrese de Id. de instancia de servidor en el recurso del servidor coincide con el identificador de Host en el registro (apéndice para hacer referencia a *HostNotConnected* código de error)  
* Recopilar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errores de inquilinos de SLBM (hospedaje proveedor y el inquilino de las acciones de servicio)

1.  [Hosting] Comprobar *depuración NetworkControllerConfigurationState* para ver si cualquier recurso de equilibrador de carga está en un estado de error. Intentan mitigar siguiendo los elementos de acción de tabla en el apéndice.   
    1.  Compruebe que un punto de conexión VIP está presente y anunciar rutas  
    2.  Comprobar cuántos puntos de conexión DIP se han detectado para el extremo de VIP  
2.  [Inquilino] Validar los recursos de equilibrador de carga se han especificado correctamente  
    1.  Validar DIP extremos que están registrados en SLBM hospedan máquinas virtuales de inquilinos que corresponden a las configuraciones de IP del grupo de direcciones de Back-end de LoadBalancer  
3.  [Hosting] Si los puntos de conexión DIP no son detectadas o conectados:   
    1.  Comprobar *NetworkControllerConfigurationState de depuración*  
        1.  Validar ese NC y agente de Host de SLB esté correctamente conectado al Coordinador de eventos del controlador de red mediante ``netstat -anp tcp |findstr 6640)``  
    2.  Comprobar *HostId* en *nchostagent* servicio regkey (referencia *HostNotConnected* código de error en el apéndice) coincide con la instancia del recurso de servidor correspondiente (Id. ``Get-NCServer |convertto-json -depth 8``)  
    3.  Compruebe el Id. de instancia del recurso correspondiente máquina virtual NIC coincide con el Id. de perfil de puerto para el puerto de la máquina virtual   
4.  [Hospedaje de proveedor] Recopilación de registros   

#### <a name="slb-mux-tracing"></a>Seguimiento de SLB/Mux

También se puede determinar la información de la MUX de equilibrador de carga de Software a través del Visor de eventos. 
1. Haga clic en "Mostrar registros analíticos y de depuración" en el menú Vista del Visor de eventos
2. Vaya a "Registros de aplicaciones y servicios" > Microsoft > Windows > SlbMuxDriver > en el Visor de eventos de seguimiento 
3. Haga clic con el botón derecho en él y seleccione "Habilitar el registro"

>[!NOTE]
>Se recomienda que solo tienen este registro habilitado durante un breve tiempo mientras se está intentando reproducir un problema

### <a name="vfp-and-vswitch-tracing"></a>VFP y seguimiento de vSwitch

Desde cualquier Hyper-V host que hospeda un Invitado VM asociada a una red virtual de inquilino, puede recopilar un seguimiento VFP para determinar dónde pueden haber problemas.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
