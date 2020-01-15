---
title: Mejoras de seguridad SMB
description: Explicación de la característica de cifrado de SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7b96574dcfc2a4417aa36780d7bd87c2556f61f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950262"
---
# <a name="smb-security-enhancements"></a>Mejoras de seguridad SMB

>Se aplica a: Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016

En este tema se explican las mejoras de seguridad de SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

## <a name="smb-encryption"></a>Cifrado SMB

El cifrado SMB proporciona cifrado de un extremo a otro de datos SMB y protege los datos de las repeticiones de interceptación en redes que no son de confianza. Puede implementar el cifrado SMB con el mínimo esfuerzo, pero puede que requiera costos adicionales reducidos para hardware o software especializado. No tiene ningún requisito para el protocolo de seguridad de Internet (IPsec) o los aceleradores de WAN. El cifrado SMB puede configurarse por recurso compartido o para todo el servidor de archivos, y puede habilitarse para una variedad de escenarios en los que los datos atraviesan redes que no son de confianza.

>[!NOTE]
>El cifrado SMB no cubre la seguridad en reposo, que normalmente se controla mediante Cifrado de unidad BitLocker.

El cifrado SMB debe tenerse en cuenta en cualquier escenario en el que sea necesario proteger la información confidencial de ataques de tipo "Man in the Middle". Los posibles escenarios incluyen:

- Los datos confidenciales del trabajador de la información se mueven mediante el protocolo SMB. El cifrado SMB ofrece un control de la privacidad y la integridad de un extremo a otro entre el servidor de archivos y el cliente, independientemente de las redes recorridas, como las conexiones de red de área extensa (WAN) que se mantienen por proveedores que no son de Microsoft.
- SMB 3,0 permite que los servidores de archivos proporcionen almacenamiento disponible continuamente para las aplicaciones de servidor, como SQL Server o Hyper-V. Habilitar el cifrado SMB ofrece la oportunidad de proteger esa información de ataques de supervisión. El cifrado SMB es más fácil de usar que las soluciones de hardware dedicadas que se requieren para la mayoría de las redes de área de almacenamiento (San).

>[!IMPORTANT]
>Tenga en cuenta que hay un costo de funcionamiento importante en el rendimiento con cualquier protección de cifrado de un extremo a otro en comparación con la no cifrada.

## <a name="enable-smb-encryption"></a>Habilitar el cifrado SMB

Puede habilitar el cifrado SMB para todo el servidor de archivos o solo para recursos compartidos de archivos específicos. Use uno de los procedimientos siguientes para habilitar el cifrado SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitación del cifrado SMB con Windows PowerShell

1. Para habilitar el cifrado SMB para un recurso compartido de archivos individual, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar el cifrado SMB para todo el servidor de archivos, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para crear un nuevo recurso compartido de archivos SMB con el cifrado SMB habilitado, escriba el siguiente script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar el cifrado SMB con Administrador del servidor

1. En Administrador del servidor, Abra **servicios de archivos y almacenamiento**.
2. Seleccione **recursos compartidos** para abrir la página de administración de recursos compartidos.
3. Haga clic con el botón secundario en el recurso compartido en el que desea habilitar el cifrado SMB y, a continuación, seleccione **propiedades**.
4. En la página **configuración** del recurso compartido, seleccione **cifrar acceso a datos**. El acceso a archivos remotos para este recurso compartido está cifrado.

### <a name="considerations-for-deploying-smb-encryption"></a>Consideraciones para implementar el cifrado SMB

