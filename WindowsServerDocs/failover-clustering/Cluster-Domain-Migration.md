---
title: Cruzar la migración de clústeres de dominio de Windows Server 2016/2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Este artículo describe el traslado de un clúster de Windows Server 2019 de un dominio a otro
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042039"
---
# Migración de dominio de clúster de conmutación por error

> Se aplica a: Windows Server 2019, Windows Server 2016

Este tema proporciona que información general sobre el movimiento de conmutación por error de Windows Server de clústeres de un dominio a otro.

## ¿Por qué migrar entre dominios

Hay varios escenarios donde es necesario migrar un clúster de uno doamin a otro.

- Compañíaa se combina con EmpresaB y debe mover todos los clústeres en compañíaa dominio
- Clústeres se crean en el centro de datos principal y se envían a ubicaciones remotas
- Clúster se creó como un clúster de grupo de trabajo y ahora debe formar parte de un dominio
- Clúster se creó como un clúster de dominio y ahora debe formar parte de un grupo de trabajo
- Clúster se mueve a un área de la compañía a otro y es un subdominio diferente

Microsoft no proporciona soporte técnico a los administradores que intenten mover los recursos de un dominio a otro si la operación subyacente de la aplicación no es compatible. Por ejemplo, Microsoft no proporciona soporte técnico a los administradores que intenten mover un servidor Microsoft Exchange desde un dominio a otro.

   > [!WARNING]
   > Te recomendamos que realice una copia de seguridad completa de almacenamiento compartido de todos los en el clúster antes de pasar el clúster.

## Windows Server 2016 y versiones anterior

En Windows Server 2016 y versiones anteriores, el servicio de clúster no tiene la capacidad de pasar de un dominio a otro.  Esto era debido a la dependencia de servicios de dominio de Active Directory y los nombres virtuales que se creó la mayor.   

## Opciones

Para realizar un movimiento de este tipo, hay dos opciones.

La primera opción implica la destrucción del clúster y volver a crearlo en el nuevo dominio.

![Destruir y volver a compilar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Como se muestra en la animación, esta opción es destructiva con los pasos que se va a:

1. Destruir el clúster.
2. Cambiar la pertenencia a un dominio de los nodos en el nuevo dominio.
3. Vuelve a crear el clúster como nuevo en el dominio actualizado.  Esto podría implicar tener que volver a crear todos los recursos.

La segunda opción es menos destructiva, pero requiere hardware adicional, ya que sería necesario un nuevo clúster deben integrarse en el nuevo dominio.  Una vez que el clúster está en el nuevo dominio, ejecuta el Asistente para la migración de clúster para migrar los recursos. Ten en cuenta que esto no migra datos, tendrás que usar otra herramienta para migrar los datos, como el [Servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md)(una vez que se agrega compatibilidad con clústeres).

![Crear y migrar](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Como se muestra en la animación, esta opción no es destructiva, pero requiere hardware diferente o un nodo del clúster existente que se ha quitado.

1. Crear un nuevo clusterin el nuevo dominio mientras sigues teniendo el clúster antiguo disponible.
2. Usa el [Asistente para la migración de clúster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) a migrar todos los recursos para el nuevo clúster. Recordatorio, esto no copiar los datos, por lo que deberán hacerse por separado.
3. Retirar o destruir el clúster antiguo.

En ambos casos, el nuevo clúster sería necesario tener todas las [aplicaciones con reconocimiento de clúster](https://technet.microsoft.com/aa369082(v=vs.90)) instaladas, actualizado para todos los controladores y posiblemente pruebas para garantizar que todos se ejecutarán correctamente.  Este es un proceso mucho tiempo si los datos también deben moverse.

## Windows Server 2019

En Windows Server 2019, presentamos las capacidades de migración de clúster entre dominios.  Por lo tanto, ahora, pueden realizar fácilmente los escenarios mencionados anteriormente y ya no es necesaria la necesidad de volver a generar.  

Mover un clúster de un dominio es un proceso sencillo. Para ello, hay dos los cmdlets de PowerShell de nuevo.

**New-ClusterNameAccount** : crea una cuenta de nombre de clúster en Active Directory **Remove-ClusterNameAccount** : quita las cuentas de nombre de clúster de Active Directory

El proceso para lograr esto es cambiar el clúster de un dominio a un grupo de trabajo y volver al nuevo dominio.  La necesidad de destruir un clúster, volver a compilar un clúster, instalar aplicaciones, etcetera no es un requisito. Por ejemplo, podría tener el siguiente aspecto:

![Migración](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## Migración de un clúster a un nuevo dominio

En los pasos siguientes, un clúster se mueve desde el dominio Contoso.com para el nuevo dominio Fabrikam.com.  El nombre del clúster es *CLUSCLUS* y con un rol de servidor de archivos denominado *FS CLUSCLUS*.

1. Crear una cuenta de administrador local con el mismo nombre y contraseña en todos los servidores del clúster.  Esto puede ser necesario para iniciar sesión, mientras que los servidores se mueven entre dominios.
2. Inicia sesión en el primer servidor con una cuenta de usuario o el Administrador de dominio que tiene permisos de Active Directory para el clúster nombre de objeto (CNO), objetos de equipo Virtual (VCO), tiene acceso al clúster y PowerShell abierta.
3. Asegúrese de todos los recursos de nombre de red de clúster en sin conexión en un estado y se ejecuta el siguiente comando.  Este comando quitará los objetos de Active Directory que puede tener el clúster.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Usar equipos y usuarios de Active Directory para asegurarse de que el equipo CNO y VCO objetos asociados a todos los nombres en clúster se han quitado.

   > [!NOTE]
   > Es una buena idea dejar el servicio de clúster en todos los servidores del clúster y establece el tipo de inicio del servicio manual para que el servicio de clúster no se inicia cuando se reinicie los servidores al cambiar los dominios.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Cambiar la pertenencia al dominio de los servidores a un grupo de trabajo, reinicie los servidores, unirse a los servidores con el nuevo dominio y otra vez.
6. Una vez que los servidores están en el nuevo dominio, inicia sesión en un servidor con una cuenta de usuario o el Administrador de dominio que tiene permisos de Active Directory para crear objetos, tiene acceso al clúster y abre PowerShell. Inicia el servicio de clúster y vuelve a automático establecer.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Llevar el nombre del clúster y todos los demás clúster los recursos de nombre de red a un estado en línea.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Cambiar el clúster para formar parte del nuevo dominio con objetos de active directory asociado. Para ello, el comando es inferior y los recursos de nombre de red deben estar en un estado en línea.  Este comando ¿Qué es volver a crear los objetos con nombre en Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Nota: Si no tienes ningún grupo adicional con nombres de red (es decir, un clúster de Hyper-V con solo las máquinas virtuales), no es necesario el modificador de parámetro - UpgradeVCOs.

9. Usa los equipos y usuarios de Active Directory para comprobar el nuevo dominio y asegúrate de que se crearon los objetos de equipo asociado. Si lo ha hecho, a continuación, ponga el resto de los recursos de los grupos en línea.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## Problemas conocidos

Si estás usando la nueva característica de testigo USB, podrá agregar el clúster para el nuevo dominio.  La lógica es que el tipo de testigo de recurso compartido de archivo debe utilizar Kerberos para la autenticación.  Cambiar al testigo en ninguno antes de agregar el clúster al dominio.  Una vez que se complete, vuelve a crear al testigo USB.  El error que se verá es:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

