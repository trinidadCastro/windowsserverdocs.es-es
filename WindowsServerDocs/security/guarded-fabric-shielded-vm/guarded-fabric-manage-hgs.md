---
title: Administrar el servicio de protección de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: ed3a3d4c5d0e55126f4dae8ecaf0ba1f32e46317
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820226"
---
# <a name="managing-the-host-guardian-service"></a>Administrar el servicio de protección de Host

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

El servicio de protección de Host (HGS) es la pieza central de la solución de tejido protegido.
Es responsable de garantizar que los hosts de Hyper-V en el tejido se sabe que el proveedor de hospedaje o enterprise y ejecutar software de confianza y para administrar las claves utilizadas para iniciar las máquinas virtuales blindadas.
Cuando un inquilino decide confiar usted para hospedar sus máquinas virtuales blindadas, coloca su confianza en la configuración y administración del servicio guardián de Host.
Por lo tanto, es muy importante seguir las prácticas recomendadas al administrar el servicio de protección de Host para garantizar la seguridad, disponibilidad y confiabilidad de su tejido protegido.
Las instrucciones de las siguientes secciones aborda los problemas operativos más comunes que enfrentan los administradores de HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitar el acceso de administrador a HGS
Dada la naturaleza confidencial de seguridad de HGS, es importante asegurarse de que sus administradores son miembros de plena confianza de su organización y, idealmente, se separan de los administradores de los recursos de tejido.
Además, se recomienda que administre solo HGS desde estaciones de trabajo seguras mediante protocolos de comunicación segura, como WinRM a través de HTTPS.

### <a name="separation-of-duties"></a>Separación de tareas
Al configurar HGS, tiene la opción de crear un bosque de Active Directory aislado solo para HGS o para unir HGS a un dominio existente y de confianza.
Esta decisión, así como las funciones que asignar los administradores de su organización, determinan el límite de confianza de HGS.
Persona que tiene acceso a HGS, ya sea directamente como un administrador o indirectamente como un administrador de otra cosa (por ejemplo, Active Directory) que puede influir en el HGS, tiene control sobre el tejido protegido.
Los administradores HGS elegir qué hosts de Hyper-V están autorizados para ejecutar máquinas virtuales blindadas y administrar los certificados necesarios para iniciar las máquinas virtuales blindadas.
Un atacante o un administrador malintencionado quién tiene acceso a HGS puede usar esta capacidad para autorizar a los hosts en peligro para ejecutar máquinas virtuales blindadas, iniciar un ataque por denegación de servicio mediante la eliminación de material de clave y mucho más.

Para evitar este riesgo, es *fuertemente* recomienda que limite la superposición entre los administradores de su HGS (incluido el dominio al que está unido HGS) y entornos Hyper-V.
Asegurándose de que no hay un administrador tiene acceso a ambos sistemas, un atacante tendría que poner en peligro 2 diferentes cuentas de 2 personas para completar su misión para cambiar las directivas HGS.
Esto también significa que los administradores de dominio y empresarial para los dos entornos de Active Directory no deben ser la misma persona ni HGS debe usar el mismo bosque de Active Directory como los hosts de Hyper-V.
Cualquier persona que ellos mismos puede conceder acceso a más recursos plantea un riesgo de seguridad.

