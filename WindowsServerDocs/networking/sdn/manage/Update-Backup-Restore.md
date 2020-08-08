---
title: Actualización, copia de seguridad y restauración de la infraestructura de SDN
description: En este tema, aprenderá a actualizar, realizar una copia de seguridad y restaurar una infraestructura de SDN.
manager: grcusanz
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/27/2018
ms.openlocfilehash: 71ea9f33dbebf34614a67a9c305761d66b31411b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993983"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Actualización, copia de seguridad y restauración de la infraestructura de SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a actualizar, realizar una copia de seguridad y restaurar una infraestructura de SDN.

## <a name="upgrade-the-sdn-infrastructure"></a>Actualización de la infraestructura de SDN
La infraestructura de SDN se puede actualizar de Windows Server 2016 a Windows Server 2019. Para la ordenación de la actualización, siga la misma secuencia de pasos que se menciona en la sección "actualización de la infraestructura de SDN". Antes de la actualización, se recomienda realizar una copia de seguridad de la base de datos de la controladora de red.

En el caso de los equipos de la controladora de red, use el cmdlet Get-NetworkControllerNode para comprobar el estado del nodo una vez completada la actualización. Asegúrese de que el nodo vuelve al estado "activo" antes de actualizar los demás nodos. Una vez que haya actualizado todos los nodos de la controladora de red, la controladora de red actualizará los microservicios que se ejecutan en el clúster de la controladora de red en un plazo de una hora. Puede desencadenar una actualización inmediata mediante el cmdlet Update-networkcontroller.

Instale las mismas actualizaciones de Windows en todos los componentes del sistema operativo del sistema de redes definidas por software (SDN), que incluye:

- Hosts de Hyper-V habilitados para SDN
- Máquinas virtuales de controladora de red
- Máquinas virtuales de software Load Balancer MUX
- Máquinas virtuales de puerta de enlace RAS

>[!IMPORTANT]
>Si usa System Center Virtual Manager, debe actualizarlo con los paquetes acumulativos de actualizaciones más recientes.

Al actualizar cada componente, puede usar cualquiera de los métodos estándar para instalar actualizaciones de Windows. Sin embargo, para garantizar un tiempo de inactividad mínimo para las cargas de trabajo y la integridad de la base de datos de la controladora de red, siga estos pasos:

1. Actualice las consolas de administración de.<p>Instale las actualizaciones en cada uno de los equipos en los que usa el módulo de PowerShell de la controladora de red.  Incluye cualquier lugar en el que tenga el rol RSAT-NetworkController instalado por sí mismo. Excluyendo las propias máquinas virtuales de la controladora de red; los actualizará en el paso siguiente.

2. En la primera máquina virtual de controladora de red, instale todas las actualizaciones y reinicie.

3. Antes de continuar con la siguiente máquina virtual de controladora de red, use el `get-networkcontrollernode` cmdlet para comprobar el estado del nodo que ha actualizado y reiniciado.

4. Durante el ciclo de reinicio, espere a que el nodo de la controladora de red se apague y vuelva a aparecer.<p>Después de reiniciar la máquina virtual, pueden transcurrir varios minutos antes de volver al estado **_activo_** . Para obtener un ejemplo de la salida, vea.

5. Instale actualizaciones en cada máquina virtual MUX de SLB de una en una para garantizar la disponibilidad continua de la infraestructura del equilibrador de carga.

6. Actualice los hosts de Hyper-V y las puertas de enlace de RAS, empezando por los hosts que contienen las puertas de enlace RAS que están en modo de **espera** .<p>Las máquinas virtuales de puerta de enlace RAS no se pueden migrar en directo sin perder conexiones de inquilino. Durante el ciclo de actualización, debe tener cuidado de minimizar el número de veces que la conmutación por error de las conexiones de inquilino a una nueva puerta de enlace de RAS. Al coordinar la actualización de hosts y puertas de enlace RAS, cada inquilino conmuta por error una vez, como máximo.

    a. Vacíe el host de las máquinas virtuales que son capaces de realizar la migración en vivo.<p>Las máquinas virtuales de puerta de enlace RAS deben permanecer en el host.

    b. Instale actualizaciones en cada máquina virtual de puerta de enlace en este host.

    c. Si la actualización requiere que se reinicie la máquina virtual de puerta de enlace, reinicie la máquina virtual.

    d. Instale actualizaciones en el host que contenga la máquina virtual de puerta de enlace que se acaba de actualizar.

    e. Reinicie el host si lo requieren las actualizaciones.

    f. Repita el procedimiento para cada host adicional que contenga una puerta de enlace en espera.<p>Si no hay ninguna puerta de enlace en espera, siga estos mismos pasos para todos los hosts restantes.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Ejemplo: uso del cmdlet Get-networkcontrollernode

