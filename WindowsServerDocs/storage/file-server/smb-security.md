---
title: Mejoras de seguridad SMB
description: Explicación de la característica de cifrado de SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838056"
---
# <a name="smb-security-enhancements"></a>Mejoras de seguridad SMB

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tema explica las mejoras de seguridad SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

## <a name="smb-encryption"></a>Cifrado SMB

El cifrado SMB proporciona cifrado de extremo a otro de los datos SMB y protege los datos de las repeticiones de interceptación de redes no confiables. Puede implementar el cifrado SMB con el mínimo esfuerzo, pero puede requerir pequeño costes adicionales para software o hardware especializado. No tiene ningún requisito para los aceleradores de WAN o de seguridad de protocolo Internet (IPsec). El cifrado SMB puede configurarse por recurso compartido o para el servidor de archivos completo y se puede habilitar para una variedad de escenarios donde los datos atraviesan redes no confiables.

>[!NOTE]
>El cifrado SMB no cubre la seguridad en reposo, que normalmente se controla mediante el cifrado de unidad BitLocker.

El cifrado SMB debe considerarse para cualquier escenario en el que deben protegerse frente a ataques man-in-the-middle datos confidenciales. Los posibles escenarios incluyen:

- Se mueven datos confidenciales del trabajador de la información mediante el protocolo SMB. El cifrado SMB ofrece una garantía de privacidad e integridad de extremo a otro entre el servidor de archivos y el cliente, independientemente de las redes atravesados, por ejemplo, de área extensa (WAN) conexiones de red se mantienen por los proveedores que no sean de Microsoft.
- SMB 3.0 permite a los servidores de archivos proporcionar almacenamiento disponible continuamente para aplicaciones de servidor, como SQL Server o Hyper-V. Habilitación del cifrado SMB proporciona una oportunidad para proteger esa información de ataques de snooping. El cifrado SMB es más fácil de usar que las soluciones de hardware dedicado que son necesarias para la mayoría redes de área de almacenamiento (SAN).

>[!IMPORTANT]
>Tenga en cuenta que hay una notable de rendimiento operativo costo con cualquier protección de cifrado de extremo a extremo en comparación con sin cifrar.

## <a name="enable-smb-encryption"></a>Habilitar el cifrado SMB

Puede habilitar el cifrado SMB para el servidor de archivos completo o solo para recursos compartidos de archivos específico. Use uno de los procedimientos siguientes para habilitar el cifrado de SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar el cifrado SMB con Windows PowerShell

1. Para habilitar el cifrado SMB para un recurso compartido de archivos individuales, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar el cifrado SMB para el servidor de archivos completo, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para crear un nuevo recurso compartido SMB con habilitado el cifrado SMB, escriba el siguiente script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar el cifrado de SMB con el administrador del servidor

1. En el administrador del servidor, abra **File and Storage Services**.
2. Seleccione **recursos compartidos** para abrir la página de administración de recursos compartidos.
3. Haga clic en el recurso compartido en el que desea habilitar el cifrado SMB y, a continuación, seleccione **propiedades**.
4. En el **configuración** página del recurso compartido, seleccione **cifrar acceso a datos**. Acceso a este recurso compartido de archivos remoto está cifrado.

### <a name="considerations-for-deploying-smb-encryption"></a>Consideraciones para implementar el cifrado SMB

