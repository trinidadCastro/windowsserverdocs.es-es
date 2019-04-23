---
title: Migración de clúster de dominio en Windows Server 2016/2019 entre
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Este artículo describe cómo mover un clúster de Windows Server 2019 de un dominio a otro
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875946"
---
# <a name="failover-cluster-domain-migration"></a>Migración de clústeres de conmutación por error de dominio

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema proporciona que información general para la conmutación por error de Windows Server mover clústeres de un dominio a otro.

## <a name="why-migrate-between-domains"></a>¿Por qué migrar entre dominios

Hay varios escenarios donde es necesario migrar un clúster de uno doamin a otro.

- Compañíaa se combina con EmpresaB y debe mover todos los clústeres en el dominio compañíaa
- Los clústeres son creados en el centro de datos principal y se envían a ubicaciones remotas
- Clúster se creó como un clúster de grupo de trabajo y ahora debe formar parte de un dominio
- Clúster se creó como un clúster de dominio y ahora debe formar parte de un grupo de trabajo
- Clúster se está moviendo a un área de la empresa a otro y es un subdominio distinto

Microsoft no ofrece soporte técnico a los administradores que intentan mover los recursos de un dominio a otro si no se admite la operación de aplicación subyacente. Por ejemplo, Microsoft no proporciona soporte técnico a los administradores que intentan mover un servidor Microsoft Exchange de un dominio a otro.

   > [!WARNING]
   > Se recomienda realizar una copia de seguridad completa de todo el almacenamiento compartido del clúster antes de mover el clúster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 y versiones anterior

En Windows Server 2016 y versiones anteriores, el servicio de clúster no tenía la capacidad de traslado de un dominio a otro.  Esto fue debido a la mayor dependencia de Active Directory Domain Services y los nombres virtuales creados.   

## <a name="options"></a>Opciones

Para llevar a cabo este movimiento, hay dos opciones.

La primera opción implica destruir el clúster y volver a crearlo en el nuevo dominio.

![Destruir y volver a generar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Como se muestra en la animación, esta opción es destructiva con los pasos que se va a:

1. Destruir el clúster.
2. Cambiar la pertenencia al dominio de los nodos en el nuevo dominio.
3. Vuelva a crear el clúster como nuevo en el dominio actualizado.  Esto podría implicar tener que volver a crear todos los recursos.

La segunda opción es menos destructiva, pero requiere hardware adicional, ya que sería necesario un nuevo clúster que se crea en el nuevo dominio.  Una vez que el clúster está en el nuevo dominio, ejecute el Asistente para migración de clúster para migrar los recursos. Tenga en cuenta que esto no migra datos, deberá usar otra herramienta para migrar datos, como [Storage Migration Service](../storage/storage-migration-service/overview.md)(una vez que se ha agregado compatibilidad con clústeres).

![Crear y migrar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Como se muestra en la animación, esta opción no es destructiva, pero requiere un hardware diferente o un nodo del clúster existente que se ha quitado.

1. Crear un nuevo clusterin el nuevo dominio mientras sigue teniendo el clúster antiguo disponible.
2. Use la [Asistente para migración de clúster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) para migrar todos los recursos al nuevo clúster. Recordatorio, esto no copia los datos, por lo que deberán realizarse por separado.
3. Retirar o destruir el clúster anterior.

En ambos casos, el nuevo clúster necesitaría tener todos [aplicaciones compatibles con clústeres](https://technet.microsoft.com/aa369082(v=vs.90)) instalado, actualizado para todos los controladores y, posiblemente, las pruebas para garantizar que todos se ejecutarán correctamente.  Este es un proceso mucho tiempo si los datos también es necesario mover.

## <a name="windows-server-2019"></a>Windows Server 2019

En Windows Server 2019, presentamos las funcionalidades de migración de clúster entre dominios.  Ahora, puede hacer fácilmente los escenarios mencionados anteriormente y ya no se necesita la necesidad de volver a crear.  

Mover un clúster de un dominio es un proceso sencillo. Para lograr esto, hay dos nuevos cmdlets de PowerShell.

**Nuevo ClusterNameAccount** : crea una cuenta de nombre de clúster en Active Directory **Remove-ClusterNameAccount** – quita las cuentas de nombre de clúster de Active Directory

El proceso para lograr esto consiste en cambiar el clúster de un dominio a un grupo de trabajo y volver al nuevo dominio.  La necesidad de destruir un clúster, volver a generar un clúster, instalar aplicaciones, etcetera no es un requisito. Por ejemplo, sería similar al siguiente:

![Migrar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migración de un clúster a un nuevo dominio

En los pasos siguientes, que se va a se mueve un clúster desde el dominio Contoso.com para el nuevo dominio Fabrikam.com.  Es el nombre del clúster *CLUSCLUS* y con un rol de servidor de archivos llamado *FS CLUSCLUS*.

1. Cree una cuenta de administrador local con el mismo nombre y contraseña en todos los servidores del clúster.  Esto puede ser necesario para iniciar sesión mientras los servidores se mueve entre dominios.
2. Inicie sesión en el primer servidor con una cuenta de usuario o administrador de dominio que tenga permisos de Active Directory al clúster nombre de objeto (CNO), objetos de equipo Virtual (VCO), tiene acceso al clúster y abra PowerShell.
3. Asegúrese de todos los recursos de nombre de red del clúster están en sin conexión en un estado y ejecute el comando siguiente.  Este comando quitará los objetos de Active Directory que puede tener el clúster.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Utilice equipos y usuarios de Active Directory para asegurarse de que el equipo CNO y VCO objetos asociados con todos los nombres de clúster se han quitado.

   > [!NOTE]
   > Es una buena idea para detener el servicio de clúster en todos los servidores del clúster y establecer el tipo de inicio del servicio a Manual para que el servicio de clúster no se inicia cuando se reinician los servidores al cambiar dominios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Cambiar la pertenencia al dominio de los servidores a un grupo de trabajo, reiniciar los servidores, unir los servidores al dominio nuevo y vuelva a reiniciar.
6. Una vez que los servidores están en el nuevo dominio, inicie sesión en un servidor con una cuenta de usuario o administrador de dominio que tiene permisos de Active Directory para crear objetos, tiene acceso al clúster y abra PowerShell. Inicie el servicio de clúster y volver a establecer en automático.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Devolver el nombre del clúster y todos los clústeres de otros recursos de nombre de red a un estado en línea.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Cambiar el clúster para formar parte del nuevo dominio con objetos de Active Directory asociado. Para ello, el comando está por debajo y los recursos de nombre de red deben estar en un estado en línea.  Este comando lo que sucederá es volver a crear los objetos de nombre en Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Nota: Si no tiene los grupos adicionales con nombres de red (es decir, un clúster de Hyper-V con solo las máquinas virtuales), no es necesario el modificador de parámetro - UpgradeVCOs.

9. Usar equipos y usuarios de Active Directory para comprobar el nuevo dominio y asegúrese de que se crearon los objetos de equipo asociada. Si tienen, a continuación, ponga los recursos restantes en los grupos en línea.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemas conocidos

Si usa la nueva característica de testigo USB, podrá agregar el clúster al nuevo dominio.  El razonamiento es que el tipo de testigo del recurso compartido de archivos debe utilizar Kerberos para la autenticación.  Cambiar al testigo a ninguno antes de agregar el clúster al dominio.  Una vez completada, vuelva a crear al testigo USB.  El error que verá es:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

