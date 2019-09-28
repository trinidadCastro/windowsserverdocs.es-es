---
title: Migración de clústeres entre dominios en Windows Server 2016/2019
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: En este artículo se describe cómo mover un clúster de Windows Server 2019 de un dominio a otro
ms.localizationpriority: medium
ms.openlocfilehash: 68f49795124dedf0655726853a4d865686f6d697
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361411"
---
# <a name="failover-cluster-domain-migration"></a>Migración de dominios de clúster de conmutación por error

> Se aplica a: Windows Server 2019 y Windows Server 2016

En este tema se proporciona información general para mover los clústeres de conmutación por error de Windows Server de un dominio a otro.

## <a name="why-migrate-between-domains"></a>Por qué migrar entre dominios

Hay varios escenarios en los que es necesario migrar un clúster de una Doamin a otra.

- CompanyA combina con CompanyB y debe trasladar todos los clústeres a un dominio de Empresaa
- Los clústeres se compilan en el centro de información principal y se envían a ubicaciones remotas.
- El clúster se compiló como un clúster de grupo de trabajo y ahora debe formar parte de un dominio.
- El clúster se compiló como un clúster de dominio y ahora debe formar parte de un grupo de trabajo.
- El clúster se está moviendo a un área de la empresa a otra y es un subdominio diferente.

Microsoft no proporciona soporte técnico a los administradores que intenten trasladar recursos de un dominio a otro si no se admite la operación de aplicación subyacente. Por ejemplo, Microsoft no proporciona soporte técnico a los administradores que intentan trasladar un servidor de Microsoft Exchange de un dominio a otro.

   > [!WARNING]
   > Se recomienda realizar una copia de seguridad completa de todo el almacenamiento compartido en el clúster antes de migrar el clúster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 y versiones anteriores

En Windows Server 2016 y versiones anteriores, los Servicio de clúster no tenían la capacidad de pasar de un dominio a otro.  Esto se debe a la mayor dependencia de Active Directory Domain Services y los nombres virtuales creados.   

## <a name="options"></a>Opciones

Para realizar este tipo de movimiento, hay dos opciones.

La primera opción implica destruir el clúster y volver a generarlo en el nuevo dominio.

![Destruir y volver a generar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Como se muestra en la animación, esta opción es destructiva con los pasos:

1. Destruya el clúster.
2. Cambie la pertenencia al dominio de los nodos al nuevo dominio.
3. Vuelva a crear el clúster como nuevo en el dominio actualizado.  Esto implicaría tener que volver a crear todos los recursos.

La segunda opción es menos destructiva, pero requiere hardware adicional, ya que se debe generar un nuevo clúster en el nuevo dominio.  Una vez que el clúster esté en el nuevo dominio, ejecute el Asistente para migración de clústeres para migrar los recursos. Tenga en cuenta que esto no migra datos; deberá usar otra herramienta para migrar datos, como el servicio de [migración de almacenamiento](../storage/storage-migration-service/overview.md)(una vez agregada la compatibilidad con el clúster).

![Compilar y migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Como se muestra en la animación, esta opción no es destructiva, pero requiere un hardware diferente o un nodo del clúster existente del que se ha quitado.

1. Cree un nuevo clúster en el nuevo dominio sin dejar disponible el clúster anterior.
2. Use el [Asistente para migración de clústeres](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) para migrar todos los recursos al nuevo clúster. Recuerde que no copia los datos, por lo que deberá realizarse por separado.
3. Retirar o destruir el clúster anterior.

En ambas opciones, el nuevo clúster necesitaría tener todas [las aplicaciones habilitadas para clústeres](https://technet.microsoft.com/aa369082(v=vs.90)) instaladas, controladores actualizadas y, posiblemente, pruebas para asegurarse de que todas se ejecutarán correctamente.  Se trata de un proceso lento si también es necesario moverse los datos.

## <a name="windows-server-2019"></a>Windows Server 2019

En Windows Server 2019, se introdujeron las capacidades de migración de dominios entre clústeres.  Por lo tanto, ahora se pueden realizar fácilmente los escenarios indicados anteriormente y ya no es necesario volver a generar.  

Mover un clúster de un dominio es un proceso directo. Para ello, hay dos nuevos commandlets de PowerShell.

**New-ClusterNameAccount** : crea una cuenta de nombre de clúster en Active Directory **Remove-ClusterNameAccount** – quita las cuentas de nombre de clúster de Active Directory

El proceso para lograrlo es cambiar el clúster de un dominio a un grupo de trabajo y volver al nuevo dominio.  La necesidad de destruir un clúster, recompilar un clúster, instalar aplicaciones, etc., no es un requisito. Por ejemplo, tendría el siguiente aspecto:

![Migrar](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migración de un clúster a un nuevo dominio

En los pasos siguientes, se mueve un clúster del dominio Contoso.com al nuevo dominio Fabrikam.com.  El nombre del clúster es *CLUSCLUS* y con un rol de servidor de archivos denominado *FS-CLUSCLUS*.

1. Cree una cuenta de administrador local con el mismo nombre y la misma contraseña en todos los servidores del clúster.  Esto puede ser necesario para iniciar sesión mientras los servidores se mueven entre dominios.
2. Inicie sesión en el primer servidor con una cuenta de administrador o de usuario de dominio que tenga Active Directory permisos para el objeto de nombre de clúster (CNO), los objetos de equipo virtual (VCO), tiene acceso al clúster y abra PowerShell.
3. Asegúrese de que todos los recursos de nombre de red del clúster están en estado sin conexión y ejecute el siguiente comando.  Este comando quitará el Active Directory los objetos que puede tener el clúster.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Use Active Directory usuarios y equipos para asegurarse de que se han quitado los objetos de equipo CNO y VCO asociados a todos los nombres en clúster.

   > [!NOTE]
   > Es una buena idea detener el Servicio de clúster en todos los servidores del clúster y establecer el tipo de inicio del servicio en manual para que el Servicio de clúster no se inicie cuando se reinicien los servidores mientras se cambian los dominios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Cambie la pertenencia al dominio de los servidores a un grupo de trabajo, reinicie los servidores, una los servidores al nuevo dominio y vuelva a iniciar.
6. Una vez que los servidores estén en el nuevo dominio, inicie sesión en un servidor con una cuenta de administrador o de usuario de dominio que tenga Active Directory permisos para crear objetos, tenga acceso al clúster y abra PowerShell. Inicie el servicio de clúster y vuelva a establecerlo en automático.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Ponga el nombre del clúster y todos los demás recursos de nombre de red del clúster a un estado en línea.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Cambiar el clúster para que forme parte del nuevo dominio con objetos asociados de Active Directory. Para ello, el comando se muestra a continuación y los recursos de nombre de red deben estar en estado en línea.  Lo que hará este comando es volver a crear los objetos de nombre en Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Nota: Si no tiene ningún grupo adicional con nombres de red (es decir, un clúster de Hyper-V con solo máquinas virtuales), no es necesario el modificador de parámetro-UpgradeVCOs.

9. Use Active Directory usuarios y equipos para comprobar el nuevo dominio y asegurarse de que se crearon los objetos de equipo asociados. Si tienen, ponga en línea los recursos restantes de los grupos.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problemas conocidos

Si usa la nueva característica de testigo USB, no podrá agregar el clúster al nuevo dominio.  La razón es que el tipo de testigo del recurso compartido de archivos debe emplear Kerberos para la autenticación.  Cambie el testigo a ninguno antes de agregar el clúster al dominio.  Una vez finalizado, vuelva a crear el testigo USB.  El error que verá es el siguiente:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

