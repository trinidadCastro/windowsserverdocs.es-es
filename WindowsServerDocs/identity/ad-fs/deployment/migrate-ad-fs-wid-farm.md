---
title: Migrar una granja WID del servidor de Federación AD FS 2,0
description: Proporciona información sobre cómo migrar una granja de servidores de AD FS 2,0 Server WID a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: 8ee9d015b56dfec59e1d13a9ffa51f6297d60787
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962759"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar una granja AD FS 2,0 WID
En este documento se proporciona información detallada sobre cómo migrar una granja de servidores de Windows Internal Database (WID) AD FS 2,0 a Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar una granja de AD FS WID
Para migrar una granja WID a Windows Server 2012, realice el procedimiento siguiente:

1.  Para cada nodo (servidor) de la granja WID, revise y lleve a cabo los procedimientos de [preparación para migrar una granja WID](prepare-to-migrate-a-wid-farm.md).

2.  Elimina los nodos no principales del equilibrador de carga.

3.  Actualización del sistema operativo de este servidor de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte el tema sobre la [instalación de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).

> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. En su lugar, se instala el rol de servidor de AD FS de Windows Server 2012, pero no está configurado. Es necesario crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.

4. Crea la configuración de AD FS original en el servidor.

Para crear la configuración de AD FS original se puede usar el **Asistente para configuración de servidor de federación de AD FS** para agregar un servidor de federación a una granja WID. Para obtener más información, consulta [Agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).

> [!NOTE]
> Cuando llegues a la página **Especificar el servidor de federación principal y una cuenta de servicio** del **Asistente para configuración de servidor de federación de AD FS**, escribe el nombre del servidor de federación principal de la granja WID y asegúrate de especificar la información de la cuenta de servicio que registraste al preparar la migración de AD FS. Para obtener más información, vea [preparar la migración del servidor de federación AD FS 2,0](prepare-to-migrate-a-wid-farm.md).
>
> Cuando llegue a la página **especificar el nombre del servicio de Federación** , asegúrese de seleccionar el mismo certificado SSL que registró en la sección "preparación para migrar una granja WID" en [preparación para migrar el servidor de Federación AD FS 2,0](prepare-to-migrate-a-wid-farm.md).

5. Actualiza las páginas web de AD FS del servidor. Si realizó una copia de seguridad de las páginas web de AD FS personalizadas mientras preparaba la migración, debe usar los datos de copia de seguridad para sobrescribir las páginas Web predeterminadas de AD FS que se crearon de forma predeterminada en el directorio **%SystemDrive%\inetpub\adfs\ls** como resultado de la configuración de AD FS en Windows Server 2012.

6. Agregue el servidor que acaba de actualizar a Windows Server 2012 al equilibrador de carga.

7. Repite los pasos del 1 al 6 con el resto de servidores secundarios de la granja WID.

8. Promociona uno de los servidores secundarios actualizados para que sea el servidor principal de la granja WID. Para ello, abra Windows PowerShell y ejecute el siguiente comando: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.

9. Elimina del equilibrador de carga el servidor principal original de la granja WID.

10. Degrada el servidor principal de la granja WID para que sea un servidor secundario mediante Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecute el comando siguiente para degradar el servidor principal original a un servidor secundario: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.

11. Actualización del sistema operativo en este último nodo (servidor) de la granja WID de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte el tema sobre la [instalación de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).

> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. En su lugar, se instala el rol de servidor de AD FS de Windows Server 2012, pero no está configurado. Es necesario crear de forma manual la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.

12. Crea la configuración de AD FS original en el último nodo (servidor) de la granja WID.

Para crear la configuración de AD FS original se puede usar el **Asistente para configuración de servidor de federación de AD FS** para agregar un servidor de federación a una granja WID. Para obtener más información, consulta [Agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).

> [!NOTE]
> Cuando llegues a la página **Especificar el servidor de federación principal y una cuenta de servicio** del **Asistente para configuración de servidor de federación de AD FS**, especifica la información de la cuenta de servicio que registraste al preparar la migración de AD FS. Para obtener más información, vea [preparar la migración del servidor de federación AD FS 2,0](prepare-to-migrate-a-wid-farm.md).
>
> Cuando llegue a la página **especificar el nombre del servicio de Federación** , asegúrese de seleccionar el mismo certificado SSL que registró en [preparación para migrar el servidor de Federación AD FS 2,0](prepare-to-migrate-a-wid-farm.md).

13. Actualiza las páginas web de AD FS del último servidor de la granja WID. Si realizó una copia de seguridad de las páginas web de AD FS personalizadas mientras preparaba la migración, use los datos de copia de seguridad para sobrescribir las páginas Web predeterminadas AD FS que se crearon de forma predeterminada en el directorio **%SystemDrive%\inetpub\adfs\ls** como resultado de la configuración de AD FS en Windows Server 2012.

14. Agregue el último servidor de la granja WID que acaba de actualizar a Windows Server 2012 al equilibrador de carga.

15. Restaure el resto de las personalizaciones de AD FS, como los almacenes de atributos personalizados.

## <a name="next-steps"></a>Pasos a seguir
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparación para migrar el servidor proxy de federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar el servidor de Federación de AD FS 2,0](migrate-the-ad-fs-fed-server.md) [migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) [migrar los agentes Web de AD FS 1,1](migrate-the-ad-fs-web-agent.md)
