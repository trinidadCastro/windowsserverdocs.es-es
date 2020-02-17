---
title: Mejoras de seguridad SMB
description: Explicación de la característica de cifrado SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7b96574dcfc2a4417aa36780d7bd87c2556f61f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950262"
---
# <a name="smb-security-enhancements"></a>Mejoras de seguridad SMB

>Se aplica a: Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016

En este tema se explican las mejoras de seguridad SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

## <a name="smb-encryption"></a>Cifrado SMB

El cifrado SMB proporciona cifrados de datos SMB de un extremo a otro y protege los datos de las ocurrencias de interceptación en redes que no son de confianza. Puedes implementar el cifrado SMB con un mínimo de esfuerzo, pero podrían requerirse costos adicionales reducidos para el hardware o software especializados. No tiene ningún requisito para el protocolo de seguridad de Internet (IPsec) o los aceleradores WAN. El cifrado SMB puede configurarse por recurso compartido o para todo el servidor de archivos y puede habilitarse para una variedad de escenarios donde los datos se envían a través de redes que no son de confianza.

>[!NOTE]
>El cifrado SMB no cubre la seguridad en reposo, que normalmente se controla mediante el Cifrado de unidad BitLocker.

El cifrado SMB debería ser una opción en cualquier escenario en el que se deba proteger la información confidencial de ataques de tipo "Man in the Middle". Los posibles escenarios incluyen:

- Los datos confidenciales de un trabajador de la información se mueven mediante el protocolo SMB. El cifrado SMB ofrece un control de la privacidad y la integridad de un extremo a otro entre el servidor de archivos y el cliente, independientemente de las redes recorridas, como las conexiones de red de área extensa (WAN) que mantienen proveedores que no son Microsoft.
- SMB 3.0 permite que los servidores de archivos proporcionen almacenamiento disponible continuamente para las aplicaciones de servidor, como SQL Server o Hyper-V. Al habilitar el cifrado SMB, tienes la oportunidad de proteger esa información de ataques de "snooping". El cifrado SMB es más fácil de usar que las soluciones de hardware dedicadas que se requieren para la mayoría de las redes de área de almacenamiento (SAN).

>[!IMPORTANT]
>Ten en cuenta que cualquier protección de cifrado de un extremo a otro tiene un costo operativo de rendimiento importante en comparación con la protección sin cifrado.

## <a name="enable-smb-encryption"></a>Habilitación del cifrado SMB

Puedes habilitar el cifrado SMB para todo el servidor de archivos o solo para recursos compartidos de archivos específicos. Usa uno de los siguientes procedimientos para habilitar el cifrado SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitación del cifrado SMB con Windows PowerShell

1. Para habilitar el cifrado SMB para un recurso compartido de archivos individual, escribe el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar el cifrado SMB para todo el servidor de archivos, escribe el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para crear un nuevo recurso compartido de archivos SMB con el cifrado SMB habilitado, escribe el siguiente script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitación del cifrado SMB con el Administrador del servidor

1. En el Administrador del servidor, abre **Servicios de archivos y almacenamiento**.
2. Selecciona **Recursos compartidos** para abrir la página de administración de recursos compartidos.
3. Haz clic con el botón secundario en el recurso compartido en el que quieres habilitar el cifrado SMB y, a continuación, selecciona **Propiedades**.
4. En la página **Configuración** del recurso compartido, selecciona **Cifrar acceso de datos**. El acceso a archivos remotos para este recurso compartido está cifrado.

### <a name="considerations-for-deploying-smb-encryption"></a>Consideraciones a la hora de implementar el cifrado SMB

