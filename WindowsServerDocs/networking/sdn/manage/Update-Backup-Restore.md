---
title: Actualizar copia de seguridad y restauración de infraestructura de SDN
description: En este tema, aprenderá a actualizar, de copia de seguridad y restauración de una infraestructura de SDN.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7916377f58261d0ccaa3fa24f135fccca3d5e79b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446334"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Actualizar copia de seguridad y restauración de infraestructura de SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a actualizar, de copia de seguridad y restauración de una infraestructura de SDN. 

## <a name="upgrade-the-sdn-infrastructure"></a>Actualizar la infraestructura de SDN
Infraestructura de SDN se puede actualizar desde Windows Server 2016 para Windows Server 2019. Para la actualización de ordenación, siga la misma secuencia de pasos como se mencionó en la sección "Actualizar la infraestructura de SDN". Antes de la actualización, se recomienda realizar una copia de seguridad de la base de datos de la controladora de red.

Para las máquinas de la controladora de red, use el cmdlet Get-NetworkControllerNode para comprobar el estado del nodo de una vez completada la actualización. Asegúrese de que el nodo vuelve al estado "activo" antes de actualizar los demás nodos. Una vez que se ha actualizado todos los nodos de controladora de red, la controladora de red actualiza los microservicios que se ejecutan en el clúster de controladora de red dentro de una hora. Puede desencadenar una actualización inmediata mediante el cmdlet update-networkcontroller. 

Instale las mismas actualizaciones de Windows en todos los componentes del sistema operativo del sistema de redes definidas por Software (SDN), que incluye:

- SDN habilitado hosts de Hyper-V
- Máquinas virtuales de controladora de red
- Máquinas virtuales Mux de equilibrador de carga de software
- Máquinas virtuales de puerta de enlace RAS 

>[!IMPORTANT]
>Si usa System Center Virtual Manager, debe actualizarla con los paquetes acumulativos de actualizaciones más recientes.

Cuando se actualiza cada componente, puede usar cualquiera de los métodos estándares para la instalación de actualizaciones de Windows. Sin embargo, para garantizar un tiempo de inactividad mínimo para las cargas de trabajo y la integridad de la base de datos de la controladora de red, siga estos pasos:

1. Actualice las consolas de administración.<p>Instale las actualizaciones en cada uno de los equipos donde se utilice el módulo de Powershell de controlador de red.  Incluido en cualquier lugar que tienen el rol de RSAT NetworkController instalado por sí mismo. Exclusión de las máquinas virtuales de controlador de red a sí mismos; actualícelas en el paso siguiente.

2. En la primera VM de controladora de red, instale todas las actualizaciones y reinicie.

3. Antes de continuar con la siguiente máquina virtual de controlador de red, utilice el `get-networkcontrollernode` cmdlet para comprobar el estado del nodo que se ha actualizado y reiniciado.

4. Durante el ciclo de reinicio, espere a que el nodo de la controladora de red ir hacia abajo y, a continuación, vuelva a intentarlo.<p>Después de reiniciar la máquina virtual, puede tardar varios minutos antes de enviarlos a la **_seguridad_** estado. Para obtener un ejemplo de la salida, vea 

5. Instalar actualizaciones en cada máquina virtual de Mux de SLB uno por uno para garantizar la disponibilidad continua de la infraestructura de equilibrador de carga.

6. Actualizar los hosts de Hyper-V y las puertas de enlace RAS, empezando por los hosts que contienen las puertas de enlace RAS que se encuentran en **Standby** modo.<p>Las máquinas virtuales de puerta de enlace RAS no se puede migrar en vivo sin perder las conexiones del inquilino. Durante el ciclo de actualización, debe tener cuidado para minimizar el número de tiempos de conmutación por error de las conexiones a una puerta de enlace RAS de inquilinos. Mediante la coordinación de la actualización de los hosts y las puertas de enlace RAS, cada inquilino conmuta por error una vez, como máximo.

    a. Evacuar el host de máquinas virtuales que son capaces de migración en vivo.<p>Las máquinas virtuales de puerta de enlace RAS debe permanecer en el host.

    b. Instalar actualizaciones en cada máquina virtual de puerta de enlace en este host.

    c. Si la actualización requiere el reinicio de máquina virtual de puerta de enlace, a continuación, reinicie la máquina virtual.  

    d. Instalar actualizaciones en el host que contiene la máquina virtual que se haya actualizado de puerta de enlace.

    e. Si es necesario para las actualizaciones, reinicie el host.

    f. Repita para cada host adicional que contiene una puerta de enlace en espera.<p>Si no permanecen puertas de enlace en espera, a continuación, seguir estos mismos pasos para todos los demás hosts.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Por ejemplo: Use el cmdlet get-networkcontrollernode 