En este ejemplo, verá que la salida del `get-networkcontrollernode` cmdlet se ejecuta en una de las máquinas virtuales de la controladora de red.

El estado de los nodos que se ven en el resultado del ejemplo es:

- NCNode1.contoso.com = inactivo
- NCNode2.contoso.com = up
- NCNode3.contoso.com = up

>[!IMPORTANT]
>Debe esperar varios minutos hasta que el estado del nodo cambie a _**activo**_ antes de actualizar los nodos adicionales, de uno en uno.

Una vez que haya actualizado todos los nodos de la controladora de red, la controladora de red actualizará los microservicios que se ejecutan en el clúster de la controladora de red en un plazo de una hora.

>[!TIP]
>Puede desencadenar una actualización inmediata mediante el `update-networkcontroller` cmdlet.


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

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Ejemplo: uso del cmdlet Update-networkcontroller
En este ejemplo, verá la salida del `update-networkcontroller` cmdlet para forzar la actualización de la controladora de red.

>[!IMPORTANT]
>Ejecute este cmdlet cuando no tenga más actualizaciones para instalar.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Copia de seguridad de la infraestructura de SDN

Copias de seguridad periódicas de la base de datos de la controladora de red garantiza la continuidad empresarial en caso de desastre o pérdida de datos.  La copia de seguridad de las máquinas virtuales de la controladora de red no es suficiente porque no garantiza que la sesión continúe a través de varios nodos de la controladora de red.

**Requisitos:**
* Un recurso compartido SMB y credenciales con permisos de lectura y escritura en el recurso compartido y el sistema de archivos.
* Opcionalmente, puede usar una cuenta de servicio administrada de grupo (GMSA) si la controladora de red se instaló también con GMSA.

**Pasos**

1. Use el método de copia de seguridad de máquina virtual que prefiera o use Hyper-V para exportar una copia de cada máquina virtual de la controladora de red.<p>La copia de seguridad de la máquina virtual de la controladora de red garantiza que estén presentes los certificados necesarios para descifrar la base de datos.

2. Si usa System Center Virtual Machine Manager (SCVMM), detenga el servicio SCVMM y haga una copia de seguridad de él mediante SQL Server.<p>El objetivo es asegurarse de que no se realicen actualizaciones en SCVMM durante este tiempo, lo que podría crear una incoherencia entre la copia de seguridad de la controladora de red y SCVMM.

   >[!IMPORTANT]
   >No vuelva a iniciar el servicio SCVMM hasta que se complete la copia de seguridad de la controladora de red.

3. Realice una copia de seguridad de la base de datos del controlador de red con el `new-networkcontrollerbackup` cmdlet.

4. Compruebe la finalización y el éxito de la copia de seguridad con el `get-networkcontrollerbackup` cmdlet.

5. Si usa SCVMM, inicie el servicio SCVMM.



### <a name="example-backing-up-the-network-controller-database"></a>Ejemplo: copia de seguridad de la base de datos de la controladora de red

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

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Ejemplo: comprobar el estado de una operación de copia de seguridad de la controladora de red

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

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Restaurar la infraestructura de SDN a partir de una copia de seguridad

Cuando se restauran todos los componentes necesarios de la copia de seguridad, el entorno de SDN vuelve a un estado operativo.

>[!IMPORTANT]
>Los pasos varían en función del número de componentes restaurados.


1. Si es necesario, vuelva a implementar los hosts de Hyper-V y el almacenamiento necesario.

2. Si es necesario, restaure las máquinas virtuales de la controladora de red, las máquinas virtuales de puerta de enlace RAS y las máquinas virtuales de Mux.

3. Detenga el agente de host de NC y el agente de host de SLB en todos los hosts de Hyper-V:

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Detenga las máquinas virtuales de puerta de enlace RAS.

5. Detenga las máquinas virtuales de SLB Mux.

6. Restaure la controladora de red con el `new-networkcontrollerrestore` cmdlet.

7. Compruebe la **ProvisioningState** de restauración para saber cuándo se completó correctamente la restauración.

8. Si usa SCVMM, restaure la base de datos de SCVMM con la copia de seguridad que se creó al mismo tiempo que la copia de seguridad de la controladora de red.

9. Si desea restaurar las máquinas virtuales de carga de trabajo a partir de una copia de seguridad, hágalo ahora.

10. Compruebe el estado del sistema con el cmdlet Debug-networkcontrollerconfigurationstate.

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

### <a name="example-restoring-a-network-controller-database"></a>Ejemplo: restaurar una base de datos de controladora de red

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

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Ejemplo: comprobar el estado de una restauración de la base de datos de la controladora de red

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


Para obtener información sobre los mensajes de estado de configuración que pueden aparecer, consulte [solución de problemas de la pila de redes definidas por software de Windows Server 2016](../troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md).