De forma predeterminada, cuando se habilita el cifrado SMB para un recurso compartido de archivos o un servidor, se permiten solo los clientes SMB 3.0 para tener acceso a los recursos compartidos de archivo especificado. Esto refuerza la intención del Administrador de protección de los datos para todos los clientes que tienen acceso a los recursos compartidos. Sin embargo, en algunas circunstancias, un administrador que desee permitir el acceso sin cifrar para los clientes que no admiten SMB 3.0 (por ejemplo, durante un período de transición cuando se emplean las versiones de sistemas operativos cliente mixtos). Para permitir el acceso sin cifrar para los clientes que no admiten SMB 3.0, escriba el siguiente script de Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La capacidad de negociación de dialecto segura que se describe en la sección siguiente evita que un ataque man-in-the-middle degradar una conexión de SMB 3.0 para SMB 2.0 (que podría usar el acceso sin cifrar). Sin embargo, no impide una degradación a SMB 1.0, lo que daría lugar también acceso sin cifrar. Para garantizar que los clientes SMB 3.0 usan siempre el cifrado SMB para acceder a recursos compartidos cifrados, debe deshabilitar al servidor de SMB 1.0. (Para obtener instrucciones, consulte la sección [deshabilitar SMB 1.0](#disabling-smb-1.0).) Si el **– RejectUnencryptedAccess** valor se deja en su valor predeterminado de **$true**, se permiten los clientes solo compatible con cifrado de SMB 3.0 para tener acceso a los recursos compartidos de archivos (también se rechazarán los clientes SMB 1.0).

>[!NOTE]
>* El cifrado SMB usa el estándar de cifrado avanzado (AES)-algoritmo CCM para cifrar y descifrar los datos. AES-CCM también proporciona validación de integridad de datos (firma) para recursos compartidos de archivos cifrados, independientemente de la configuración de firma de SMB. Si desea habilitar la firma sin cifrado SMB, puede seguir hacerlo. Para obtener más información, consulte [los conceptos básicos de la firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Puede encontrar problemas cuando se intenta tener acceso el recurso compartido de archivos o el servidor si su organización usa dispositivos de aceleración de área extensa (WAN) de la red.
>* Con una configuración predeterminada (donde no se permite a los recursos compartidos de archivos cifrados ningún acceso sin cifrar), si los clientes que no admiten SMB 3.0 intenta tener acceso a un recurso compartido de archivos cifrados, 1003 de Id. de evento se registra en el registro de eventos de Microsoft-Windows-SmbServer/Operational , y el cliente recibirá un **acceso denegado** mensaje de error.
>* No están relacionadas con el cifrado SMB y el sistema de archivos cifrados (EFS) en el sistema de archivos NTFS y el cifrado SMB no requiere ni dependen utilizando EFS.
>* No están relacionadas con el cifrado SMB y el cifrado de unidad BitLocker y el cifrado SMB no requiere ni dependen utilizando el cifrado de unidad BitLocker.

## <a name="secure-dialect-negotiation"></a>Negociación del dialecto segura

SMB 3.0 es capaz de detectar ataques man-in-the-middle que intentan cambiar el protocolo SMB 2.0 o SMB 3.0 o las funcionalidades que el cliente y servidor negociar. Cuando se detecta este tipo de ataque por el cliente o el servidor, la conexión se desconecta y 1005 de Id. de evento se registra en el registro de eventos de Microsoft-Windows-SmbServer/Operational. Proteger el dialecto de negociación no puede detectar ni evitar degradaciones de SMB 2.0 ó 3.0 para SMB 1.0. Por este motivo y para aprovechar todas las capacidades del cifrado SMB, se recomienda que deshabilite al servidor SMB 1.0. Para obtener más información, consulte [deshabilitar SMB 1.0](#disabling-smb-1.0).

La capacidad de negociación de dialecto segura que se describe en la sección siguiente evita que un ataque man-in-the-middle degradar una conexión entre 3 SMB y SMB 2 (lo que usaría un acceso sin cifrar); Sin embargo, no impide degradaciones SMB 1, lo que daría lugar también acceso sin cifrar. Para obtener más información sobre posibles problemas con versiones anteriores que no sean Windows implementaciones de SMB, consulte el [Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Algoritmo de firma nueva

SMB 3.0 usa un algoritmo de cifrado más reciente para la firma: Estándar de cifrado avanzado (AES) - cipher - en una función de código de autenticación de mensajes (CMAC). SMB 2.0 utiliza el algoritmo de cifrado antiguo de HMAC-SHA256. AES CMAC y AES-CCM pueden acelerar considerablemente el cifrado de datos en las CPU más modernas que tienen la instrucción de AES de soporte técnico. Para obtener más información, consulte [los conceptos básicos de la firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Deshabilitación de SMB 1.0

El servicio Examinador de equipos heredado y las características de protocolo de administración remota de SMB 1.0 ahora son independientes, y puede eliminarse. Estas características todavía están habilitadas de forma predeterminada, pero si no tiene clientes anteriores de SMB, como los equipos que ejecutan Windows Server 2003 o Windows XP, puede quitar las características de SMB 1.0 para aumentar la seguridad y reducir la posible aplicación de revisiones.

>[!NOTE]
>SMB 2.0 se introdujo en Windows Server 2008 y Windows Vista. Los clientes más antiguos, como los equipos que ejecutan Windows Server 2003 o Windows XP, no admiten SMB 2.0; y, por lo tanto, no podrán tener acceso a recursos compartidos de archivos o recursos compartidos de impresión si el servidor SMB 1.0 está deshabilitado. Además, algunos clientes SMB que no sea Microsoft no puede tener acceso a recursos compartidos de archivos de SMB 2.0 o imprimir los recursos compartidos (por ejemplo, impresoras con la funcionalidad de "examen para compartir").

Antes de empezar a deshabilitar SMB 1.0, deberá averiguar si los clientes SMB están conectados actualmente al servidor que ejecuta SMB 1.0. Para ello, escriba el siguiente cmdlet de Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Debe ejecutar este script varias veces en el transcurso de una semana (varias veces al día) para crear una traza de auditoría. También podría ejecutar como una tarea programada.

Para deshabilitar SMB 1.0, escriba el siguiente script de Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si una conexión de cliente SMB se deniega porque se ha deshabilitado el servidor de SMB 1.0, identificador de evento 1001 se registrará en el registro de eventos de Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Más información

Estos son algunos recursos adicionales acerca de SMB y tecnologías relacionadas en Windows Server 2012.

- [Bloque de mensajes del servidor](file-server-smb-overview.md)
- [Almacenamiento en Windows Server](../storage.md)
- [Servidor de archivos de escalabilidad horizontal para datos de la aplicación](../../failover-clustering/sofs-overview.md)