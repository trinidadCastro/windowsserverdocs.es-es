---
title: Creación de directivas de seguridad con listas de control de acceso de puerto extendido
description: En este tema se proporciona información sobre Access Control Lists (ACL) en Windows Server 2016 de puerto extendido.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d847213f0332b57ae38ada444d7a6cd98ab325ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848986"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>Creación de directivas de seguridad con listas de control de acceso de puerto extendido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre Access Control Lists (ACL) en Windows Server 2016 de puerto extendido. Puede configurar ACL de extendidas en el conmutador virtual de Hyper-V para permitir y bloquear el tráfico de red hacia y desde las máquinas virtuales (VM) que se encuentran conectadas al conmutador a través de adaptadores de red virtual.  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Reglas de ACL detalladas](#bkmk_detailed)  
  
-   [Reglas de ACL con estado](#bkmk_stateful)  
  
## <a name="bkmk_detailed"></a>Reglas de ACL detalladas  
Conmutador Virtual de Hyper-V ACL extendidas le permiten crear reglas detalladas que se pueden aplicar a adaptadores de red VM individuales que están conectados al conmutador Virtual de Hyper-V. La capacidad de crear reglas detalladas permite a las empresas y proveedores de servicios en la nube (CSP) para resolver las amenazas de seguridad basada en red en un entorno de servidor compartido para varios inquilinos.  
  
Con las ACL extendidas, en lugar de tener que crear reglas generales que bloquean o permiten todo el tráfico desde todos los protocolos hacia o desde una VM, ahora es posible bloquear o permitir el tráfico de red de protocolos individuales que se ejecutan en VM. Puede crear reglas de ACL extendidas en Windows Server 2016 que incluyen el siguiente conjunto de 5-tuplas de parámetros: dirección IP, dirección IP de destino, protocolo, puerto de origen y puerto de destino de origen. Además, cada regla puede especificar la dirección del tráfico de red (entrante o saliente) y la acción a la que se refiere la regla (bloquear o permitir el tráfico).  
  
Por ejemplo, puede configurar ACL de puerto para que una VM permita todo el tráfico de HTTP y HTTPS entrante y saliente en el puerto 80 mientras bloquea el tráfico de red de todos los demás protocolos en todos los puertos.  
  
Esta posibilidad de designar el tráfico de protocolo que pueden recibir o no las VM de inquilinos aporta flexibilidad a la hora de configurar directivas de seguridad.  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>Configurar reglas de ACL con Windows PowerShell  
Para configurar una ACL extendida, debe usar el comando de Windows PowerShell **Add-VMNetworkAdapterExtendedAcl**. Este comando tiene cuatro sintaxis diferentes, cada una de ellas con un uso diferenciado:  
  
1.  Agregar una ACL extendida a todos los adaptadores de red de una máquina virtual con nombre: que se especifica mediante el primer parámetro, - VMName. Sintaxis:  
  
    > [!NOTE]  
    > Si desea agregar una ACL extendida a un adaptador de red en lugar de todo, puede especificar el adaptador de red con el parámetro - VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  Agregar una ACL extendida a un adaptador de red virtual específico en una VM determinada. Sintaxis:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  Agregar una ACL extendida a todos los adaptadores de red virtual que están reservados para que los use el sistema operativo de administración de hosts de Hyper-V.  
  
    > [!NOTE]  
    > Si desea agregar una ACL extendida a un adaptador de red en lugar de todo, puede especificar el adaptador de red con el parámetro - VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  Agregar una ACL extendida a un objeto de máquina virtual que ha creado en Windows PowerShell, como **$vm = get-vm "mi_vm"**. En la siguiente línea de código puede ejecutar este comando para crear una ACL extendida con la siguiente sintaxis:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>Ejemplos de reglas de ACL detalladas  
A continuación, se ofrecen varios ejemplos de cómo usar el comando **Add-VMNetworkAdapterExtendedAcl** para configurar ACL de puerto extendido y para crear directivas de seguridad para VM.  
  
-   [Aplicar la seguridad de nivel de aplicación](#bkmk_enforce)  
  
-   [Aplicar la seguridad de nivel de usuario y nivel de aplicación](#bkmk_both)  
  
-   [Proporcionar compatibilidad con seguridad a una aplicación que no es TCP/UDP](#bkmk_tcp)  
  
> [!NOTE]  
> Los valores del parámetro de regla **Direction** que figura en las tablas siguientes se basan en el flujo de tráfico hacia o desde la VM para la que se crea la regla. Si la VM recibe tráfico, dicho tráfico es entrante; si la VM envía tráfico, dicho tráfico es saliente. Por ejemplo, si aplica una regla a una VM que bloquea el tráfico entrante, la dirección del tráfico entrante es desde los recursos externos hacia la VM. Si aplica una regla que bloquea el tráfico saliente, la dirección del tráfico saliente es desde la VM local hacia los recursos externos.  
  
### <a name="bkmk_enforce"></a>Aplicar la seguridad de nivel de aplicación  
Debido a que muchos servidores de aplicaciones usan puertos TCP/UDP estandarizados para comunicarse con equipos cliente, resulta sencillo crear reglas que bloqueen o permitan el acceso a un servidor de aplicaciones filtrando el tráfico que se dirige y que proviene del puerto designado a la aplicación.  
  
Por ejemplo, es posible que desee permitir que un usuario se conecte a un servidor de aplicaciones de su centro de datos usando Conexión a Escritorio remoto (RDP). RDP usa el puerto TCP 3389, por lo que se puede configurar rápidamente la siguiente regla:  
  
|IP de origen|IP de destino|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|Acercar|Permitir|  
  
A continuación, se ofrecen dos ejemplos de cómo crear reglas con comandos de Windows PowerShell. La regla del primer ejemplo bloquea todo el tráfico a la máquina virtual denominada "ApplicationServer". La segunda regla de ejemplo, que se aplica al adaptador de red de la máquina virtual denominada "ApplicationServer", permite solo el tráfico RDP entrante a la máquina virtual.  
  
> [!NOTE]  
> Al crear reglas, puede usar el **-peso** parámetro para determinar el orden en el que el conmutador Virtual de Hyper-V procesa las reglas. Los valores de **-peso** se expresan como enteros; las reglas con un entero superior se procesan antes que las reglas con enteros inferiores. Por ejemplo, si ha aplicado dos reglas al adaptador de red de una VM, una con un valor Weight de 1 y otra con un valor Weight de 10, se aplica primero la regla con el valor de 10.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="bkmk_both"></a>Aplicar la seguridad de nivel de usuario y nivel de aplicación  
Una regla puede corresponderse con un paquete de IP de tupla 5 (IP de origen, IP de destino, Protocolo, Puerto de origen y Puerto de destino); por este motivo, la regla puede imponer una directiva de seguridad más detallada que una ACL de puerto.  
  
Por ejemplo, si desea prestar servicio DHCP a un número limitado de cliente en equipos con un conjunto específico de servidores DHCP, puede configurar las reglas siguientes en el equipo de Windows Server 2016 que se está ejecutando Hyper-V, donde se hospedan las VM del usuario:  
  
|IP de origen|IP de destino|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|Alejar|Permitir|  
|*|10.175.124.0/25|UDP|*|67|Alejar|Permitir|  
|10.175.124.0/25|*|UDP|*|68|Acercar|Permitir|  
  
A continuación, se ofrecen ejemplos de cómo crear estas reglas con comandos de Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="bkmk_tcp"></a>Proporcionar compatibilidad con seguridad a una aplicación que no es TCP/UDP  
Si bien la mayoría del tráfico de red en un centro de datos es TCP y UDP, hay una parte del tráfico que utiliza otros protocolos. Por ejemplo, si desea permitir que un grupo de servidores ejecute una aplicación de transmisión por multidifusión de IP basada en el Protocolo de administración de grupos de Internet (IGMP), puede crear la siguiente regla.  
  
> [!NOTE]  
> IGMP tiene un número de protocolo IP designado de 0x02.  
  
|IP de origen|IP de destino|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|Acercar|Permitir|  
|*|*|0x02|*|*|Alejar|Permitir|  
  
A continuación, se ofrece un ejemplo de cómo crear estas reglas con comandos de Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="bkmk_stateful"></a>Reglas de ACL con estado  
Otra nueva capacidad de las ACL extendidas le permite configurar reglas con estado. Una regla con estado filtra los paquetes según cinco atributos en un paquete - dirección IP de origen, IP de destino, protocolo, puerto de origen y puerto de destino.  
  
Las reglas con estado tienen las siguientes capacidades:  
  
-   Siempre permiten el tráfico y no se usan para bloquearlo.  
  
-   Si especifica que el valor del parámetro **Direction** es de entrada y el tráfico coincide con la regla, el conmutador virtual de Hyper-V crea de forma dinámica una regla coincidente que permite a la VM enviar tráfico de salida en respuesta al recurso externo.  
  
-   Si especifica que el valor del parámetro **Direction** es de salida y el tráfico coincide con la regla, el conmutador virtual de Hyper-V crea de forma dinámica una regla coincidente que permite que la VM reciba el tráfico de entrada del recurso externo.  
  
-   Incluyen un atributo de tiempo de espera que se mide en segundos. Cuando un paquete de red llega al conmutador y dicho paquete coincide con una regla con estado, el conmutador virtual de Hyper-V crea un estado para que se permitan todos los paquetes posteriores de ambas direcciones del mismo flujo. El estado expira si no se registra tráfico en ninguna dirección durante el periodo de tiempo especificado por el valor de tiempo de espera.  
  
A continuación, se ofrece un ejemplo de cómo se pueden utilizar las reglas de estado.  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>Permitir el tráfico de entrada de servidor remoto solo después de que lo haya contactado el servidor local  
En algunos casos, se debe emplear una regla con estado porque solamente una regla con estado puede realizar el seguimiento de una conexión conocida establecida y diferenciar la conexión de otras.  
  
Por ejemplo, si desea permitir que un servidor de aplicaciones de VM inicie conexiones en el puerto 80 con servicios web en Internet y quiere que los servidores web remotos puedan responder al tráfico de VM, es posible configurar una regla con estado que permita el tráfico de salida inicial desde la VM a los servicios web; al tratarse de una regla con estado, también se permite el tráfico de retorno hacia la VM desde los servidores web. Por motivos de seguridad, puede bloquear todo el tráfico de red de entrada restante hacia la VM.  
  
Para conseguir esta configuración de las reglas, puede usar los ajustes de la tabla siguiente.  
  
> [!NOTE]  
> Debido a limitaciones de formato y a la cantidad de información que aparece en la tabla siguiente, la información se muestra de forma distinta a como se hace en las tablas anteriores de este documento.  
  
|Parámetro|Regla 1|Regla 2|Regla 3|  
|-------------|----------|----------|----------|  
|IP de origen|*|*|*|  
|IP de destino|*|*|*|  
|Protocolo|*|*|TCP|  
|Puerto de origen|*|*|*|  
|Puerto de destino|*|*|80|  
|Dirección|Acercar|Alejar|Alejar|  
|Acción|Denegar|Denegar|Permitir|  
|Con estado|No|No|Sí|  
|Tiempo de espera (en segundos)|N/D|N/D|3600|  
  
La regla con estado permite al servidor de aplicaciones de VM conectarse a un servidor web remoto. Cuando se envía el primer paquete, el conmutador virtual de Hyper-V crea dinámicamente dos estados de flujo para permitir todos los paquetes enviados al servidor web remoto y todos los paquetes retornados de este. Cuando se detiene el flujo de paquetes entre los servidores, los estados de flujo inician un tiempo de espera en el valor correspondiente designado de 3600 segundos, equivalente a una hora.  
  
A continuación, se ofrece un ejemplo de cómo crear estas reglas con comandos de Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