En este ejemplo, verá la salida para el `get-networkcontrollernode` cmdlet se ejecuta desde dentro de una de las máquinas virtuales de controlador de red.  

El estado de los nodos que se ven en la salida de ejemplo es:

- NCNode1.contoso.com = hacia abajo
- NCNode2.contoso.com: copia de seguridad
- NCNode3.contoso.com: copia de seguridad

>[!IMPORTANT]
>Debe esperar varios minutos hasta que el estado para los cambios en el nodo _**seguridad**_ antes de actualizar los nodos adicionales, uno en uno.

Una vez que haya actualizado todos los nodos de controladora de red, la controladora de red actualiza los microservicios que se ejecutan en el clúster de controladora de red dentro de una hora. 

>[!TIP]
>Puede desencadenar una actualización inmediata mediante la `update-networkcontroller` cmdlet.


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Por ejemplo: Use el cmdlet update-networkcontroller
En este ejemplo, verá la salida para el `update-networkcontroller` para forzar el controlador de red para la actualización. 

>[!IMPORTANT]
>Ejecute este cmdlet cuando no tiene más actualizaciones para instalar.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Copia de seguridad de la infraestructura de SDN

Copias de seguridad periódicas de la base de datos de la controladora de red garantiza la continuidad empresarial en caso de una desastre o pérdida de datos.  La copia de seguridad de las máquinas virtuales de controlador de red no es suficiente porque no garantiza que la sesión sigue en varios nodos de controladora de red.

**Requisitos:**
* Un recurso compartido SMB y las credenciales con permisos de lectura/escritura para el sistema de archivos y recursos compartidos.
* También puede usar una cuenta de servicio administrada de grupo (GMSA) si se instaló el controlador de red con una GMSA también.

**Procedimiento:**

1. Utilice el método de copia de seguridad de máquina virtual de su elección o usar Hyper-V para exportar una copia de cada máquina virtual de controlador de red.<p>La copia de seguridad de la máquina virtual de controlador de red garantiza que los certificados necesarios para descifrar la base de datos están presentes.  

2. Si usa System Center Virtual Machine Manager (SCVMM), detenga el servicio SCVMM y realizar copias de seguridad a través de SQL Server.<p>El objetivo es asegurarse de que no hay actualizaciones obtengan realizan a SCVMM durante este tiempo, lo que podría crear una incoherencia entre la copia de seguridad de controladora de red y SCVMM.  

   >[!IMPORTANT]
   >No vuelva a iniciar el servicio SCVMM hasta que se complete la copia de seguridad de la controladora de red.

3. Copia de seguridad de la base de datos de la controladora de red con el `new-networkcontrollerbackup` cmdlet.

4. Comprobar la finalización y el éxito de la copia de seguridad con el `get-networkcontrollerbackup` cmdlet.

5. Si se usa SCVMM, inicie el servicio SCVMM.



### <a name="example-backing-up-the-network-controller-database"></a>Por ejemplo: La copia de seguridad de la base de datos de la controladora de red

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Por ejemplo: Comprobando el estado de una operación de copia de seguridad de la controladora de red

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Restauración desde una copia de seguridad de la infraestructura de SDN

Cuando se restauración todos los componentes necesarios de copia de seguridad, el entorno de SDN se devuelve a un estado operativo.  

>[!IMPORTANT]
>Los pasos varían según el número de componentes que se puede restaurar.


1. Si es necesario, volver a implementar hosts de Hyper-V y el almacenamiento necesario.

2. Si es necesario, restaurar las máquinas virtuales de controlador de red, máquinas virtuales de puerta de enlace RAS y máquinas virtuales Mux de copia de seguridad. 

3. Agente de host del CN de palabras irrelevantes y SLB agente en todos los hosts de Hyper-V de host:

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Detener las máquinas virtuales de puerta de enlace RAS.

5. Detener las máquinas virtuales SLB/Mux.

6. Restaurar la controladora de red con el `new-networkcontrollerrestore` cmdlet.

7. Comprobar la restauración **ProvisioningState** saber si la restauración había finalizado correctamente.

8. Si se usa SCVMM, restaure la base de datos SCVMM mediante la copia de seguridad que se creó al mismo tiempo que la copia de seguridad de la controladora de red.

9. Si desea restaurar las máquinas virtuales de carga de trabajo de copia de seguridad, hágalo ahora.

10. Compruebe el estado del sistema con el cmdlet debug-networkcontrollerconfigurationstate.

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>Por ejemplo: Restaurar una base de datos de la controladora de red
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Por ejemplo: Comprobando el estado de una restauración de base de datos de la controladora de red

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


Para obtener información sobre los mensajes de estado de configuración que pueden aparecer, consulte [solucionar problemas de la Windows Server 2016 definido redes pila de Software](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).