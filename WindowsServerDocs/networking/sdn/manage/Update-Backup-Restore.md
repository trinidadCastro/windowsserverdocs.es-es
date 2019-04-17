---
title: "Update, copia de seguridad y restauración Software definen la infraestructura de red"
description: "Este tema es parte de la guía definido redes Software acerca de cómo la infraestructura de actualización, copia de seguridad y restaurar SDN en Windows Server 2016."
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>Update, copia de seguridad y restauración Software definen la infraestructura de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema contiene las siguientes secciones.

- [Actualización de la infraestructura SDN](#bkmk_Updating)
- [Copia de seguridad de la infraestructura SDN](#bkmk_backup)
- [Restaurar la infraestructura SDN desde una copia de seguridad](#bkmk_restore)

## <a name="bkmk_Updating"></a>Actualización de la infraestructura SDN

La actualización es el proceso de instalación de actualizaciones de Windows en todos los componentes de sistema operativo del sistema Software definido de redes (SDN).  Esto incluye la SDN habilitado hosts de Hyper-V, máquinas virtuales de controlador de red, máquinas virtuales Mux de equilibrado de carga de Software y las máquinas virtuales de RAS puerta de enlace.  Es fundamental que todos estos componentes tienen el mismo conjunto exacto de las actualizaciones instaladas.  Si se usa System Center Virtual Machine Manager también se recomienda que también actualización con la última acumulativos así.

La actualización de cada componente se realiza con cualquiera de los métodos estándar para instalar actualizaciones de windows, sin embargo, los pasos descritos a continuación se deben seguir para garantizar un mínimo tiempo para cargas de trabajo de inactividad y para garantizar la integridad de la base de datos del controlador de red.

### <a name="step-1-update-the-management-consoles"></a>Paso 1: Actualice las consolas de administración
Instala las actualizaciones necesarias en cada uno de los equipos en los que usas el módulo de Powershell de controlador de red.  Esto incluye cualquier parte que tiene el rol de RSAT NetworkController instalado por sí solo.  Esto no no incluye las máquinas virtuales de controlador de red a sí mismos, como se actualizará en el paso 2.

### <a name="step-2-update-the-network-controllers"></a>Paso 2: Actualizar los controladores de red
Este es el paso más importante en el ciclo de actualización, ya que cada máquina virtual de controlador de red debe actualizarse y estar conectado a Internet completo del clúster de controlador de red antes de continuar al siguiente.

Iniciar con una máquina virtual de controlador de red e instalar todas las actualizaciones necesarias.  Si es necesario, reiniciar la máquina virtual.

Antes de continuar con la siguiente máquina virtual de controlador de red usar get networkcontrollernode para comprobar el estado del nodo que se ha actualizado y reinicia.  Espere a que el nodo del controlador de red ir hacia abajo durante el ciclo de reinicio y, luego, regresa nuevo.  Después de que se haya reiniciado la máquina virtual, todavía puede tardar varios minutos volver al estado de seguridad.

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>Ejemplo: Mediante get-networkcontrollernode para comprobar el estado de los nodos de controlador de red

Este ejemplo muestra el resultado de la ejecución de get-networkcontrollernode desde dentro de una de las máquinas virtuales de controlador de red.  Se muestra que NCNode1.contoso.com está inactivo mientras los otros dos nodos están en buenas condiciones.  Debe esperar hasta varios minutos hasta que el estado para ese nodo se cambia a arriba antes de continuar con la actualización de los nodos adicionales.

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

Después de que todos los nodos de controlador de red están en estado arriba repetir estos pasos para cada nodo del controlador de red adicional.  Sigue actualizando cada nodo de uno en uno.

Una vez que se actualizan todos los nodos de controlador de red, el controlador de red se actualizará el microservicios ejecutando dentro del clúster de controlador de red dentro de una hora.  Puedes activar una actualización inmediata mediante el cmdlet update-networkcontroller.

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>Ejemplo: Buscar actualización networkcontroller para forzar el controlador de red para la actualización

Este comando muestra el resultado de la actualización networkcontroller cuando no haya actualizaciones restante para instalarse.

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>Paso 3: Actualización SLB Muxes

Instalar actualizaciones en cada VM de Mux SLB uno en uno para garantizar la disponibilidad de la infraestructura de equilibrado carga continua.

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>Paso 4: Actualización Hosts de Hyper-V y las puertas de enlace RAS

Como máquinas virtuales de puerta de enlace de RAS puede estar dinámicas migran sin perder las conexiones de inquilinos, debe tenerse cuidado para reducir el número de veces que inquilino conexiones error sobre a un nuevo puertas de enlace RAS durante el ciclo de actualización.  Al coordinar la actualización de los hosts y puertas de enlace RAS cada inquilino se solo conmutación por error al menos una vez.  

Sigue estos pasos para cada host, a partir de los hosts que contienen las puertas de enlace de RAS que están en modo de espera:

1.  Vacíe el host de máquinas virtuales que son capaces de migración en vivo.  Máquinas virtuales de puerta de enlace de RAS debe permanecer en el equipo host.
2.  Instalar actualizaciones en cada VM de puerta de enlace en este host.
3.  Si la actualización requiere la puerta de enlace VM reiniciar, a continuación, reinicia la máquina virtual.  
4.  Instalar actualizaciones en el equipo host que contiene la máquina virtual que se haya actualizado la puerta de enlace.
5.  Si es necesario por las actualizaciones, reinicie el host.
6.  Repite para cada host adicional que contiene una puerta de enlace en modo de espera.  Si no hay puertas de enlace en modo de espera permanecen, a continuación, sigue estos pasos mismo para todos los hosts restantes.

## <a name="bkmk_backup"></a>Copia de seguridad de la infraestructura SDN

Copias de seguridad regulares de la base de datos del controlador de red son importantes para garantizar la continuidad del negocio en caso de pérdida de datos o desastres.  La copia de seguridad de las máquinas virtuales de controlador de red es insuficiente porque no garantiza que quórum se mantiene entre varios nodos de controlador de red.
Requisitos:
 * Un recurso compartido de SMB y las credenciales con permisos de lectura y escritura en el sistema de archivos y recursos compartidos.
 * También puede usar una cuenta de servicio administrado de grupo (GMSA) si se ha instalado el controlador de red con un GMSA así.

Sigue estos pasos para realizar una copia de seguridad:

1. Copia de seguridad de las máquinas virtuales de controlador de red mediante el método de copia de seguridad de máquina virtual de tu elección, o usa Hyper-V para exportar una copia de cada máquina virtual de controlador de red.  Esto garantiza que si se realiza una reconstrucción completa que incluya la restauración de la infraestructura de máquinas virtuales, los certificados necesarios para descifrar la base de datos están presentes.
2. Si estás usando System Center Virtual Machine Manager (SCVMM), detener el servicio SCVMM y copia de seguridad a través de SQL Server para garantizar que no se actualicen a SCVMM durante este tiempo, lo que podría crear una incoherencia entre la copia de seguridad del controlador de red y SCVMM.  No volver a iniciar el servicio SCVMM hasta que se complete la copia de seguridad del controlador de red.
3. La base de datos del controlador de red con el nuevo networkcontrollerbackup una copia de seguridad.

 #### <a name="example-backing-up-the-network-controller-database"></a>Ejemplo: Hacer una copia de la base de datos del controlador de red
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

4. Usar get networkcontrollerbackup para comprobar si se ha realizado correctamente la copia de seguridad y la finalización.

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Ejemplo: Comprobar el estado de una operación de copia de seguridad del controlador de red

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

5.  Si usas SCVMM ahora puede iniciar el servicio SCVMM.

## <a name="bkmk_restore"></a>Restaurar la infraestructura SDN desde una copia de seguridad

Restauración es el proceso de restauración de todos los componentes necesarios de copia de seguridad para devolver un entorno SDN a un estado de funcionamiento.  Los pasos varía ligeramente según la cantidad de componentes que se están restaurando.

1. Si es necesario, vuelve a implementar los hosts de Hyper-V y el almacenamiento necesarios.

2. Si es necesario, restaurar las máquinas virtuales de controlador de red, máquinas virtuales de puerta de enlace de RAS y máquinas virtuales Mux de copia de seguridad. 

3. Detener el agente de Host CN y SLB Host en todos los hosts de Hyper-V

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Dejar de máquinas virtuales de puerta de enlace RAS

5. Detener SLB Mux virtuales

6. Restaure el controlador de red mediante el cmdlet de nuevo networkcontrollerrestore.

 #### <a name="example-restoring-a-network-controller-database"></a>Ejemplo: Restauración de una base de datos del controlador de red
 
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

7. Comprueba la restauración ProvisioningState saber cuándo la restauración hubiera completado correctamente.

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Ejemplo: Comprobar el estado de una restauración de base de datos del controlador de red

    ```
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

8. Si usas SCVMM, restaurar la base de datos con la copia de seguridad que se creó al mismo tiempo que la copia de seguridad del controlador de red.

9. Si se están restaurando máquinas virtuales de carga de trabajo de copia de seguridad, puedes hacerlo ahora.

10. Usa el cmdlet de depuración networkcontrollerconfigurationstate para comprobar el estado del sistema.

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

Para obtener información sobre los mensajes de estado de configuración que pueden aparecer, consulta [solucionar problemas de la Windows Server 2016 definido redes pila de Software](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).