De forma predeterminada, cuando se habilita el cifrado SMB para un servidor o recurso compartido de archivos, solo los clientes SMB 3,0 tienen permiso para acceder a los recursos compartidos de archivos especificados. Esto exige la intención del administrador de proteger los datos de todos los clientes que tienen acceso a los recursos compartidos. Sin embargo, en algunas circunstancias, es posible que un administrador desee permitir el acceso sin cifrar a los clientes que no admiten SMB 3,0 (por ejemplo, durante un período de transición cuando se usan versiones de sistema operativo cliente mixtas). Para permitir el acceso sin cifrar a los clientes que no admiten SMB 3,0, escriba el siguiente script en Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La funcionalidad de negociación de dialectos segura que se describe en la sección siguiente evita que un ataque de tipo "Man in the Middle" cambie de una conexión de SMB 3,0 a SMB 2,0 (que usaría acceso sin cifrar). Sin embargo, no impide la degradación de SMB 1,0, lo que también daría lugar a un acceso sin cifrar. Para garantizar que los clientes SMB 3,0 usen siempre el cifrado SMB para tener acceso a los recursos compartidos cifrados, debe deshabilitar el servidor SMB 1,0. (Para obtener instrucciones, consulte la sección [deshabilitación de SMB 1,0](#disabling-smb-10)). Si el valor de **– RejectUnencryptedAccess** se deja en su configuración predeterminada de **$true**, solo los clientes de SMB 3,0 compatibles con el cifrado pueden acceder a los recursos compartidos de archivos (también se rechazarán los clientes de SMB 1,0).

>[!NOTE]
>* El cifrado SMB usa el algoritmo Estándar de cifrado avanzado (AES)-CCM para cifrar y descifrar los datos. AES-CCM también proporciona validación de integridad de datos (firma) para recursos compartidos de archivos cifrados, independientemente de la configuración de firma de SMB. Si desea habilitar la firma SMB sin cifrado, puede continuar. Para obtener más información, consulte [los conceptos básicos de la firma de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Puede encontrar problemas al intentar tener acceso al recurso compartido de archivos o al servidor si su organización usa dispositivos de aceleración de red de área extensa (WAN).
>* Con una configuración predeterminada (donde no se permite el acceso sin cifrar a los recursos compartidos de archivos cifrados), si los clientes que no admiten SMB 3,0 intentan acceder a un recurso compartido de archivos cifrado, el identificador de evento 1003 se registra en el registro de eventos Microsoft-Windows-SmbServer/Operational y el cliente recibirá un mensaje de error de **denegación**
>* El cifrado SMB y el Sistema de cifrado de archivos (EFS) del sistema de archivos NTFS no están relacionados y el cifrado SMB no requiere ni depende del uso de EFS.
>* El cifrado SMB y el Cifrado de unidad BitLocker no están relacionados, y el cifrado SMB no requiere ni depende del uso de Cifrado de unidad BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociación segura de dialectos

SMB 3,0 es capaz de detectar ataques de tipo "Man in the Middle" que intentan degradar el protocolo SMB 2,0 o SMB 3,0 o las capacidades que el cliente y el servidor negocian. Cuando el cliente o el servidor detecta este tipo de ataque, la conexión se desconecta y se registra el ID. de evento 1005 en el registro de eventos Microsoft-Windows-SmbServer/Operational. La negociación de dialectos seguros no puede detectar ni impedir la degradación de SMB 2,0 o 3,0 a SMB 1,0. Por esta razón, y para aprovechar las capacidades completas de cifrado SMB, se recomienda encarecidamente deshabilitar el servidor SMB 1,0. Para obtener más información, vea [deshabilitar SMB 1,0](#disabling-smb-10).

La funcionalidad de negociación de dialectos segura que se describe en la sección siguiente evita que un ataque de tipo "Man in the Middle" cambie de una conexión de SMB 3 a SMB 2 (que usaría acceso sin cifrar). sin embargo, no impide la degradación en SMB 1, lo que también daría lugar a un acceso no cifrado. Para obtener más información sobre los posibles problemas de las implementaciones anteriores de SMB que no son de Windows, consulte [Microsoft Knowledge Base](https://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuevo algoritmo de firma

SMB 3,0 usa un algoritmo de cifrado más reciente para firmar: código de autenticación de mensajes basado en cifrado (CMAC) de Estándar de cifrado avanzado (AES). SMB 2,0 usó el algoritmo de cifrado HMAC-SHA256 anterior. AES-CMAC y AES-CCM pueden acelerar significativamente el cifrado de datos en la mayoría de las CPU modernas que tienen compatibilidad con las instrucciones AES. Para obtener más información, consulte [los conceptos básicos de la firma de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Deshabilitar SMB 1,0

El servicio explorador de equipos heredado y las características del Protocolo de administración remota en SMB 1,0 ahora son independientes y se pueden eliminar. Estas características todavía están habilitadas de forma predeterminada, pero si no tiene clientes SMB anteriores, como equipos que ejecutan Windows Server 2003 o Windows XP, puede quitar las características de SMB 1,0 para aumentar la seguridad y, potencialmente, reducir la revisión.

>[!NOTE]
>SMB 2,0 se presentó en Windows Server 2008 y Windows Vista. Los clientes más antiguos, como los equipos que ejecutan Windows Server 2003 o Windows XP, no admiten SMB 2,0; por lo tanto, no podrán tener acceso a recursos compartidos de archivos ni a recursos compartidos de impresión si el servidor SMB 1,0 está deshabilitado. Además, es posible que algunos clientes SMB que no sean de Microsoft no puedan tener acceso a recursos compartidos de archivos de SMB 2,0 o recursos compartidos de impresión (por ejemplo, impresoras con funcionalidad de "examen a recurso compartido").

Antes de empezar a deshabilitar SMB 1,0, debe averiguar si los clientes SMB están conectados actualmente al servidor que ejecuta SMB 1,0. Para ello, escriba el siguiente cmdlet en Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Este script debe ejecutarse repetidamente en el transcurso de una semana (varias veces al día) para crear una traza de auditoría. También puede ejecutarlo como una tarea programada.

Para deshabilitar SMB 1,0, escriba el siguiente script en Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si se deniega una conexión de cliente SMB porque se ha deshabilitado el servidor que ejecuta SMB 1,0, se registrará el ID. de evento 1001 en el registro de eventos Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Información adicional

Estos son algunos recursos adicionales sobre SMB y tecnologías relacionadas en Windows Server 2012.

- [Bloque de mensajes del servidor](file-server-smb-overview.md)
- [Almacenamiento en Windows Server](../storage.md)
- [Servidor de archivos de escalabilidad horizontal para los datos de la aplicación](../../failover-clustering/sofs-overview.md)