De forma predeterminada, cuando se habilita el cifrado SMB para un servidor o recurso compartido de archivos, solo los clientes SMB 3.0 tienen permitido acceder a los recursos compartidos de los archivos especificados. De este modo, se refuerza la intención del administrador de proteger los datos de todos los clientes que tienen acceso a los recursos compartidos. Sin embargo, en algunas circunstancias, es posible que un administrador quiera permitir el acceso sin cifrar a los clientes que no admiten SMB 3.0 (por ejemplo, durante un período de transición cuando se usan varias versiones del sistema operativo cliente). Para permitir el acceso sin cifrar a los clientes que no admiten SMB 3.0, escribe el siguiente script en Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La funcionalidad de negociación segura de dialectos que se describe en la sección siguiente evita que un ataque de tipo "Man in the Middle" cambie una conexión de SMB 3.0 a SMB 2.0 (que usaría acceso sin cifrar). Sin embargo, no impide el cambio a SMB 1.0, lo que también daría lugar a un acceso sin cifrar. Para garantizar que los clientes SMB 3.0 usen siempre el cifrado SMB para acceder a los recursos compartidos cifrados, debes deshabilitar el servidor SMB 1.0. (Para obtener instrucciones, consulta la sección [Deshabilitación de SMB 1.0](#disabling-smb-10)). Si la opción de configuración **–RejectUnencryptedAccess** se deja en su valor predeterminado **$true**, solo los clientes SMB 3.0 compatibles con cifrado podrán acceder a los recursos compartidos de archivos (también se rechazará a los clientes SMB 1.0).

>[!NOTE]
>* El cifrado SMB usa el algoritmo Estándar de cifrado avanzado (AES)-CCM para cifrar y descifrar los datos. AES-CCM también proporciona validación de integridad de datos (firma) para los recursos compartidos de archivos cifrados, independientemente de la configuración de firma de SMB. Si quieres habilitar la firma SMB sin cifrado, puedes continuar. Para obtener más información, consulta [los aspectos básicos de la firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Puedes tener problemas al tratar de acceder al recurso compartido de archivos o al servidor si tu organización usa dispositivos de aceleración de red de área extensa (WAN).
>* Con una configuración predeterminada (donde no se permite el acceso sin cifrar a los recursos compartidos de archivos cifrados), si los clientes que no admiten SMB 3.0 intentan acceder a un recurso compartido de archivos cifrado, el identificador de evento 1003 se registra en el registro de eventos Microsoft-Windows-SmbServer/Operational y el cliente recibe un mensaje de error **Acceso denegado**.
>* El cifrado SMB y el Sistema de cifrado de archivos (EFS) del sistema de archivos NTFS no están relacionados, y el cifrado SMB no requiere ni depende del uso de EFS.
>* El cifrado SMB y el Cifrado de unidad BitLocker no están relacionados, y el cifrado SMB no requiere ni depende del uso del Cifrado de unidad BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociación segura de dialectos

SMB 3.0 es capaz de detectar ataques de tipo "Man in the Middle" que intentan cambiar el protocolo SMB 2.0 o SMB 3.0 o las funcionalidades que el cliente y el servidor negocian. Cuando el cliente o el servidor detectan este tipo de ataque, la conexión se desconecta y se registra el identificador de evento 1005 en el registro de eventos Microsoft-Windows-SmbServer/Operational. La negociación segura de dialectos no puede detectar ni impedir el cambio de SMB 2.0 o 3.0 a SMB 1.0. Por esta razón, y para aprovechar todas las funcionalidades del cifrado SMB, se recomienda deshabilitar el servidor SMB 1.0. Para obtener más información, consulta [Deshabilitación de SMB 1.0](#disabling-smb-10).

La funcionalidad de negociación segura de dialectos que se describe en la sección siguiente evita que un ataque de tipo "Man in the Middle" cambie una conexión de SMB 3 a SMB 2 (que usaría acceso sin cifrar); sin embargo, no evita los cambios a SMB 1, que también resulta en un acceso sin cifrar. Para obtener más información sobre los posibles problemas con las implementaciones anteriores de SMB que no son de Windows, consulta [Microsoft Knowledge Base](https://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuevo algoritmo de firma

SMB 3.0 usa un algoritmo de cifrado más reciente para la firma: el código de autenticación de mensajes basado en cifrado con Estándar de cifrado avanzado (AES). SMB 2.0 usaba el algoritmo de cifrado HMAC-SHA256 anterior. AES-CMAC y AES-CCM pueden acelerar significativamente el cifrado de datos en la mayoría de las CPU modernas que son compatibles con las instrucciones AES. Para obtener más información, consulta [los aspectos básicos de la firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Deshabilitación de SMB 1.0

Las características como el servicio de explorador de equipos heredado y el protocolo de administración remota (RAP) de SMB 1.0 ahora son independientes y se pueden eliminar. Estas características todavía están habilitadas de manera predeterminada, pero si no tienes ningún cliente SMB antiguo, como equipos que ejecutan Windows Server 2003 o Windows XP, puedes eliminar las características de SMB 1.0 para aumentar la seguridad y reducir la posible aplicación de revisiones.

>[!NOTE]
>SMB 2.0 se introdujo en Windows Server 2008 y Windows Vista. Los clientes más antiguos, como los equipos que ejecutan Windows Server 2003 o Windows XP, no admiten SMB 2.0; por lo tanto, no podrán acceder a recursos compartidos de archivos ni a recursos compartidos de impresión si el servidor SMB 1.0 está deshabilitado. Además, es posible que algunos clientes SMB que no son de Microsoft no puedan acceder a recursos compartidos de archivos de SMB 2.0 o a recursos compartidos de impresión (por ejemplo, las impresoras con la funcionalidad de digitalización a recurso compartido).

Antes de empezar a deshabilitar SMB 1.0, debes averiguar si los clientes SMB están conectados actualmente al servidor que ejecuta SMB 1.0. Para ello, introduce el siguiente cmdlet en Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Este script debe ejecutarse varias veces en el transcurso de una semana (varias veces al día) para crear una pista de auditoría. También puedes ejecutarlo como una tarea programada.

Para deshabilitar SMB 1.0, escribe el siguiente script en Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si se rechaza la conexión de cliente SMB porque se deshabilitó el servidor que ejecutaba SMB 1.0, se registrará el identificador de evento 1001 en el registro de eventos Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Información adicional

A continuación se mencionan recursos adicionales acerca de SMB y las tecnologías relacionadas en Windows Server 2012.

- [Bloque de mensajes del servidor](file-server-smb-overview.md)
- [Almacenamiento en Windows Server](../storage.md)
- [Servidor de archivos de escalabilidad horizontal para datos de aplicación](../../failover-clustering/sofs-overview.md)