### <a name="using-just-enough-administration"></a>Usar Just Enough Administration
HGS viene con [Just Enough Administration](https://aka.ms/JEAdocs) roles (JEA) integrados para ayudarle a administrarlo de forma más segura.
JEA ayuda por lo que le permite delegar tareas administrativas a los usuarios sin derechos administrativos, lo que significa que las personas que administran las directivas de HGS no tiene que ser realmente los administradores del dominio o toda la máquina.
JEA funciona limitar qué comandos puede ejecutar un usuario en una sesión de PowerShell y el uso de una cuenta local temporal en segundo plano (único para cada sesión de usuario) para ejecutar los comandos que normalmente requieren la elevación.

HGS se suministra con 2 roles de JEA preconfiguradas:
- **Los administradores de HGS** que permite a los usuarios administrar todas las directivas HGS, incluidas autorizar nuevos hosts para ejecutar máquinas virtuales blindadas.
- **Los revisores de HGS** que sólo permite a los usuarios el derecho a las directivas existentes de auditoría. Pero no realizan cambios en la configuración de HGS.

Para usar JEA, primero deberá crear un nuevo usuario estándar y que sean miembro del HGS administradores o del grupo de revisores HGS.
Si ha usado `Install-HgsServer` para configurar un bosque nuevo de HGS, estos grupos se denominará "*servicename*administradores" y "*servicename*revisores", respectivamente, donde *servicename*  es el nombre de red del clúster HGS.
Si HGS se unió a un dominio existente, debe hacer referencia a los nombres de grupo que especificó en `Initialize-HgsServer`.

**Crear usuarios estándares para los roles de administrador y revisor HGS**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Directivas de auditoría con el rol de revisor**

En un equipo remoto que tenga conectividad de red con HGS, ejecute los siguientes comandos de PowerShell para iniciar la sesión JEA con las credenciales de revisor.
Es importante tener en cuenta que dado que la cuenta de revisor es simplemente un usuario estándar, no puede utilizarse para regular comunicación remota de Windows PowerShell, acceso de escritorio remoto a HGS, etcetera.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

A continuación, puede comprobar qué comandos se permiten en la sesión con `Get-Command` y ejecute los comandos permitidos para la configuración de auditoría.
En el ejemplo siguiente, vamos a comprobar las directivas que están habilitadas en el HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Escriba el comando `Exit-PSSession` o su alias, `exit`, cuando termine de trabajar con la sesión de JEA. 

**Agregar una nueva directiva para usar el rol de administrador HGS**

Para modificar una directiva, deberá conectarse al punto de conexión JEA con una identidad que pertenezca al grupo 'hgsAdministrators'.
En el ejemplo siguiente, se muestra cómo copiar una nueva directiva de integridad de código en HGS y registrarlo con JEA.
La sintaxis puede ser diferente de lo que se usan para.
Esto sirve para dar cabida a algunas de las restricciones de JEA como no tener acceso al sistema de archivos completa.

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>Supervisión de HGS
### <a name="event-sources-and-forwarding"></a>Reenvío y orígenes de eventos
Los eventos de HGS mostrarán en el registro de eventos de Windows en 2 orígenes:
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

Puede ver estos eventos, abra el Visor de eventos y navegue hasta atestación-Microsoft-Windows-HostGuardianService y Microsoft-Windows-HostGuardianService-KeyProtection.

En un entorno de gran tamaño, a menudo es preferible para reenviar eventos a un recopilador de eventos de Windows central para facilitar el análisis de los eventos.
Para obtener más información, consulte el [documentación de reenvío de eventos de Windows](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>Con System Center Operations Manager
También puede usar System Center 2016 - Operations Manager para supervisar los hosts protegidos y el HGS.
El módulo de administración de tejido protegido tiene los monitores de eventos para comprobar las configuraciones erróneas comunes que pueden dar lugar a tiempos de inactividad del centro de datos, incluidos los hosts que no supera la atestación y servidores HGS informan de errores.

Para empezar a trabajar, [instalar y configurar SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) y [descargar el módulo de administración de tejido protegido](https://www.microsoft.com/download/details.aspx?id=52764).
La Guía del módulo de administración incluidos explica cómo configurar el módulo de administración y comprender el ámbito de sus monitores.

## <a name="backing-up-and-restoring-hgs"></a>Copia de seguridad y restauración de HGS
### <a name="disaster-recovery-planning"></a>Planeamiento de la recuperación ante desastres
Al esbozar los planes de recuperación ante desastres, es importante tener en cuenta los requisitos únicos de los servicio guardián de Host en el tejido protegido.
Si pierde algunos o todos los nodos HGS, pueden enfrentar problemas de disponibilidad inmediata que impiden que los usuarios iniciar sus máquinas virtuales blindadas.
En un escenario donde se pierde todo el clúster HGS, deberá tener copias de seguridad completadas de la configuración de HGS disponibles para el clúster HGS de restaurar y reanudar las operaciones normales.
En esta sección se trata los pasos necesarios para prepararse para este escenario.

En primer lugar, es importante comprender qué sucede HGS es importante realizar copias de seguridad.
HGS conserva varios fragmentos de información para ayudarle a determinan qué hosts están autorizados a ejecutar máquinas virtuales blindadas.
Esto incluye:
1. Identificadores de seguridad de Active Directory para los grupos que contengan confianza hosts (cuando se usa la atestación de Active Directory);
2. Identificadores únicos de TPM para cada host de su entorno;
3. Directivas TPM para cada configuración única de host; y
4. Directivas de integridad de código que determinan qué software se puede ejecutar en los hosts.

Estos artefactos de atestación requieren la coordinación con los administradores de su tejido de hospedaje para obtener, potencialmente y es difícil obtener esta información de nuevo tras un desastre.

Además, HGS requiere acceso a 2 o más certificados usados para cifrar y firmar la información necesaria para iniciar una máquina virtual blindada (el protector de clave).
Estos certificados son bien conocidos (usa los propietarios de las máquinas virtuales blindadas para autorizar el tejido para ejecutar sus máquinas virtuales) y deben restaurarse después de un desastre para una experiencia de recuperación sin problemas.
Debe no restaurar HGS con los mismos certificados después de un desastre, cada máquina virtual deberá actualizarse para autorizar a las claves para descifrar la información de nuevo.
Por motivos de seguridad, solo el propietario de la máquina virtual pueda actualizar la configuración de máquina virtual para autorizar a estas nuevas claves, el error significado para restaurar las claves después de un desastre se producirá cada propietario de la máquina virtual que necesitan tomar medidas para obtener sus máquinas virtuales que se ejecute de nuevo.

#### <a name="preparing-for-the-worst"></a>Preparación para lo peor
Para preparar una pérdida completa de HGS, hay 2 pasos que debe realizar:
1. Realizar copias de seguridad de las directivas de atestación HGS
2. Realizar una copia de seguridad de las claves HGS

Se proporciona orientación sobre cómo realizar estos dos pasos en el [copia de seguridad HGS](#backing-up-hgs) sección.

Se recomienda Además, aunque no es necesario, realizar copias de seguridad de la lista de usuarios autorizados a administrar HGS en su dominio de Active Directory o Active Directory.

Las copias de seguridad deben realizarse con regularidad para asegurarse de que la información está actualizada y almacenados de forma segura para evitar la manipulación o el robo.

Es **no recomienda** para realizar copias de seguridad o intenta restaurar una imagen de sistema completo de un nodo de HGS.
En caso de que ha perdido todo el clúster, el procedimiento recomendado es configurar un nuevo nodo HGS y restaurar solo el estado HGS, no todo el servidor del sistema operativo.

#### <a name="recovering-from-the-loss-of-one-node"></a>Recuperación de la pérdida de un nodo
Si se pierden uno o más nodos (pero no todos los nodos) en el clúster HGS, puede simplemente [agregar nodos al clúster](guarded-fabric-configure-additional-hgs-nodes.md) siguiendo las instrucciones de la Guía de implementación.
Las directivas de atestación se sincronizarán automáticamente, así como los certificados que se proporcionaron HGS como archivos PFX con que lo acompañan contraseñas.
Para los certificados agregados a HGS mediante una huella digital (no exportable y respaldo de hardware de los certificados, con frecuencia), deberá asegurarse de que cada nuevo nodo tiene acceso a la clave privada de cada certificado.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Recuperación de la pérdida de todo el clúster
Si todo el clúster HGS deja de funcionar y no puede ponerlo en línea, deberá restaurar HGS desde una copia de seguridad.
La restauración de HGS desde una copia de seguridad implica la primera que configura un nuevo clúster HGS por la [instrucciones en la Guía de implementación](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
Es muy recomendable, pero no es necesario, para usar el mismo nombre de clúster al configurar el entorno de recuperación de HGS para ayudar con la resolución de nombres de hosts.
Con el mismo nombre, evita tener que volver a configurar los hosts con direcciones URL de protección de claves y atestación de nuevo.
Si restaura los objetos en el dominio de Active Directory HGS de seguridad, se recomienda que quite los objetos que representan el clúster HGS, equipos, cuenta de servicio y grupos JEA antes de inicializar un servidor HGS.

Una vez haya configurado el primer nodo HGS (por ejemplo, se ha instalado e inicializado), se siguen los procedimientos en [HGS restaurar una copia de seguridad](#restoring-hgs-from-a-backup) para restaurar las directivas de atestación y mitades públicas de la protección de clave certificados.
Deberá restaurar las claves privadas de los certificados manualmente según las instrucciones de su proveedor de certificados (por ejemplo, importe el certificado en Windows, o configurar el acceso a certificados respaldada por HSM).
Después de configura el primer nodo, puede seguir [instalar nodos adicionales al clúster](guarded-fabric-configure-additional-hgs-nodes.md) hasta que hayan alcanzado la capacidad y resistencia que desee.

### <a name="backing-up-hgs"></a>Copia de seguridad HGS
El Administrador de HGS debe ser responsable de la copia de seguridad de HGS de forma periódica.
Una copia de seguridad completa contiene información importante que debe protegerse correctamente.
Debe una entidad de confianza acceder a estas claves, podría utilizar que material para configurar un entorno de HGS malintencionado con el fin de poner en peligro las máquinas virtuales blindadas.

**La copia de seguridad de las directivas de atestación** a atrás las directivas de atestación HGS, ejecute el siguiente comando en cualquier nodo de servidor HGS de trabajo.
Se le pedirá que proporcione una contraseña.
Esta contraseña se utiliza para cifrar los certificados agregados a HGS mediante un archivo PFX (en lugar de una huella digital del certificado).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Si usa la atestación de administrador de confianza, por separado debe realizar una copia pertenencia en los grupos de seguridad utilizado por el HGS para autorizar a los hosts protegidos.
> HGS solo copia el SID de los grupos de seguridad, no la pertenencia a ellos.
> En caso de que estos grupos se pierden durante un desastre, deberá volver a crear los grupos y vuelva a agregar cada host protegido a ellos.

**La copia de seguridad de certificados**

El `Export-HgsServerState` comando realizará una copia de los certificados basados en PFX agregados a HGS en el momento se ejecuta el comando.
Si agrega certificados a HGS mediante una huella digital (típica para los certificados no exportable y respaldo de hardware), deberá realizar copias de seguridad de las claves privadas de los certificados manualmente.
Para identificar qué certificados están registrados en HGS y necesitan una copia de seguridad manualmente, ejecute el siguiente comando de PowerShell en cualquier nodo de servidor HGS de trabajo.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Para cada uno de los certificados que se muestran, deberá realizar manualmente una copia de seguridad de la clave privada.
Si usa certificados basados en software que no es exportable, póngase en contacto con su entidad emisora de certificados para asegurarse de que tienen una copia de seguridad de su certificado, o pueden volver a enviarlo a petición.
Para los certificados creados y almacenados en módulos de seguridad de hardware, debe consultar la documentación para el dispositivo para obtener instrucciones sobre el planeamiento de la recuperación ante desastres.

Debe almacenar las copias de seguridad del certificado junto con las copias de seguridad de directiva de atestación en una ubicación segura para que ambas partes se pueden restaurar conjuntamente.

**Configuración adicional para realizar copias de seguridad**

La copia de seguridad HGS, estado del servidor no incluirá el nombre del clúster HGS, toda la información de Active Directory, o usan los certificados SSL para proteger las comunicaciones con las API de HGS.
Esta configuración es importante para mantener la coherencia, pero no son fundamentales para obtener el clúster HGS en línea después de un desastre.

Para capturar el nombre del servicio HGS, ejecute `Get-HgsServer` y anote el nombre de la atestación y protección de clave de las direcciones URL sin formato.
Por ejemplo, si la dirección URL de atestación es "http://hgs.contoso.com/Attestation", "hgs" es el nombre de servicio HGS.

El dominio de Active Directory que usan HGS debe administrarse como cualquier otro dominio de Active Directory.
Al restaurar HGS tras un desastre, no necesariamente debe volver a crear los objetos exactos que se encuentran en el dominio actual.
Sin embargo, facilitarán recuperación si realizar una copia de seguridad de Active Directory y mantener una lista de los usuarios JEA autorizado para administrar el sistema, así como la pertenencia de los grupos de seguridad utilizado por la atestación de administrador de confianza para autorizar a los hosts protegidos.

Para identificar la huella digital de los certificados SSL configurados para HGS, ejecute el siguiente comando en PowerShell.
A continuación, hacer copias de seguridad los certificados SSL, según las instrucciones del proveedor de su certificado.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>HGS restauración desde una copia de seguridad
Los pasos siguientes describen cómo restaurar la configuración de HGS desde una copia de seguridad.
Los pasos son pertinentes para ambas situaciones donde intenta deshacer los cambios realizados a las instancias HGS está ya en ejecución y cuándo se va a configurar un clúster de HGS de nuevo después de una pérdida completa de la anterior.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurar un clúster HGS de reemplazo
Antes de poder restaurar HGS, deberá tener un clúster HGS inicializado a la que pueda restaurar la configuración.
Si simplemente va a importar la configuración que se eliminaron accidentalmente a un clúster (en ejecución) existente, puede omitir este paso.
Si va a recuperar de una pérdida completa de HGS, deberá instalar e inicializar las siguientes acciones al menos un nodo HGS el [instrucciones en la Guía de implementación](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

En concreto, deberá:
1. [Configurar el dominio HGS](guarded-fabric-choose-where-to-install-hgs.md) o unir HGS a un dominio existente
2. [Inicializar el servidor HGS](guarded-fabric-initialize-hgs.md) mediante las claves existentes *o* un conjunto de claves temporales. También puede [quitar las claves temporales](#renewing-or-replacing-keys) después de importar las claves reales desde el HGS archivos de copia de seguridad.
3. [Importar la configuración de HGS](#import-settings-from-a-backup) desde la copia de seguridad para restaurar los grupos host de confianza, las directivas de integridad de código, líneas de base de TPM y los identificadores TPM

> [!TIP]
> El nuevo clúster HGS no es necesario usar el mismo dominio, nombre de servicio o los certificados que la instancia HGS desde el que se exportó el archivo de copia de seguridad.

#### <a name="import-settings-from-a-backup"></a>Importar la configuración de una copia de seguridad

Para restaurar la atestación de directivas, los certificados PFX y las claves públicas de los certificados que no sean PFX en el nodo HGS desde un archivo de copia de seguridad, ejecutan el siguiente comando en un nodo de servidor HGS inicializado.
Se le pedirá que escriba la contraseña que especificó al crear la copia de seguridad.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Si sólo desea importar las directivas de atestación de administrador de confianza o atestación de TPM de confianza, puede hacerlo especificando el `-ImportActiveDirectoryModeState` o `-ImportTpmModeState` marcas a [importación HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Asegúrese de que la actualización acumulativa más reciente para Windows Server 2016 está instalado antes de ejecutar `Import-HgsServerState`.
Si no lo hace puede producir un error de importación.

> [!NOTE]
> Si restaura las directivas en un nodo HGS existente que ya tiene uno o más de esas directivas instaladas, el comando de importación mostrará un error para cada directiva duplicada.
> Esto es un comportamiento esperado y puede omitirse en la mayoría de los casos.

#### <a name="reinstall-private-keys-for-certificates"></a>Vuelva a instalar las claves privadas de certificados
Si cualquiera de los certificados usados en el HGS desde el que se creó la copia de seguridad se han agregado mediante huellas digitales, solo la clave pública de los certificados se incluirán en el archivo de copia de seguridad.
Esto significa que deberá instalar manualmente o conceder acceso a las claves privadas para cada uno de esos certificados antes de HGS puede atender solicitudes de los hosts de Hyper-V.
Las acciones necesarias para completar ese paso varía en función de cómo el certificado se emitió originalmente.
Para los certificados de software de seguridad emitidos por una entidad emisora de certificados, deberá ponerse en contacto con la entidad de certificación para obtener la clave privada e instálelo en **cada** nodo HGS por sus instrucciones.
De forma similar, si los certificados de respaldo de hardware, deberá consultar la hardware security module documentación del fabricante para instalar los controladores necesarios en cada nodo HGS para conectarse a lo HSM y concedérselo a cada máquina a la clave privada.

Como recordatorio, agregados a HGS mediante huellas digitales de certificados requieren replicación manual de las claves privadas a cada nodo.
Deberá repetir este paso en cada nodo adicional que agregue al clúster HGS restaurado.

#### <a name="review-imported-attestation-policies"></a>Revise las directivas de atestación importados
Después de haber importado la configuración de una copia de seguridad, se recomienda revisar detenidamente de todas las directivas importadas utilizando `Get-HgsAttestationPolicy` para asegurarse de que solo los hosts de confianza para ejecutar máquinas virtuales blindadas pueda correctamente dar fe.
Si encuentra cualquier directiva que ya no coincide con la posición de seguridad, puede [deshabilitar o quítelas](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Ejecute los diagnósticos para comprobar el estado del sistema
Cuando haya terminado de configurar y restaurar el estado del nodo HGS, debe ejecutar la herramienta de diagnóstico HGS para comprobar el estado del sistema.
Para ello, ejecute el siguiente comando en el nodo HGS donde haya restaurado la configuración:

```powershell
Get-HgsTrace -RunDiagnostics
```

Si el "resultado general" es "Pasar" no, pasos adicionales deben finalizar la configuración del sistema.
Compruebe los mensajes notificados en el que no se pudo subtest(s) para obtener más información.

## <a name="patching-hgs"></a>Aplicación de revisiones de HGS
Es importante mantener los nodos del servicio guardián de Host al día mediante la instalación de la actualización acumulativa más reciente cuando se trata de. Si está configurando un nuevo nodo HGS, se recomienda que instale las actualizaciones disponibles antes de instalar el rol HGS o configurarlo.
Esto garantizará que cualquier nueva o modificada funcionalidad surtirá efecto inmediatamente.

Al aplicar revisiones en el tejido protegido, se recomienda encarecidamente que actualice primero *todas* hosts de Hyper-V **antes de actualizar HGS**.
Esto es para asegurarse de que se realizan cambios en las directivas de atestación de HGS *después* los hosts de Hyper-V se han actualizado para proporcionar la información necesaria para ellos.
Si una actualización se va a cambiar el comportamiento de las directivas, no se activa de manera automática para evitar interrumpir a su tejido.
Estas actualizaciones requieren que siga las instrucciones en la sección siguiente para activar las directivas nuevas o modificadas de atestación.
Le recomendamos que lea las notas de la versión para Windows Server y las actualizaciones acumulativas que se instala para comprobar si se requieren las actualizaciones de directiva.

### <a name="updates-requiring-policy-activation"></a>Actualizaciones que requieren activación de la directiva
Si una actualización de HGS presenta o cambia el comportamiento de una directiva de atestación de forma significativa, es necesario un paso adicional para activar la Directiva modificada.
Cambios en las directivas entran en vigor solo después de exportar e importar el estado HGS.
Solo debe activar las directivas nuevas o modificadas después de haber aplicado la actualización acumulativa a todos los hosts y todos los nodos HGS en su entorno.
Una vez que se ha actualizado todos los equipos, ejecute los comandos siguientes en cualquier nodo HGS para desencadenar el proceso de actualización:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Si se introdujo una nueva directiva, se deshabilitarán de forma predeterminada.
Para habilitar la nueva directiva, primero se encuentra en la lista de las directivas de Microsoft (como precedida 'HGS_') y, a continuación, habilitarlo mediante los siguientes comandos:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Administración de directivas de atestación
HGS mantiene varias directivas de atestación que definen el conjunto mínimo de los requisitos de que un host debe cumplir para que se consideran "correctos" y se pueden ejecutar las máquinas virtuales blindadas.
Algunas de estas directivas se definen por Microsoft, más se agregaron por el usuario para definir las directivas de integridad de código permitidos, líneas de base de TPM y hosts en su entorno.
Mantenimiento periódico de estas directivas es necesario para garantizar los hosts puedan continuar avalar correctamente como actualizar y reemplazarlos, y para asegurarse de todos los hosts de confianza o configuraciones están bloqueadas en avalar correctamente.

Para la atestación de administrador de confianza, hay solo una directiva que determina si un host está en buen estado: pertenencia a un grupo de seguridad conocidos y de confianza.
Atestación de TPM es más complicado e implica varias directivas para medir el código y la configuración de un sistema antes de determinar si es correcto.

Un único HGS puede configurarse con las directivas de Active Directory y TPM a la vez, pero el servicio solo comprobará las directivas para el modo actual que está configurada para cuando un host intenta avalar.
Para comprobar el modo del servidor HGS, ejecute `Get-HgsServer`.

### <a name="default-policies"></a>Directivas predeterminadas
Para la atestación de TPM de confianza, hay varias directivas integradas configuradas en HGS.
Algunas de estas directivas están "bloqueadas", lo que significa que no puede deshabilitarse por motivos de seguridad.
En la tabla siguiente se explica el propósito de cada directiva predeterminada.

Nombre de directiva                    | Finalidad
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Requiere los hosts tengan habilitado el arranque seguro. Esto es necesario medir los archivos binarios de inicio y otras opciones bloqueado en UEFI.
Hgs_UefiDebugDisabled          | Garantiza que los hosts no tiene habilitado un depurador del kernel. Los depuradores de modo de usuario se bloquean con directivas de integridad de código.
Hgs_SecureBootSettings         | Directiva negativo para asegurarse de hosts coincide con al menos una instantánea TPM (definido por el administrador).
Hgs_CiPolicy                   | Directiva negativo para asegurarse de hosts está utilizando una de las directivas de CI definido por el administrador.
Hgs_HypervisorEnforcedCiPolicy | Requiere la directiva de integridad de código que se aplicará el hipervisor. Si deshabilita esta directiva, debilita la protección contra ataques de directiva de integridad de código de modo kernel.
Hgs_FullBoot                   | Garantiza que el host no reanudar en modo de suspensión o hibernación. Hosts deben ser reiniciar o apagar para pasar esta directiva correctamente.
Hgs_VsmIdkPresent              | Requiere la seguridad en función de virtualización que se esté ejecutando en el host. El IDK representa la clave necesaria para cifrar la información enviada al espacio de memoria seguro del host.
Hgs_PageFileEncryptionEnabled  | Requiere los archivos de paginación que se cifren en el host. Si deshabilita esta directiva, se podría producir exposición de información si un archivo de paginación sin cifrar se inspecciona para los secretos del inquilino.
Hgs_BitLockerEnabled           | Requiere BitLocker esté habilitado en el host de Hyper-V. Esta directiva está deshabilitada de forma predeterminada por motivos de rendimiento y no se recomienda que se habilite. Esta directiva no influye en el cifrado de las máquinas virtuales blindadas a sí mismos.
Hgs_IommuEnabled               | Requiere que el host tenga un dispositivo IOMMU en uso para evitar ataques de acceso directo a la memoria. Si deshabilita esta directiva y el uso de hosts sin un IOMMU habilitada pueden exponer los secretos de máquina virtual de inquilino para dirigir los ataques de memoria.
Hgs_NoHibernation              | Requiere la hibernación se deshabilite en el host de Hyper-V. Si deshabilita esta directiva podría permitir que los hosts ahorrar memoria VM blindada a un archivo de hibernación sin cifrar.
Hgs_NoDumps                    | Requiere que se deshabilite en el host de Hyper-V los volcados de memoria. Si deshabilita esta directiva, se recomienda que configure el cifrado de volcado de memoria para impedir que se va a guardar en archivos de volcado de bloqueo sin cifrar memoria de máquina virtual blindada.
Hgs_DumpEncryption             | Requiere los volcados de memoria, si se habilita en el host de Hyper-V, se cifre con una clave de cifrado de HGS de confianza. Esta directiva no es aplicable si volcados de memoria no están habilitadas en el host. Si esta directiva y *Hgs\_NoDumps* son ambos deshabilitado, memoria de máquina virtual blindada pudieran guardarse en un archivo de volcado de memoria sin cifrar.
Hgs_DumpEncryptionKey          | Directiva negativo para garantizar los hosts configurados para permitir que usan una clave de cifrado de archivo de volcado de memoria definido por el administrador sabe que HGS volcados de memoria. Esta directiva no aplica cuando *Hgs\_DumpEncryption* está deshabilitado.

### <a name="authorizing-new-guarded-hosts"></a>Autorizar nuevos hosts protegidos
Para autorizar a un nuevo host para convertirse en un host protegido (por ejemplo, dar fe correctamente), HGS debe confiar en el host y (cuando se configura para usar la atestación de TPM de confianza) el software que se ejecuta en él.
Los pasos para autorizar a un nuevo host varían según el modo de atestación que HGS está configurado actualmente.
Para comprobar el modo de atestación para el tejido protegido, ejecute `Get-HgsServer` en cualquier nodo HGS.

#### <a name="software-configuration"></a>Configuración de software
En el nuevo host de Hyper-V, asegúrese de que Windows Server 2016 Datacenter edition está instalado.
Windows Server 2016 Standard no se puede ejecutar máquinas virtuales blindadas en un tejido protegido.
El host puede ser instalar experiencia de escritorio o Server Core.

En el servidor con experiencia de escritorio y Server Core, deberá instalar los roles de servidor Hyper-V y compatibilidad de Hyper-V de guardián de Host:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Atestación de administrador de confianza
Para registrar un nuevo host en HGS cuando se usa la atestación de administrador de confianza, primero debe agregar el host a un grupo de seguridad en el dominio al que está unido.
Normalmente, cada dominio tendrán un grupo de seguridad para hosts protegidos.
Si ya ha registrado ese grupo con HGS, es la única acción que debe realizar reiniciar el host para actualizar a su pertenencia al grupo.

Puede comprobar qué grupos de seguridad son de confianza HGS, ejecute el comando siguiente:

```powershell
Get-HgsAttestationHostGroup
```

Para registrar un nuevo grupo de seguridad con HGS, capture el identificador de seguridad (SID) del grupo en el dominio del host y registrar al SID con HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Instrucciones sobre cómo configurar la confianza entre el dominio de host y el HGS están disponibles en la Guía de implementación.

#### <a name="tpm-trusted-attestation"></a>Atestación de TPM de confianza
Cuando HGS está configurado en modo TPM, hosts deben pasar todas las directivas de bloqueadas y directivas "habilitado", el prefijo "Hgs_", así como al menos una línea de base TPM, el identificador TPM y directiva de integridad de código.
Cada vez que agregue un nuevo host, deberá registrar el nuevo identificador TPM con HGS.
Siempre que el host se está ejecutando el mismo software (y se aplica la misma directiva de integridad de código) y la línea de base TPM como otro host en su entorno, no necesitará agregar nuevas directivas de CI o líneas de base.

**Adición del identificador de TPM para un nuevo host** en el nuevo host, ejecute el siguiente comando para capturar el identificador TPM.
Asegúrese de especificar un nombre único para el host que le ayudará a buscar en HGS.
Necesitará esta información si desea impedir que se está ejecutando las máquinas virtuales blindadas en HGS o retirar el host.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copie este archivo en el servidor HGS, a continuación, ejecute el siguiente comando para registrar el host con HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Agregar una nueva línea de base TPM** si el nuevo host se está ejecutando un nuevo hardware o la configuración de firmware para su entorno, es posible que deba tomar una nueva instantánea TPM.
Para ello, ejecute el siguiente comando en el host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Si recibe un error que indica que el host de un error de validación y no podrá atestiguar correctamente, no se preocupe.
> Esto es una comprobación de requisitos previos para asegurarse de que el host puede ejecutar las máquinas virtuales blindadas y probablemente significa que aún no ha aplicado una directiva de integridad de código u otros configuración requerida.
> Leer el mensaje de error, realice los cambios sugeridos por él y vuelva a intentarlo.
> Como alternativa, puede omitir la validación en este momento agregando el `-SkipValidation` marca al comando.

Copie la línea de base TPM al servidor HGS, a continuación, registrarlo con el siguiente comando.
Le recomendamos que utilice una convención de nomenclatura que le ayudará a comprender la configuración de hardware y firmware de esta clase de host de Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Agregar una nueva directiva de integridad de código** si ha cambiado la directiva de integridad de código que se ejecutan en los hosts de Hyper-V, deberá registrar la nueva directiva con HGS antes de que estos hosts atestigua correctamente.
En un host de referencia, que actúa como una imagen maestra para las máquinas de Hyper-V de confianza en su entorno, capturar una nueva directiva de CI mediante el `New-CIPolicy` comando.
Le recomendamos que use el **FilePublisher** nivel y **Hash** reserva para las directivas de CI de host de Hyper-V.
En primer lugar debe crear una directiva de CI en modo de auditoría para asegurarse de que todo funciona según lo previsto.
Después de validar una carga de trabajo de ejemplo en el sistema, puede aplicar la directiva y copie la versión exigida en HGS.
Para obtener una lista completa de opciones de configuración de directiva de integridad de código, consulte el [documentación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

Una vez que la directiva creada, probado y se apliquen, copie el archivo binario (. p7b) al servidor HGS y registre la directiva.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Agregar una clave de cifrado de volcado de memoria**

Cuando el *Hgs\_NoDumps* directiva está deshabilitada y *Hgs\_DumpEncryption* directiva está habilitada, los hosts protegidos pueden tener los volcados de memoria (incluidos los volcados de memoria) para ser habilitado, siempre se cifran los volcados de memoria. Los hosts protegidos sólo pasará atestación si tiene volcados de memoria deshabilitados o cifraban con una clave conocida por el HGS. De forma predeterminada, no hay claves de cifrado de volcado de memoria se configuran en HGS.

Para agregar una clave de cifrado de volcado de memoria a HGS, use el `Add-HgsAttestationDumpPolicy` cmdlet para proporcionar HGS con el hash de la clave de cifrado de volcado de memoria.
Si captura una instantánea TPM en un host de Hyper-V configurado con el cifrado de volcado de memoria, el hash se incluye en el tcglog y se pueden proporcionar a los `Add-HgsAttestationDumpPolicy` cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Como alternativa, puede proporcionar directamente la representación de cadena del valor hash para el cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

No olvide agregar cada clave de cifrado de volcado de memoria exclusivos a HGS si opta por usar claves diferentes a través de su tejido protegido.
Hosts que están cifrando los volcados de memoria con una clave no conocida el HGS no pasará la atestación.

Consulte la documentación de Hyper-V para obtener más información acerca de [configuración de cifrado en los hosts de volcado de memoria](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).

#### <a name="check-if-the-system-passed-attestation"></a>Compruebe si el sistema pasado la atestación
Después de registrar la información necesaria con HGS, debe comprobar si el host pase la atestación.
En el host de Hyper-V recién agregado, ejecute `Set-HgsClientConfiguration` y proporcionar las direcciones URL correctas para el clúster HGS.
Estas direcciones URL se pueden obtener ejecutando `Get-HgsServer` en cualquier nodo HGS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Si no indica el estado resultante "IsHostGuarded: True"si que necesita solucionar problemas de configuración.
En el host que no se pudo atestación, ejecute el comando siguiente para obtener un informe detallado sobre los problemas que pueden ayudarle a resolver la atestación con errores.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Si usa Windows Server 2019 o Windows 10, versión 1809 y está usando directivas de integridad de código, `Get-HgsTrace` puede devolver un error para el **directiva de integridad de código activo** diagnóstico.
> Puede ignorar este resultado cuando es el solo errores diagnóstico.

### <a name="review-attestation-policies"></a>Revise las directivas de atestación
Para revisar el estado actual de las directivas configuradas en HGS, ejecute los comandos siguientes en cualquier nodo HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Si se encuentra habilitada una directiva que ya no cumple los requisitos de seguridad (por ejemplo, un antiguo código directiva de integridad que ahora se considera no seguro), puede deshabilitarlo si se reemplaza el nombre de la directiva en el siguiente comando:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

De forma similar, puede usar `Enable-HgsAttestationPolicy` para volver a habilitar una directiva.

Si ya no necesita una directiva y desea quitarlo de todos los nodos HGS, ejecute `Remove-HgsAttestationPolicy -Name 'PolicyName'` para eliminar permanentemente la directiva.

## <a name="changing-attestation-modes"></a>Cambiar los modos de atestación
Si ha comenzado al tejido protegido con la atestación de administrador de confianza, probablemente deseará actualizar al modo de atestación de TPM mucho mayor en cuanto tiene suficientes hosts 2.0 compatible TPM en su entorno.
Cuando esté listo para cambiar, puede cargar previamente todos los artefactos de atestación de HGS (directivas de CI, líneas de base de TPM y los identificadores TPM) mientras se sigue ejecutando HGS con la atestación de administrador de confianza.
Para ello, basta con que siga las instrucciones de la [autorizar a un host protegido nuevo](#authorizing-new-guarded-hosts) sección.

Una vez haya agregado todas sus directivas a HGS, el siguiente paso es ejecutar un intento de atestación sintético en los hosts para ver si desea pasar la atestación en modo TPM.
Esto no afecta el estado operativo actual de HGS.
Los siguientes comandos se deben ejecutar en un equipo que tenga acceso a todos los hosts en el entorno y al menos un nodo HGS.
Si el firewall u otras directivas de seguridad evitarlo, puede omitir este paso.
Cuando sea posible, se recomienda ejecutar la atestación sintética para ofrecerle una buena indicación de si "volteo" al modo TPM provocará tiempo de inactividad para las máquinas virtuales. 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

Después de los diagnósticos completados, revise la información de archivos para determinar si todos los hosts habría producido un error de atestación en modo TPM.
Vuelva a ejecutar el diagnóstico hasta obtener un "pass" de cada host y luego continúe con HGS de cambiar al modo TPM.

**Cambiar al modo TPM** tarda un segundo en completarse.
Ejecute el siguiente comando en cualquier nodo HGS para actualizar el modo de atestación.

```powershell
Set-HgsServer -TrustTpm
```

Si experimenta problemas y necesita volver a cambiar al modo de Active Directory, puede hacerlo mediante la ejecución de `Set-HgsServer -TrustActiveDirectory`.

Una vez que haya confirmado que todo funciona según lo previsto, debe quitar todos los grupos de host de Active Directory confianza de HGS y quitar la confianza entre los dominios HGS y el tejido.
Si deja la confianza de Active Directory en su lugar, se arriesga a que alguien volver a habilitar la confianza y HGS el cambio al modo de Active Directory, lo que podría permitir código no seguro para ejecutarse sin comprobar en los hosts protegidos.

## <a name="key-management"></a>Administración de claves
La solución de tejido protegido utiliza varios pares de claves pública y privada para validar la integridad de los distintos componentes de la solución y cifrar los secretos de inquilino.
El servicio de protección de Host está configurado con al menos dos certificados (con claves públicas y privadas), que se usan para firmar y cifrar las claves utilizadas para iniciar las máquinas virtuales blindadas.
Dichas claves se deben administrar con cuidado.
Si la clave privada se adquiere un adversario, podrán unshield todas las máquinas virtuales que se ejecutan en el tejido o configurar un clúster HGS impostor que usa directivas de atestación más débiles para eludir las protecciones que coloque en su lugar.
Debe perder las claves privadas durante un desastre y no encontrarlos en una copia de seguridad, deberá configurar un nuevo par de claves y tiene cada máquina virtual vuelve a organizados para autorizar a los certificados nuevos.

Esta sección tratan temas generales de administración de claves para ayudarle a configurar las claves para que sean seguras y funcional.

### <a name="adding-new-keys"></a>Agregar nuevas claves
Mientras HGS se debe inicializar con un conjunto de claves, puede agregar más de un cifrado y firma de clave de HGS.
Las dos razones más comunes, ¿por qué podría agregar nuevas claves a HGS son:
1. Para admitir escenarios "bring your own key" donde los inquilinos copiar sus claves privadas en el módulo de seguridad de hardware y autorizar solamente a sus claves para iniciar sus máquinas virtuales blindadas.
2. Para reemplazar las claves existentes para HGS por primera vez que agregue las nuevas claves y mantener dos conjuntos de claves hasta que cada máquina virtual la configuración se actualizó para usar las nuevas claves.

El proceso para agregar las nuevas claves varía según el tipo de certificado que usa.

**Opción 1: Agregar un certificado almacenado en un HSM**

Nuestro enfoque recomendado para proteger las claves HGS es usar certificados creados en un módulo de seguridad de hardware (HSM).
Los HSM Asegúrese de que está asociado el uso de las claves para el acceso físico a un dispositivo de seguridad del centro de datos.
Cada HSM es diferente y tiene un único proceso para crear certificados y registrarlas con HGS.
Los pasos siguientes están pensados para proporcionar instrucciones generales para usar HSM certificados de seguridad.
Consulte la documentación de su proveedor HSM para conocer los pasos exactos y capacidades.

1. Instalar el software HSM en cada nodo HGS en el clúster. Dependiendo de si tiene una red o un dispositivo HSM local, debe configurar para conceder acceso a la máquina a su almacén de claves HSM.
2. Cree 2 certificados en el HSM con **claves RSA de 2048 bits** para cifrado y firma
    1. Crear un certificado de cifrado con la **cifrado de datos** propiedad de uso en el HSM clave
    2. Crear un certificado de firma con el **firma Digital** propiedad de uso en el HSM clave
3. Instalar los certificados en el almacén de certificados local de cada nodo HGS siguiendo las instrucciones de su proveedor HSM.
4. Si el HSM usa permisos granulares para conceder a aplicaciones específicas o los usuarios permiso para usar la clave privada, deberá conceder el acceso de cuenta de servicio administrada de grupo HGS al certificado. Puede encontrar el nombre de la cuenta de gMSA HGS mediante la ejecución `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Agregar los certificados de firma y cifrado a HGS reemplazando las huellas digitales con las de los certificados en los siguientes comandos:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opción 2: Adición de certificados de software no exportable**

Si tiene un certificado de software de seguridad emitido por su empresa o una entidad de certificación pública que tenga una clave privada no exportable necesitará agregar su certificado a HGS mediante su huella digital.
1. Instale el certificado en el equipo de acuerdo con las instrucciones de la entidad emisora de certificados.
2. Conceda el HGS administradas de grupo servicio cuenta acceso de lectura a la clave privada del certificado. Puede encontrar el nombre de la cuenta de gMSA HGS mediante la ejecución `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registrar el certificado con HGS mediante el siguiente comando y sustituyendo en huella digital del certificado (cambiar *cifrado* a *firma* para certificados de firma):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Deberá instalar la clave privada y conceder acceso de lectura a la cuenta de gMSA en cada nodo HGS manualmente.
> HGS no puede replicar automáticamente las claves privadas *cualquier* registrado mediante su huella digital del certificado.

**Opción 3: Agregar los certificados almacenados en archivos PFX**

Si tiene un certificado basado en software con una clave privada exportable que se puede almacenar en el formato de archivo PFX y protegido con una contraseña, HGS puede administrar los certificados automáticamente para usted.
Certificados agregados con archivos PFX se replican automáticamente en todos los nodos del clúster HGS y HGS protege el acceso a las claves privadas.
Para agregar un certificado nuevo con un archivo PFX, ejecute los siguientes comandos en cualquier nodo HGS (cambiar *cifrado* a *firma* para certificados de firma):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificar y cambiar los certificados principales** mientras HGS puede admitir varios certificados de firma y cifrado, usa un par que sus certificados "principales".
Estos son los certificados que se usará si alguien descarga los metadatos del guardián para ese clúster HGS.
Para comprobar los certificados que están marcados actualmente como los certificados principales, ejecute el siguiente comando:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Para establecer un nuevo primario de cifrado o el certificado de firma, encontrar la huella digital del certificado deseado y márquelo como principal con los siguientes comandos:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renovar o reemplazar las claves
Al crear los certificados usados por HGS, los certificados se asignará una fecha de caducidad según la directiva de la entidad emisora de certificados y la información de la solicitud.
Normalmente, en escenarios donde la validez del certificado es importante, como asegurar las comunicaciones HTTP, los certificados se deben renovar antes de que caduquen para evitar una interrupción del servicio o un mensaje de error preocupante.
HGS no utiliza certificados en ese sentido.
HGS simplemente usa certificados como una manera cómoda de crear y almacenar un par de claves asimétricas.
Un cifrado caducado o certificado de firma en HGS no indica una debilidad o una pérdida de protección para máquinas virtuales blindadas.
Además, no se realizan comprobaciones de revocación de certificados por HGS.
Si se revoca un certificado HGS o la entidad de certificación emisora, no afectará a uso HGS del certificado.

La única vez que tiene que preocuparse sobre un certificado HGS es si tiene motivos para creer que tiene su clave privada se ha robado.
En ese caso, la integridad de las máquinas virtuales blindadas está en riesgo porque la posesión de la mitad privada del par de claves de firma y el cifrado de HGS es suficiente para quitar las protecciones de blindaje en una máquina virtual o suspensión de un servidor HGS falso que tiene directivas más débiles de atestación.

Si se encuentra en esa situación, o son necesarios para actualizar periódicamente las claves de certificado por los estándares de cumplimiento, los siguientes pasos describen el proceso para cambiar las claves en un servidor HGS.
Tenga en cuenta que la guía siguiente representa un reto importante que provocará una interrupción del servicio en cada máquina virtual atendida por el clúster HGS.
Se requiere la planeación adecuada para cambiar las claves HGS para minimizar la interrupción del servicio y garantizar la seguridad de máquinas virtuales de inquilino.

En un nodo HGS, realice los pasos siguientes para registrar un nuevo par de cifrado y firma certificados.
Consulte la sección sobre [agregar nuevas claves](#adding-new-keys) para obtener información detallada las diversas maneras de agregar nuevas claves a HGS.
1. Cree un nuevo par de cifrado y certificados de firma para el servidor HGS. Idealmente, estos se crearán en un módulo de seguridad de hardware.
2. Registrar el nuevo cifrado y firma de certificados con **agregar HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Si ha usado las huellas digitales, deberá ir a cada nodo del clúster para instalar la clave privada y conceder el acceso de gMSA HGS a la clave.
4. Asegúrese de los certificados de forma predeterminada los nuevos certificados en HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

En este momento, los datos creados con metadatos obtenidos desde el nodo HGS de blindaje usará los nuevos certificados, pero las máquinas virtuales existentes seguirán funcionando dado que los certificados anteriores siguen estando disponibles.
Para asegurarse de que todas las máquinas virtuales existentes funcionará con las nuevas claves, deberá actualizar el protector de clave en cada máquina virtual.
Se trata de una acción que requiere el propietario de la máquina virtual (persona o entidad en posesión de la protección de "propietario") para participar.
Para cada máquina virtual blindada, realice los pasos siguientes:
5. Apague la máquina virtual. La máquina virtual no se puede volver a activarse hasta que finalizan los pasos restantes, o bien deberá iniciar el proceso de nuevo.
6. Guardar el protector de clave actual en un archivo: `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Transferir la KP al propietario de la máquina virtual
8. Tiene la descarga de propietario la información actualizada de protección de HGS e impórtelo en su sistema local
9. Lee el KP actual en la memoria, conceder acceso de la nueva protección a lo KP y guárdelo en un archivo nuevo ejecutando los comandos siguientes:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copia el KP actualizada en el tejido de hospedaje
11. Aplicar el KP a la máquina virtual original:

    ```powershell
    $updatedKP = Get-Content -Path .\updatedVM001.kp
    Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
    ```
12. Por último, inicie la máquina virtual y asegúrese de que se ejecuta correctamente.

> [!NOTE]
> Si el propietario de la máquina virtual establece un protector de clave incorrecto en la máquina virtual y no autorizar su tejido para ejecutar la máquina virtual, podrá iniciar la máquina virtual blindada.
> Para volver a la última protector de clave buena conocido, ejecutar `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Una vez que se han actualizado todas las máquinas virtuales para autorizar a las nuevas claves de protección, puede deshabilitar y quitar las claves antiguas.

13. Obtener las huellas digitales de los certificados antiguos de `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Deshabilitar cada certificado ejecutando los comandos siguientes:  

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
    ```

15. Después de asegurarse de que las máquinas virtuales pueden seguir para empezar con los certificados deshabilitados, quitar los certificados de HGS ejecutando los comandos siguientes:

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> Las copias de seguridad de máquina virtual contendrá información antigua de protector de clave que permiten a los certificados antiguos que se usará para iniciar la máquina virtual.
> Si es consciente de que se ha puesto en peligro la clave privada, debe suponer que las copias de seguridad de máquina virtual pueden estar en peligro, demasiado y tomar las medidas adecuadas.
> Destruir la configuración de máquina virtual desde las copias de seguridad (.vmcx) se quitarán los protectores de clave, pero a costa de tener que usar la contraseña de recuperación de BitLocker para arrancar la máquina virtual en la próxima vez.

### <a name="key-replication-between-nodes"></a>Replicación clave entre nodos
Todos los nodos del clúster de HGS deben configurarse con el mismo cifrado, firma y (cuando se configura) certificados SSL.
Esto es necesario para asegurarse de ponerse en contacto con cualquier nodo del clúster de hosts de Hyper-V pueden tener sus solicitudes atendidas correctamente.

**Si inicializa servidor HGS con los certificados PFX** , a continuación, HGS replicarán automáticamente de la clave pública y privada de dichos certificados en todos los nodos del clúster.
Sólo necesitará agregar las claves en un nodo.

**Si inicializa servidor HGS con referencias del certificado** o huellas digitales y, a continuación, HGS solo se replicará la *pública* clave en el certificado a cada nodo.
Además, no se puede conceder HGS propio acceso a la clave privada en cualquier nodo en este escenario.
Por lo tanto, es responsabilidad suya:
1. Instale la clave privada en cada nodo HGS
2. Conceder el acceso de cuenta (gMSA) HGS administradas de grupo service a la clave privada en cada nodo de que estas tareas agregan carga extra operacional, sin embargo, son necesarios para claves respaldadas con HSM y los certificados con claves privadas que no es exportable.

**Certificados SSL** nunca se replican de forma alguna.
Es su responsabilidad para inicializar cada servidor HGS con el mismo certificado SSL y actualizar cada servidor cada vez que decida renovar o reemplazar el certificado SSL.
Cuando se reemplaza el certificado SSL, se recomienda que hacerlo mediante el [conjunto HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet.

## <a name="unconfiguring-hgs"></a>Quitar la configuración de HGS

Si necesita dar de baja o significativamente volver a configurar un servidor HGS, puede hacerlo mediante el [Clear HgsServer](https://technet.microsoft.com/library/mt652176.aspx) o [desinstalar HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlets.

### <a name="clearing-the-hgs-configuration"></a>Borrar la configuración de HGS

Para quitar un nodo del clúster HGS, use el [Clear HgsServer](https://technet.microsoft.com/library/mt652176.aspx) cmdlet.
Este cmdlet realizará los siguientes cambios en el servidor donde se ejecuta:

- Anula el registro de los servicios de protección de atestación y la clave
- Quita el punto de conexión de administración de JEA "microsoft.windows.hgs"
- Quita el equipo local desde el clúster de conmutación por error HGS

Si el servidor es el último nodo HGS en el clúster, también se destruirá el clúster y su correspondiente recurso de nombre de red distribuida.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Una vez finalizada la operación de borrado, se puede volver a inicializar con el servidor HGS [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Si ha usado [Install HgsServer](https://technet.microsoft.com/library/mt652169.aspx) para configurar un dominio de Active Directory Domain Services, ese dominio permanecerá configurada y funciona después de la operación de borrado.

### <a name="uninstalling-hgs"></a>Desinstalación de HGS

Si desea quitar un nodo del clúster HGS **y** degradar el controlador de dominio de Active Directory que se ejecutan en él, utilice el [desinstalar HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet.
Este cmdlet realizará los siguientes cambios en el servidor donde se ejecuta:

- Anula el registro de los servicios de protección de atestación y la clave
- Quita el punto de conexión de administración de JEA "microsoft.windows.hgs"
- Quita el equipo local desde el clúster de conmutación por error HGS
- Degrada el controlador de dominio de Active Directory, si ha configurado

Si el servidor es el último nodo HGS en el clúster, el dominio, el clúster de conmutación por error, y también se destruirá el recurso de nombre de red distribuida del clúster.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Una vez finalizada la operación de desinstalación y se ha reiniciado el equipo, puede volver a instalar ADDC y HGS mediante [Install HgsServer](https://technet.microsoft.com/library/mt652169.aspx) o unir el equipo a un dominio e inicializar el servidor HGS en ese dominio con [ Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx).

Si ya no desea usar el equipo como un nodo HGS, puede quitar el rol de Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
