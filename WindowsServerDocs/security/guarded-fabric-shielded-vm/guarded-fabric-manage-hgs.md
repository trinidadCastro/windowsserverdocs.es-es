---
title: Administración del servicio de protección de host
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.openlocfilehash: 851ea4a57068c1544f290c48f370e04b96857cf6
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989161"
---
# <a name="managing-the-host-guardian-service"></a>Administración del servicio de protección de host

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

El servicio de protección de host (HGS) es el centro de la solución de tejido protegido.
Es responsable de garantizar que los hosts de Hyper-V en el tejido sean conocidos por el anfitrión o la empresa y que ejecuten software de confianza y para administrar las claves usadas para iniciar máquinas virtuales blindadas.
Cuando un inquilino decide confiar en que hospede sus máquinas virtuales blindadas, están colocando su confianza en la configuración y la administración del servicio de protección de host.
Por lo tanto, es muy importante seguir las prácticas recomendadas a la hora de administrar el servicio de protección de host para garantizar la seguridad, la disponibilidad y la confiabilidad del tejido protegido.
En las instrucciones de las secciones siguientes se abordan los problemas operativos más comunes para los administradores de HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitar el acceso de administrador a HGS
Debido a la naturaleza sensible a la seguridad del HGS, es importante asegurarse de que los administradores son miembros de alta confianza de la organización y, idealmente, independientes de los administradores de los recursos del tejido.
Además, se recomienda administrar HGS desde estaciones de trabajo seguras mediante protocolos de comunicación seguros, como WinRM a través de HTTPS.

### <a name="separation-of-duties"></a>Separación de obligaciones
Al configurar HGS, se le ofrece la opción de crear un bosque aislado Active Directory solo para HGS o para unir HGS a un dominio de confianza existente.
Esta decisión, así como los roles que se asignan a los administradores de la organización, determinan el límite de confianza para HGS.
Quien tiene acceso a HGS, ya sea directamente como administrador o indirectamente como administrador de otra cosa (por ejemplo, Active Directory) que puede influir en HGS, tiene el control sobre el tejido protegido.
Los administradores de HGS eligen qué hosts de Hyper-V están autorizados para ejecutar máquinas virtuales blindadas y administran los certificados necesarios para iniciar máquinas virtuales blindadas.
Un atacante o un administrador malintencionado con acceso a HGS puede usar esta capacidad para autorizar a los hosts comprometidos a ejecutar máquinas virtuales blindadas, iniciar un ataque por denegación de servicio quitando el material de clave y mucho más.

Para evitar este riesgo, se recomienda *encarecidamente* limitar la superposición entre los administradores de su HGS (incluido el dominio al que se unen HGS) y los entornos de Hyper-V.
Al asegurarse de que ningún administrador tenga acceso a ambos sistemas, un atacante tendría que poner en peligro 2 cuentas diferentes de 2 personas para completar su misión de cambiar las directivas de HGS.
Esto también significa que los administradores de dominio y de empresa para los dos entornos de Active Directory no deben ser la misma persona, ni deben usar el mismo bosque Active Directory que los hosts de Hyper-V.
Cualquier persona que pueda conceder acceso a más recursos supone un riesgo para la seguridad.

### <a name="using-just-enough-administration"></a>Usar solo una administración suficiente
HGS incluye solo roles de [Administración](https://aka.ms/JEAdocs) (jea) integrados para ayudarle a administrarlo de forma más segura.
JEA ayuda a delegar tareas de administración a usuarios que no son administradores, lo que significa que las personas que administran las directivas HGS no necesitan ser administradores de todo el equipo o dominio.
JEA funciona limitando qué comandos puede ejecutar un usuario en una sesión de PowerShell y usando una cuenta local temporal en segundo plano (única para cada sesión de usuario) para ejecutar los comandos que normalmente requieren elevación.

HGS se suministra con 2 roles de JEA preconfigurados:
- **Administradores de HGS** que permiten a los usuarios administrar todas las directivas de HGS, incluida la autorización de nuevos hosts para ejecutar máquinas virtuales blindadas.
- **Revisores de HGS** que solo permiten a los usuarios realizar auditorías de las directivas existentes. No pueden realizar cambios en la configuración de HGS.

Para usar JEA, primero debe crear un nuevo usuario estándar y convertirlos en miembros del grupo de administradores de HGS o de revisores de HGS.
Si usó `Install-HgsServer` para configurar un nuevo bosque para HGS, estos grupos se denominarán "*ServiceName*Administrators" y "*ServiceName*revisores", respectivamente, donde *ServiceName* es el nombre de red del clúster de HGS.
Si ha unido HGS a un dominio existente, debe hacer referencia a los nombres de grupo especificados en `Initialize-HgsServer` .

**Crear usuarios estándar para los roles de administrador de HGS y revisor**

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

En un equipo remoto que tenga conectividad de red a HGS, ejecute los siguientes comandos en PowerShell para especificar la sesión de JEA con las credenciales del revisor.
Es importante tener en cuenta que, puesto que la cuenta de revisor es simplemente un usuario estándar, no se puede usar para la comunicación remota de Windows PowerShell normal, Escritorio remoto el acceso a HGS, etc.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

A continuación, puede comprobar qué comandos están permitidos en la sesión `Get-Command` y ejecutar cualquier comando permitido para auditar la configuración.
En el ejemplo siguiente, se está comprobando qué directivas están habilitadas en HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Escriba el comando `Exit-PSSession` o su alias, `exit` cuando haya terminado de trabajar con la sesión de jea.

**Agregar una nueva Directiva a HGS mediante el rol de administrador**

Para cambiar una directiva realmente, debe conectarse al punto de conexión JEA con una identidad que pertenezca al grupo ' hgsAdministrators '.
En el ejemplo siguiente, se muestra cómo se puede copiar una nueva Directiva de integridad de código en HGS y registrarla mediante JEA.
La sintaxis puede ser diferente de la que se usa.
Esto es para dar cabida a algunas de las restricciones de JEA como no tener acceso al sistema de archivos completo.

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
### <a name="event-sources-and-forwarding"></a>Orígenes y reenvío de eventos
Los eventos de HGS se mostrarán en el registro de eventos de Windows en 2 orígenes:
- **La atestación de HostGuardianService**
- **HostGuardianService-KeyProtection**

Puede ver estos eventos abriendo Visor de eventos y navegando a Microsoft-Windows-HostGuardianService-Atestation y Microsoft-Windows-HostGuardianService-KeyProtection.

En un entorno de gran tamaño, a menudo es preferible reenviar eventos a un recopilador de eventos central de Windows para facilitar la análisis de los eventos.
Para obtener más información, consulte la [documentación sobre el reenvío de eventos de Windows](/windows/win32/wec/windows-event-collector).

### <a name="using-system-center-operations-manager"></a>Usar System Center Operations Manager
También puede usar System Center 2016-Operations Manager para supervisar HGS y los hosts protegidos.
El módulo de administración del tejido protegido tiene monitores de eventos para comprobar las configuraciones incorrectas comunes que pueden provocar tiempos de inactividad en el centro de recursos, incluidos los hosts que no pasan la atestación y los servidores HGS informan de errores.

Para empezar, [Instale y configure SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) y [Descargue el módulo de administración del tejido protegido](https://www.microsoft.com/download/details.aspx?id=52764).
La guía del módulo de administración incluido explica cómo configurar el módulo de administración y comprender el ámbito de sus monitores.

## <a name="backing-up-and-restoring-hgs"></a>Copia de seguridad y restauración de HGS
### <a name="disaster-recovery-planning"></a>Planeamiento de recuperación ante desastres
Al crear un borrador de los planes de recuperación ante desastres, es importante tener en cuenta los requisitos únicos del servicio de protección de host en el tejido protegido.
Si pierde algunos o todos los nodos de HGS, puede que se encuentre con problemas de disponibilidad inmediata que impedirán que los usuarios inicien sus máquinas virtuales blindadas.
En un escenario en el que pierde todo el clúster de HGS, tendrá que realizar copias de seguridad completas de la configuración de HGS a mano para restaurar el clúster de HGS y reanudar las operaciones normales.
En esta sección se describen los pasos necesarios para preparar este escenario.

En primer lugar, es importante comprender lo que es importante para HGS para realizar copias de seguridad.
HGS conserva varios fragmentos de información que ayudan a determinar qué hosts están autorizados a ejecutar máquinas virtuales blindadas.
Esto incluye:
1. Active Directory los identificadores de seguridad de los grupos que contienen hosts de confianza (cuando se usa la atestación de Active Directory).
2. Identificadores de TPM únicos para cada host del entorno;
3. Directivas de TPM para cada configuración única de host; etc
4. Directivas de integridad de código que determinan qué software se puede ejecutar en los hosts.

Estos artefactos de atestación requieren la coordinación con los administradores de su tejido de hospedaje para obtener, lo que puede dificultar la obtención de esta información después de un desastre.

Además, HGS requiere acceso a 2 o más certificados usados para cifrar y firmar la información necesaria para iniciar una máquina virtual blindada (el protector de clave).
Estos certificados son muy conocidos (usados por los propietarios de las máquinas virtuales blindadas para autorizar a su tejido a ejecutar sus máquinas virtuales) y deben restaurarse después de un desastre para una experiencia de recuperación sin problemas.
Si no restaura HGS con los mismos certificados después de un desastre, cada máquina virtual deberá actualizarse para autorizar a las nuevas claves a descifrar su información.
Por motivos de seguridad, solo el propietario de la máquina virtual puede actualizar la configuración de la máquina virtual para autorizar estas nuevas claves, lo que significa que si se produce un error al restaurar las claves después de un desastre, cada propietario de la máquina virtual debe tomar medidas para volver a ejecutar sus máquinas virtuales.

#### <a name="preparing-for-the-worst"></a>Preparación para el peor
Para prepararse para una pérdida completa de HGS, debe realizar dos pasos:
1. Copia de seguridad de las directivas de atestación de HGS
2. Copia de seguridad de las claves HGS

En la sección [copia de seguridad de HGS](#backing-up-hgs) se proporcionan instrucciones sobre cómo realizar estos dos pasos.

Además, se recomienda, pero no es necesario, que realice una copia de seguridad de la lista de usuarios autorizados para administrar HGS en su Active Directory dominio o Active Directory.

Las copias de seguridad deben realizarse con regularidad para garantizar que la información esté actualizada y se almacene de forma segura para evitar alteraciones o robos.

No se **recomienda** realizar una copia de seguridad de una imagen de sistema completa de un nodo HGS o intentar restaurarla.
En caso de que haya perdido todo el clúster, el procedimiento recomendado es configurar un nodo de HGS nuevo y restaurar solo el estado del HGS, no el sistema operativo del servidor completo.

#### <a name="recovering-from-the-loss-of-one-node"></a>Recuperación de la pérdida de un nodo
Si pierde uno o varios nodos (pero no todos) en el clúster de HGS, puede simplemente [agregar nodos al clúster](guarded-fabric-configure-additional-hgs-nodes.md) siguiendo las instrucciones de la guía de implementación.
Las directivas de atestación se sincronizarán automáticamente, como los certificados que se proporcionaron a HGS como archivos PFX con contraseñas adjuntas.
En el caso de los certificados agregados al HGS mediante una huella digital (certificados respaldados por hardware y no exportables, normalmente), deberá asegurarse de que cada nuevo nodo tenga acceso a la clave privada de cada certificado.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Recuperación de la pérdida de todo el clúster
Si todo el clúster de HGS deja de funcionar y no puede volver a ponerlo en línea, tendrá que restaurar HGS a partir de una copia de seguridad.
La restauración de HGS a partir de una copia de seguridad implica configurar primero un nuevo clúster de HGS según las [instrucciones de la guía de implementación](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
Se recomienda, pero no es necesario, usar el mismo nombre de clúster al configurar el entorno de HGS de recuperación para ayudar a la resolución de nombres de hosts.
El uso del mismo nombre evita tener que volver a configurar los hosts con nuevas direcciones URL de atestación y protección de claves.
Si restauró objetos en el HGS de respaldo del dominio de Active Directory, se recomienda que quite los objetos que representan el clúster de HGS, los equipos, la cuenta de servicio y los grupos de JEA antes de inicializar el servidor de HGS.

Una vez que haya configurado el primer nodo de HGS (por ejemplo, que se haya instalado e inicializado), siga los procedimientos que se describen en [restauración de HGS desde una copia de seguridad](#restoring-hgs-from-a-backup) para restaurar las directivas de atestación y la mitad pública de los certificados de protección de claves.
Tendrá que restaurar las claves privadas de los certificados manualmente de acuerdo con las instrucciones de su proveedor de certificados (por ejemplo, importar el certificado en Windows o configurar el acceso a los certificados con respaldo de HSM).
Después de configurar el primer nodo, puede seguir [instalando nodos adicionales en el clúster](guarded-fabric-configure-additional-hgs-nodes.md) hasta que haya alcanzado la capacidad y la resistencia que desee.

### <a name="backing-up-hgs"></a>Copia de seguridad de HGS
El administrador de HGS debe ser responsable de realizar copias de seguridad de HGS de forma periódica.
Una copia de seguridad completa contendrá material de clave confidencial que se debe proteger adecuadamente.
Si una entidad que no es de confianza obtiene acceso a estas claves, podría usar ese material para configurar un entorno de HGS malintencionado con el fin de poner en peligro máquinas virtuales blindadas.

**Copia de seguridad de las directivas de atestación** Para realizar una copia de seguridad de las directivas de atestación de HGS, ejecute el siguiente comando en cualquier nodo de servidor de HGS en funcionamiento.
Se le pedirá que proporcione una contraseña.
Esta contraseña se usa para cifrar los certificados agregados al HGS mediante un archivo PFX (en lugar de una huella digital de certificado).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Si usa la atestación de confianza de administrador, debe realizar una copia de seguridad por separado de los grupos de seguridad que usa HGS para autorizar los hosts protegidos.
> HGS solo realizará una copia de seguridad del SID de los grupos de seguridad, no de la pertenencia a ellos.
> En caso de que estos grupos se pierdan durante un desastre, tendrá que volver a crear los grupos y agregar cada uno de ellos.

**Copia de seguridad de certificados**

El `Export-HgsServerState` comando hará una copia de seguridad de los certificados basados en pfx agregados a HGS en el momento en que se ejecuta el comando.
Si agregó certificados a HGS mediante una huella digital (típica para certificados no exportables y respaldados por hardware), tendrá que realizar manualmente una copia de seguridad de las claves privadas de los certificados.
Para identificar qué certificados están registrados en HGS y deben realizarse manualmente, ejecute el siguiente comando de PowerShell en cualquier nodo de servidor de HGS en funcionamiento.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Para cada uno de los certificados que se muestran, deberá realizar una copia de seguridad manual de la clave privada.
Si usa un certificado basado en software que no es exportable, debe ponerse en contacto con la entidad de certificación para asegurarse de que tiene una copia de seguridad del certificado o puede volver a emitirlo a petición.
En el caso de los certificados creados y almacenados en módulos de seguridad de hardware, debe consultar la documentación del dispositivo para obtener instrucciones sobre el planeamiento de la recuperación ante desastres.

Debe almacenar las copias de seguridad del certificado junto con las copias de seguridad de la Directiva de atestación en una ubicación segura para que ambas partes se puedan restaurar juntas.

**Configuración adicional para realizar copias de seguridad**

La copia de seguridad del estado del servidor HGS no incluirá el nombre del clúster de HGS, ninguna información de Active Directory o cualquier certificado SSL usado para proteger las comunicaciones con las API de HGS.
Esta configuración es importante para la coherencia pero no es crítica para volver a poner en línea el clúster de HGS después de un desastre.

Para capturar el nombre del servicio HGS, ejecute `Get-HgsServer` y anote el nombre plano en las direcciones URL de atestación y protección de claves.
Por ejemplo, si la dirección URL de atestación es " <http://hgs.contoso.com/Attestation> ", "HGS" es el nombre del servicio HGS.

El dominio Active Directory que usa HGS debe administrarse como cualquier otro dominio Active Directory.
Al restaurar HGS después de un desastre, no tendrá que volver a crear los objetos exactos que se encuentran en el dominio actual.
Sin embargo, facilitará la recuperación si realiza una copia de seguridad de Active Directory y mantiene una lista de los usuarios de JEA autorizados para administrar el sistema, así como la pertenencia de los grupos de seguridad que usa la atestación de confianza de administrador para autorizar hosts protegidos.

Para identificar la huella digital de los certificados SSL configurados para HGS, ejecute el siguiente comando en PowerShell.
Después, puede hacer una copia de seguridad de esos certificados SSL según las instrucciones del proveedor de certificados.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Restauración de HGS a partir de una copia de seguridad
En los pasos siguientes se describe cómo restaurar la configuración de HGS desde una copia de seguridad.
Los pasos son relevantes para las dos situaciones en las que está intentando deshacer los cambios realizados en las instancias de HGS que ya están en ejecución y cuando se pone al día un nuevo clúster de HGS después de una pérdida completa de la anterior.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configuración de un clúster de HGS de reemplazo
Antes de poder restaurar HGS, debe tener un clúster de HGS inicializado en el que pueda restaurar la configuración.
Si simplemente está importando la configuración que se eliminó accidentalmente a un clúster existente (en ejecución), puede omitir este paso.
Si va a realizar la recuperación a partir de una pérdida completa de HGS, deberá instalar e inicializar al menos un nodo de HGS siguiendo las [instrucciones de la guía de implementación](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

En concreto, tendrá que:
1. [Configuración del dominio HGS](guarded-fabric-choose-where-to-install-hgs.md) o unión de HGS a un dominio existente
2. [Inicialice el servidor de HGS](guarded-fabric-initialize-hgs.md) con las claves existentes *o* un conjunto de claves temporales. Puede [quitar las claves temporales](#renewing-or-replacing-keys) después de importar las claves reales de los archivos de copia de seguridad de HGS.
3. [Importar la configuración de HGS](#import-settings-from-a-backup) desde la copia de seguridad para restaurar los grupos de hosts de confianza, las directivas de integridad de código, las líneas de base de TPM e identificadores de TPM

> [!TIP]
> El nuevo clúster de HGS no necesita usar los mismos certificados, el mismo nombre de servicio o el mismo dominio que la instancia de HGS desde la que se exportó el archivo de copia de seguridad.

#### <a name="import-settings-from-a-backup"></a>Importar la configuración de una copia de seguridad

Para restaurar las directivas de atestación, los certificados basados en PFX y las claves públicas de los certificados que no son PFX en el nodo HGS desde un archivo de copia de seguridad, ejecute el siguiente comando en un nodo de servidor HGS inicializado.
Se le pedirá que escriba la contraseña que especificó al crear la copia de seguridad.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Si solo desea importar directivas de atestación de confianza de administrador o directivas de atestación de confianza de TPM, puede hacerlo especificando las `-ImportActiveDirectoryModeState` marcas o `-ImportTpmModeState` para [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Asegúrese de que la actualización acumulativa más reciente para Windows Server 2016 está instalada antes de ejecutarse `Import-HgsServerState` .
Si no lo hace, puede producirse un error de importación.

> [!NOTE]
> Si restaura las directivas en un nodo de HGS existente que ya tiene una o varias de esas directivas instaladas, el comando de importación mostrará un error para cada directiva duplicada.
> Este es un comportamiento esperado y se puede omitir de forma segura en la mayoría de los casos.

#### <a name="reinstall-private-keys-for-certificates"></a>Reinstalar claves privadas para certificados
Si alguno de los certificados usados en el HGS desde el que se creó la copia de seguridad se agregó mediante huellas digitales, solo se incluirá en el archivo de copia de seguridad la clave pública de dichos certificados.
Esto significa que tendrá que instalar manualmente o conceder acceso a las claves privadas de cada uno de esos certificados para que HGS pueda atender las solicitudes de los hosts de Hyper-V.
Las acciones necesarias para completar ese paso varían en función de cómo se haya emitido originalmente el certificado.
En el caso de los certificados respaldados por software emitidos por una entidad de certificación, deberá ponerse en contacto con la entidad de certificación para obtener la clave privada e instalarla en **cada** nodo de HGS según sus instrucciones.
Del mismo modo, si los certificados están respaldados por hardware, deberá consultar la documentación del proveedor del módulo de seguridad de hardware para instalar los controladores necesarios en cada nodo de HGS para conectarse al HSM y conceder a cada equipo acceso a la clave privada.

Como recordatorio, los certificados agregados al HGS mediante huellas digitales requieren la replicación manual de las claves privadas en cada nodo.
Tendrá que repetir este paso en cada nodo adicional que agregue al clúster de HGS restaurado.

#### <a name="review-imported-attestation-policies"></a>Revisar las directivas de atestación importadas
Después de haber importado la configuración de una copia de seguridad, se recomienda revisar atentamente todas las directivas importadas mediante para asegurarse de que `Get-HgsAttestationPolicy` solo los hosts en los que confía pueden ejecutar máquinas virtuales blindadas podrán confirmar correctamente.
Si encuentra directivas que ya no coinciden con su postura de seguridad, puede [deshabilitarlas o quitarlas](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Ejecutar diagnósticos para comprobar el estado del sistema
Una vez que haya terminado de configurar y restaurar el estado del nodo de HGS, debe ejecutar la herramienta de diagnóstico de HGS para comprobar el estado del sistema.
Para ello, ejecute el siguiente comando en el nodo HGS donde restauró la configuración:

```powershell
Get-HgsTrace -RunDiagnostics
```

Si el "resultado total" no es "pass", se requieren pasos adicionales para finalizar la configuración del sistema.
Compruebe los mensajes informados en las subpruebas que dieron error para obtener más información.

## <a name="patching-hgs"></a>Aplicar revisiones a HGS
Es importante mantener actualizados los nodos del servicio de protección de host mediante la instalación de la actualización acumulativa más reciente cuando salga. Si está configurando un nuevo nodo HGS, se recomienda encarecidamente que instale las actualizaciones disponibles antes de instalar el rol HGS o la configuración.
Esto garantizará que cualquier funcionalidad nueva o modificada se aplicará inmediatamente.

Al aplicar una revisión al tejido protegido, se recomienda encarecidamente que actualice primero *todos los* hosts de Hyper-V **antes de actualizar HGS**.
Esto se hace para asegurarse de que los cambios en las directivas de atestación en HGS se realicen *una vez* que se hayan actualizado los hosts de Hyper-V para proporcionar la información necesaria para ellos.
Si una actualización va a cambiar el comportamiento de las directivas, no se habilitarán automáticamente para evitar interrumpir el tejido.
Estas actualizaciones requieren que siga las instrucciones de la sección siguiente para activar las directivas de atestación nuevas o modificadas.
Le recomendamos que lea las notas de la versión de Windows Server y las actualizaciones acumulativas que instale para comprobar si se requieren actualizaciones de directivas.

### <a name="updates-requiring-policy-activation"></a>Actualizaciones que requieren activación de Directiva
Si una actualización para HGS introduce o cambia significativamente el comportamiento de una directiva de atestación, es necesario un paso adicional para activar la directiva modificada.
Los cambios de directiva solo se implementan después de exportar e importar el estado de HGS.
Solo debe activar las directivas nuevas o modificadas después de aplicar la actualización acumulativa a todos los hosts y todos los nodos de HGS del entorno.
Una vez que se haya actualizado cada equipo, ejecute los siguientes comandos en cualquier nodo de HGS para desencadenar el proceso de actualización:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Si se presentó una nueva Directiva, se deshabilitará de forma predeterminada.
Para habilitar la nueva Directiva, debe encontrarla en la lista de directivas de Microsoft (con el prefijo ' HGS_ ') y, a continuación, habilitarla con los siguientes comandos:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Administración de directivas de atestación
HGS mantiene varias directivas de atestación que definen el conjunto mínimo de requisitos que un host debe cumplir para que se considere "correcto" y permita ejecutar máquinas virtuales blindadas.
Algunas de estas directivas las define Microsoft, otras se agregan mediante el usuario para definir las directivas de integridad de código permitidas, las líneas de base de TPM y los hosts de su entorno.
Es necesario realizar el mantenimiento periódico de estas directivas para asegurarse de que los hosts pueden seguir atestando correctamente a medida que se actualizan y reemplazan, y para asegurarse de que los hosts o las configuraciones que no son de confianza estén bloqueados contra la atestación correcta.

En el caso de la atestación de confianza de administrador, solo hay una directiva que determina si un host es correcto: pertenencia a un grupo de seguridad conocido y de confianza.
La atestación de TPM es más complicada e implica varias directivas para medir el código y la configuración de un sistema antes de determinar si es correcto.

Se puede configurar un solo HGS con directivas de Active Directory y TPM a la vez, pero el servicio solo comprobará las directivas del modo actual para el que está configurada cuando un host intenta realizar la atestación.
Para comprobar el modo del servidor HGS, ejecute `Get-HgsServer` .

### <a name="default-policies"></a>Directivas predeterminadas
En el caso de la atestación de confianza de TPM, hay varias directivas integradas configuradas en HGS.
Algunas de estas directivas están "bloqueadas", lo que significa que no se pueden deshabilitar por motivos de seguridad.
En la tabla siguiente se explica el propósito de cada directiva predeterminada.

Nombre de directiva                    | Propósito
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Requiere que los hosts tengan habilitado el arranque seguro. Esto es necesario para medir los archivos binarios de inicio y otros valores de configuración bloqueados por UEFI.
Hgs_UefiDebugDisabled          | Garantiza que los hosts no tengan habilitado un depurador de kernel. Los depuradores de modo de usuario se bloquean con directivas de integridad de código.
Hgs_SecureBootSettings         | Directiva negativa para asegurarse de que los hosts coinciden al menos con una línea base de TPM (definida por el administrador).
Hgs_CiPolicy                   | Directiva negativa para asegurarse de que los hosts usan una de las directivas de CI definidas por el administrador.
Hgs_HypervisorEnforcedCiPolicy | Requiere que el hipervisor aplique la Directiva de integridad de código. Al deshabilitar esta Directiva, se debilitan las protecciones frente a ataques de directivas de integridad de código en modo kernel.
Hgs_FullBoot                   | Garantiza que el host no se reanude de la suspensión o hibernación. Los hosts se deben reiniciar o apagar correctamente para pasar esta Directiva.
Hgs_VsmIdkPresent              | Requiere que la seguridad basada en la virtualización se ejecute en el host. IDK representa la clave necesaria para cifrar la información que se devuelve al espacio de memoria seguro del host.
Hgs_PageFileEncryptionEnabled  | Requiere que los complementos de paginación estén cifrados en el host. La deshabilitación de esta Directiva puede dar lugar a la exposición de la información si se inspecciona un archivo de paginación sin cifrar en busca de secretos de inquilino.
Hgs_BitLockerEnabled           | Requiere que BitLocker esté habilitado en el host de Hyper-V. Esta directiva está deshabilitada de forma predeterminada por motivos de rendimiento y no se recomienda que esté habilitada. Esta Directiva no tiene en cuenta el cifrado de las propias máquinas virtuales blindadas.
Hgs_IommuEnabled               | Requiere que el host tenga un dispositivo IOMMU en uso para evitar ataques de acceso directo a la memoria. La deshabilitación de esta directiva y el uso de hosts sin un IOMMU habilitado pueden exponer secretos de máquina virtual de inquilino para ataques de memoria directos.
Hgs_NoHibernation              | Requiere que se deshabilite la hibernación en el host de Hyper-V. La deshabilitación de esta directiva podría permitir que los hosts guardaran la memoria de la máquina virtual blindada en un archivo de hibernación sin cifrar.
Hgs_NoDumps                    | Requiere que se deshabiliten los volcados de memoria en el host de Hyper-V. Si deshabilita esta Directiva, se recomienda que configure el cifrado de volcado para evitar que se guarde la memoria de la máquina virtual blindada en los archivos de volcado sin cifrar.
Hgs_DumpEncryption             | Requiere volcados de memoria, si están habilitados en el host de Hyper-V, para cifrarse con una clave de cifrado que sea de confianza para HGS. Esta Directiva no se aplica si los volcados de memoria no están habilitados en el host. Si esta directiva y los archivos de * \_ volcado de HGS* están deshabilitados, la memoria de la máquina virtual blindada podría guardarse en un archivo de volcado sin cifrar.
Hgs_DumpEncryptionKey          | Directiva negativa para asegurarse de que los hosts configurados para permitir volcados de memoria usen una clave de cifrado de archivo de volcado definida por el administrador conocida para HGS. Esta Directiva no se aplica cuando se deshabilita *HGS \_ DumpEncryption* .

### <a name="authorizing-new-guarded-hosts"></a>Autorización de nuevos hosts protegidos
Para autorizar que un nuevo host se convierta en un host protegido (por ejemplo, atestar correctamente), HGS debe confiar en el host y, cuando se configura para usar la atestación de confianza de TPM, el software que se ejecuta en él.
Los pasos para autorizar un nuevo host difieren en función del modo de atestación para el que HGS está configurado actualmente.
Para comprobar el modo de atestación del tejido protegido, ejecute `Get-HgsServer` en cualquier nodo de HGS.

#### <a name="software-configuration"></a>Configuración del software
En el nuevo host de Hyper-V, asegúrese de que está instalado Windows Server 2016 Datacenter Edition.
Windows Server 2016 Standard no puede ejecutar máquinas virtuales blindadas en un tejido protegido.
Es posible que el host esté instalado en Desktop Experience o Server Core.

En el servidor con experiencia de escritorio y Server Core, debe instalar los roles de servidor de compatibilidad con Hyper-V y protección de host de Hyper-V:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Atestación de confianza de administrador
Para registrar un nuevo host en HGS cuando se usa la atestación de confianza de administrador, primero debe agregar el host a un grupo de seguridad en el dominio al que está unido.
Normalmente, cada dominio tendrá un grupo de seguridad para los hosts protegidos.
Si ya ha registrado ese grupo con HGS, la única acción que debe realizar es reiniciar el host para actualizar la pertenencia a grupos.

Para comprobar qué grupos de seguridad son de confianza para HGS, ejecute el siguiente comando:

```powershell
Get-HgsAttestationHostGroup
```

Para registrar un nuevo grupo de seguridad con HGS, primero Capture el identificador de seguridad (SID) del grupo en el dominio del host y registre el SID con HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Las instrucciones sobre cómo configurar la confianza entre el dominio de host y HGS están disponibles en la guía de implementación.

#### <a name="tpm-trusted-attestation"></a>Atestación de confianza de TPM
Cuando HGS se configura en modo TPM, los hosts deben pasar todas las directivas bloqueadas y las directivas "habilitadas" con el prefijo "Hgs_", así como al menos una línea base de TPM, el identificador de TPM y la Directiva de integridad de código.
Cada vez que agregue un nuevo host, tendrá que registrar el nuevo identificador de TPM con HGS.
Siempre que el host ejecute el mismo software (y tenga aplicada la misma directiva de integridad de código) y la línea base de TPM como otro host en su entorno, no será necesario agregar nuevas directivas de CI o líneas de base.

**Agregar el identificador de TPM para un nuevo host** En el nuevo host, ejecute el siguiente comando para capturar el identificador de TPM.
Asegúrese de especificar un nombre único para el host que le ayude a buscarlo en el HGS.
Necesitará esta información si retira el host o desea evitar que se ejecuten máquinas virtuales blindadas en HGS.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copie este archivo en el servidor HGS y luego ejecute el siguiente comando para registrar el host con HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Agregar una nueva línea base de TPM** Si el nuevo host ejecuta una nueva configuración de hardware o firmware para su entorno, es posible que tenga que tomar una nueva línea base de TPM.
Para ello, ejecute el siguiente comando en el host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Si recibe un error que indica que se ha producido un error en la validación del host y no se atestiguará correctamente, no se preocupe.
> Se trata de una comprobación de requisitos previos para asegurarse de que el host puede ejecutar máquinas virtuales blindadas y probablemente signifique que todavía no ha aplicado una directiva de integridad de código u otra configuración necesaria.
> Lea el mensaje de error, realice los cambios sugeridos por él e inténtelo de nuevo.
> Como alternativa, puede omitir la validación en este momento agregando la `-SkipValidation` marca al comando.

Copie la línea de base de TPM en el servidor HGS y regístrela con el siguiente comando.
Le recomendamos que use una Convención de nomenclatura que le ayude a comprender la configuración de hardware y firmware de esta clase de host de Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Agregar una nueva Directiva de integridad de código** Si ha cambiado la Directiva de integridad de código que se ejecuta en los hosts de Hyper-V, tendrá que registrar la nueva directiva con HGS para que esos hosts puedan confirmar correctamente.
En un host de referencia, que sirve como imagen maestra para las máquinas de Hyper-V de confianza en su entorno, Capture una nueva Directiva de CI mediante el `New-CIPolicy` comando.
Le recomendamos que use el nivel de **FilePublisher** y la reserva de **hash** para las directivas de CI del host de Hyper-V.
En primer lugar, debe crear una directiva de CI en modo auditoría para asegurarse de que todo funciona según lo previsto.
Después de validar una carga de trabajo de ejemplo en el sistema, puede aplicar la Directiva y copiar la versión forzada en HGS.
Para obtener una lista completa de las opciones de configuración de directivas de integridad de código, consulte la [documentación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

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

Una vez que la Directiva se haya creado, probado y aplicado, copie el archivo binario (. p7b) en el servidor HGS y registre la Directiva.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Agregar una clave de cifrado de volcado de memoria**

Cuando la directiva *HGS \_ nodumps* está deshabilitada y la directiva *HGS \_ DumpEncryption* está habilitada, los hosts protegidos pueden tener habilitados los volcados de memoria (incluidos los volcados de memoria) siempre que se cifren los volcados. Los hosts protegidos solo pasarán la atestación si tienen volcados de memoria deshabilitados o los cifran con una clave conocida para HGS. De forma predeterminada, no hay claves de cifrado de volcado configuradas en HGS.

Para agregar una clave de cifrado de volcado a HGS, use el `Add-HgsAttestationDumpPolicy` cmdlet para proporcionar HGS con el hash de la clave de cifrado de volcado.
Si captura una línea de base de TPM en un host de Hyper-V configurado con el cifrado de volcado, el hash se incluye en el tcglog y se puede proporcionar al `Add-HgsAttestationDumpPolicy` cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Como alternativa, puede proporcionar directamente la representación de cadena del hash al cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Asegúrese de agregar cada clave de cifrado de volcado única a HGS si decide usar diferentes claves en el tejido protegido.
Los hosts que cifran volcados de memoria con una clave no conocida para HGS no pasarán la atestación.

Consulte la documentación de Hyper-V para obtener más información acerca de cómo [configurar el cifrado de volcado en hosts](../../virtualization/hyper-v/manage/about-dump-encryption.md).

#### <a name="check-if-the-system-passed-attestation"></a>Comprobar si el sistema ha pasado la atestación
Después de registrar la información necesaria con HGS, debe comprobar si el host supera la atestación.
En el host de Hyper-V recién agregado, ejecute `Set-HgsClientConfiguration` y proporcione las direcciones URL correctas para el clúster de HGS.
Estas direcciones URL se pueden obtener ejecutando `Get-HgsServer` en cualquier nodo de HGS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Si el estado resultante no indica "IsHostGuarded: true", deberá solucionar los problemas de configuración.
En el host que no se pudo realizar la atestación, ejecute el siguiente comando para obtener un informe detallado sobre los problemas que pueden ayudarle a resolver la atestación errónea.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Si usa Windows Server 2019 o Windows 10, la versión 1809 y usa directivas de integridad de código, `Get-HgsTrace` puede devolver un error para el diagnóstico activo de la **Directiva de integridad de código** .
> Puede omitir este resultado de forma segura cuando sea el único diagnóstico con errores.

### <a name="review-attestation-policies"></a>Revisar las directivas de atestación
Para revisar el estado actual de las directivas configuradas en HGS, ejecute los siguientes comandos en cualquier nodo HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Si encuentra una directiva habilitada que ya no cumple su requisito de seguridad (por ejemplo, una directiva de integridad de código antigua que ahora se considera no segura), puede deshabilitarla reemplazando el nombre de la Directiva en el siguiente comando:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

Del mismo modo, puede usar `Enable-HgsAttestationPolicy` para volver a habilitar una directiva.

Si ya no necesita una directiva y desea quitarla de todos los nodos de HGS, ejecute `Remove-HgsAttestationPolicy -Name 'PolicyName'` para eliminar la Directiva de forma permanente.

## <a name="changing-attestation-modes"></a>Cambiar los modos de atestación
Si inició el tejido protegido con la atestación de confianza de administrador, es probable que desee actualizar al modo de atestación de TPM mucho más seguro en cuanto tenga suficientes hosts compatibles con TPM 2,0 en su entorno.
Cuando esté listo para cambiar, puede cargar previamente todos los artefactos de atestación (directivas de CI, líneas de base de TPM e identificadores de TPM) en HGS mientras continúa ejecutando HGS con la atestación de confianza de administrador.
Para ello, solo tiene que seguir las instrucciones de la sección [autorización de un nuevo host protegido](#authorizing-new-guarded-hosts) .

Una vez que haya agregado todas las directivas a HGS, el paso siguiente consiste en ejecutar un intento de atestación sintética en los hosts para ver si pasaría la atestación en modo TPM.
Esto no afecta al estado operativo actual del HGS.
Los siguientes comandos deben ejecutarse en un equipo que tenga acceso a todos los hosts del entorno y al menos a un nodo de HGS.
Si el Firewall u otras directivas de seguridad lo impiden, puede omitir este paso.
Cuando sea posible, se recomienda ejecutar la atestación sintética para darle una buena indicación de si "voltear" al modo TPM provocará un tiempo de inactividad para las máquinas virtuales.

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

Una vez completado el diagnóstico, revise la información de salida para determinar si algún host habría producido un error en la atestación en el modo TPM.
Vuelva a ejecutar el diagnóstico hasta que obtenga un "paso" de cada host y, a continuación, cambie el HGS al modo TPM.

**Cambiar al modo TPM** tarda unos segundos en completarse.
Ejecute el siguiente comando en cualquier nodo de HGS para actualizar el modo de atestación.

```powershell
Set-HgsServer -TrustTpm
```

Si experimenta problemas y necesita volver a cambiar al modo Active Directory, puede hacerlo mediante la ejecución de `Set-HgsServer -TrustActiveDirectory` .

Una vez que haya confirmado que todo funciona según lo previsto, debe quitar todos los grupos host de Active Directory de confianza del HGS y quitar la confianza entre los dominios HGS y tejido.
Si deja el Active Directory confianza en su lugar, se arriesga a que alguien vuelva a habilitar la confianza y cambie HGS a Active Directory modo, lo que podría permitir que el código que no es de confianza se ejecute sin activar en los hosts protegidos.

## <a name="key-management"></a>Administración de claves
La solución de tejido protegido usa varios pares de claves pública y privada para validar la integridad de los distintos componentes de la solución y cifrar los secretos de los inquilinos.
El servicio de protección de host se configura con al menos dos certificados (con claves públicas y privadas), que se usan para firmar y cifrar las claves que se usan para iniciar máquinas virtuales blindadas.
Esas claves deben administrarse cuidadosamente.
Si un adversario adquiere la clave privada, podrá desproteger las máquinas virtuales que se ejecuten en el tejido o configurar un clúster de impostor HGS que use directivas de atestación más débiles para omitir las protecciones que ha implementado.
Si pierde las claves privadas durante un desastre y no las encuentra en una copia de seguridad, deberá configurar un nuevo par de claves y hacer que cada máquina virtual se vuelva a crear con clave para autorizar los certificados nuevos.

En esta sección se describen los temas de administración de claves generales que le ayudarán a configurar las claves para que sean funcionales y seguras.

### <a name="adding-new-keys"></a>Agregar nuevas claves
Mientras HGS debe inicializarse con un conjunto de claves, puede agregar más de un cifrado y una clave de firma a HGS.
Las dos razones más comunes por las que agregaría claves nuevas a HGS son:
1. Para admitir escenarios de "traiga su propia clave" donde los inquilinos copian sus claves privadas en el módulo de seguridad de hardware y solo autorizan sus claves para iniciar sus máquinas virtuales blindadas.
2. Para reemplazar las claves existentes para HGS, agregue primero las nuevas claves y mantenga ambos conjuntos de claves hasta que se actualice cada configuración de máquina virtual para usar las nuevas claves.

El proceso para agregar las nuevas claves es diferente en función del tipo de certificado que se use.

**Opción 1: agregar un certificado almacenado en un HSM**

Nuestro enfoque recomendado para proteger las claves HGS es usar los certificados creados en un módulo de seguridad de hardware (HSM).
Los HSM garantizan que el uso de las claves está ligado al acceso físico a un dispositivo de seguridad en el centro de información.
Cada HSM es diferente y tiene un proceso único para crear certificados y registrarlos con HGS.
Los siguientes pasos están pensados para proporcionar una guía aproximada para el uso de certificados respaldados por HSM.
Consulte la documentación del proveedor de HSM para ver los pasos y las capacidades exactos.

1. Instale el software HSM en cada nodo de HGS del clúster. En función de si tiene un dispositivo HSM local o de red, puede que tenga que configurar el HSM para conceder acceso a su equipo al almacén de claves.
2. Crear 2 certificados en el HSM con **claves RSA de 2048 bits** para el cifrado y la firma
    1. Creación de un certificado de cifrado con la propiedad de uso de clave de **cifrado de datos** en el HSM
    2. Creación de un certificado de firma con la propiedad uso de la clave de **firma digital** en el HSM
3. Instale los certificados en el almacén de certificados local de cada nodo de HGS según las instrucciones del proveedor de HSM.
4. Si el HSM usa permisos granulares para conceder permiso a aplicaciones o usuarios específicos para usar la clave privada, tendrá que conceder a su cuenta de servicio administrada de grupo HGS acceso al certificado. Puede encontrar el nombre de la cuenta de gMSA HGS mediante la ejecución de`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Agregue los certificados de firma y cifrado a HGS reemplazando las huellas digitales por las de los certificados en los siguientes comandos:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opción 2: agregar certificados de software no exportables**

Si tiene un certificado respaldado por software emitido por una entidad de certificación pública o de la empresa que tiene una clave privada no exportable, deberá agregar el certificado a HGS mediante su huella digital.
1. Instale el certificado en el equipo según las instrucciones de la entidad de certificación.
2. Conceda a la cuenta de servicio administrada de grupo HGS acceso de lectura a la clave privada del certificado. Puede encontrar el nombre de la cuenta de gMSA HGS mediante la ejecución de`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registre el certificado con HGS mediante el siguiente comando y sustituyendo la huella digital del certificado (cambiar *cifrado* a *firma* para certificados de firma):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Tendrá que instalar manualmente la clave privada y conceder acceso de lectura a la cuenta gMSA en cada nodo de HGS.
> HGS no puede replicar automáticamente las claves privadas de *ningún* certificado registrado por su huella digital.

**Opción 3: agregar certificados almacenados en archivos PFX**

Si tiene un certificado de software respaldado con una clave privada exportable que puede almacenarse en el formato de archivo PFX y protegerse con una contraseña, HGS puede administrar automáticamente los certificados.
Los certificados agregados con archivos PFX se replican automáticamente en cada nodo del clúster de HGS y HGS protege el acceso a las claves privadas.
Para agregar un certificado nuevo mediante un archivo PFX, ejecute los siguientes comandos en cualquier nodo de HGS (cambiar *cifrado* a *firma* para certificados de firma):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificar y cambiar los certificados principales** Aunque HGS puede admitir varios certificados de firma y cifrado, usa un par como certificados "primarios".
Estos son los certificados que se usarán si alguien descarga los metadatos de protección para ese clúster de HGS.
Para comprobar qué certificados están marcados actualmente como certificados principales, ejecute el siguiente comando:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Para establecer un nuevo certificado de cifrado o firma principal, busque la huella digital del certificado deseado y márquela como principal con los siguientes comandos:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renovar o reemplazar claves
Al crear los certificados que usa HGS, a los certificados se les asigna una fecha de expiración de acuerdo con la Directiva de la entidad de certificación y la información de la solicitud.
Normalmente, en escenarios en los que la validez del certificado es importante, como la protección de las comunicaciones HTTP, los certificados se deben renovar antes de que expiren para evitar una interrupción del servicio o un mensaje de error Worrisome.
HGS no usa certificados en ese sentido.
HGS es simplemente usar certificados como una forma cómoda de crear y almacenar un par de claves asimétricas.
Un certificado de cifrado o firma expirado en HGS no indica un punto débil o pérdida de protección para las máquinas virtuales blindadas.
Además, HGS no realiza comprobaciones de revocación de certificados.
Si se revoca un certificado de HGS o un certificado de entidad emisora, no afectará al uso del certificado por HGS.

La única vez que debe preocuparse de un certificado HGS es si tiene motivos para creer que se ha robado su clave privada.
En ese caso, la integridad de las máquinas virtuales blindadas está en riesgo porque la posesión de la mitad privada del par de claves de firma y cifrado del HGS es suficiente para quitar las protecciones de blindaje de una máquina virtual o un servidor de HGS falso con directivas de atestación más débiles.

Si se encuentra en esa situación o son necesarios para que los estándares de cumplimiento actualicen las claves de certificado con regularidad, en los pasos siguientes se describe el proceso para cambiar las claves en un servidor de HGS.
Tenga en cuenta que la siguiente guía representa una tarea importante que producirá una interrupción del servicio en cada máquina virtual atendida por el clúster de HGS.
Se requiere un planeamiento adecuado para cambiar las claves de HGS con el fin de minimizar la interrupción del servicio y garantizar la seguridad de las máquinas virtuales de inquilino.

En un nodo HGS, realice los pasos siguientes para registrar un nuevo par de certificados de cifrado y firma.
Vea la sección sobre [Cómo agregar nuevas claves](#adding-new-keys) para obtener información detallada sobre las distintas formas de agregar nuevas claves a HGS.

1. Cree un nuevo par de certificados de cifrado y firma para el servidor de HGS. Idealmente, estos se crearán en un módulo de seguridad de hardware.

2. Registre los nuevos certificados de cifrado y firma con **Add-HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```

3. Si ha usado huellas digitales, deberá ir a cada nodo del clúster para instalar la clave privada y conceder al HGS gMSA acceso a la clave.

4. Convertir los certificados nuevos en los certificados predeterminados en HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

En este momento, los datos de blindaje creados con los metadatos obtenidos del nodo HGS usarán los nuevos certificados, pero las máquinas virtuales existentes seguirán funcionando porque los certificados más antiguos todavía están allí.

Para asegurarse de que todas las máquinas virtuales existentes funcionan con las nuevas claves, deberá actualizar el protector de clave en cada máquina virtual.

Se trata de una acción que requiere que el propietario de la máquina virtual (persona o entidad en posesión del guardián "propietario") esté implicado. Para cada máquina virtual blindada, realice los pasos siguientes:

1. Apague la máquina virtual. No se puede volver a activar la máquina virtual hasta que se completen los pasos restantes o, de lo contrario, deberá volver a iniciar el proceso.

2. Guarde el protector de clave actual en un archivo:`Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`

3. Transferir el KP al propietario de la máquina virtual

4. Haga que el propietario Descargue la información actualizada de la protección desde HGS e impórtela en su sistema local.

5. Lea el archivo de KP actual en la memoria, conceda al nuevo Guardián acceso a la KP y guárdelo en un nuevo archivo mediante la ejecución de los siguientes comandos:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```

6. Vuelva a copiar el KP actualizado en el tejido de hospedaje.

7. Aplique el KP a la máquina virtual original:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```

8. Por último, inicie la máquina virtual y asegúrese de que se ejecuta correctamente.

    > [!NOTE]
    > Si el propietario de la máquina virtual establece un protector de clave incorrecto en la máquina virtual y no autoriza a su tejido a ejecutar la máquina virtual, no podrá iniciar la máquina virtual blindada.
    > Para volver al último protector de clave válida conocida, ejecute`Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

    Una vez que todas las máquinas virtuales se han actualizado para autorizar las nuevas claves de protección, puede deshabilitar y quitar las claves antiguas.

9.  Obtener las huellas digitales de los certificados antiguos de`Get-HgsKeyProtectionCertificate -IsPrimary $false`

10. Deshabilite cada certificado mediante la ejecución de los siguientes comandos:

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
    ```

11. Después de asegurarse de que las máquinas virtuales sigan pudiendo empezar con los certificados deshabilitados, quite los certificados del HGS mediante la ejecución de los siguientes comandos:

    ```powershell
    Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
    Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
    ```

> [!IMPORTANT]
> Las copias de seguridad de máquinas virtuales contendrán información antigua del protector de clave que permite usar los certificados antiguos para iniciar la máquina virtual.
> Si sabe que la clave privada se ha puesto en peligro, debe suponer que las copias de seguridad de la máquina virtual también se pueden poner en peligro y tomar las medidas adecuadas.
> Al destruir la configuración de la máquina virtual a partir de las copias de seguridad (. vmcx), se quitarán los protectores de clave, a costa de que sea necesario usar la contraseña de recuperación de BitLocker para arrancar la máquina virtual la próxima vez.

### <a name="key-replication-between-nodes"></a>Replicación de claves entre nodos
Todos los nodos del clúster de HGS deben configurarse con los mismos certificados SSL de cifrado, firma y (cuando se configuran).
Esto es necesario para garantizar que los hosts de Hyper-V que llegan a cualquier nodo del clúster puedan atender correctamente sus solicitudes.

**Si ha inicializado el servidor HGS con certificados basados en pfx** , HGS replicará automáticamente la clave pública y la privada de dichos certificados en todos los nodos del clúster.
Solo tiene que agregar las claves en un nodo.

**Si ha inicializado el servidor HGS con referencias de certificado** o huellas digitales, HGS solo replicará la clave *pública* del certificado en cada nodo.
Además, HGS no puede conceder acceso a la clave privada en ningún nodo de este escenario.
Por lo tanto, es su responsabilidad:
1. Instalación de la clave privada en cada nodo de HGS
2. Conceda al grupo HGS el acceso a la cuenta de servicio administrada (gMSA) a la clave privada en cada nodo. estas tareas agregan carga operativa adicional, sin embargo, son necesarias para claves respaldadas por HSM y certificados con claves privadas no exportables.

Los **certificados SSL** nunca se replican de ninguna forma.
Es su responsabilidad inicializar cada servidor de HGS con el mismo certificado SSL y actualizar cada servidor cada vez que decida renovar o reemplazar el certificado SSL.
Al reemplazar el certificado SSL, se recomienda que lo haga mediante el cmdlet [set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) .

## <a name="unconfiguring-hgs"></a>Desconfiguración de HGS

Si necesita retirar o volver a configurar de forma significativa un servidor HGS, puede hacerlo mediante los cmdlets [Clear-hgsserver](https://technet.microsoft.com/library/mt652176.aspx) o [Uninstall-hgsserver](https://technet.microsoft.com/library/mt652182.aspx) .

### <a name="clearing-the-hgs-configuration"></a>Borrado de la configuración de HGS

Para quitar un nodo del clúster de HGS, use el cmdlet [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) .
Este cmdlet realizará los siguientes cambios en el servidor en el que se ejecuta:

- Anula el registro de los servicios de atestación y protección de claves.
- Quita el punto de conexión de administración de JEA "Microsoft. Windows. HGS"
- Quita el equipo local del clúster de conmutación por error de HGS.

Si el servidor es el último nodo de HGS del clúster, el clúster y su correspondiente recurso de nombre de red distribuida también se destruirán.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Una vez finalizada la operación de borrado, el servidor HGS se puede reinicializar con [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Si usó [install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) para configurar un dominio de Active Directory Domain Services, ese dominio permanecerá configurado y operativo después de la operación de borrado.

### <a name="uninstalling-hgs"></a>Desinstalación de HGS

Si desea quitar un nodo del clúster de HGS **y** degradar el dominio de Active Directory controlador que se ejecuta en él, use el cmdlet [Uninstall-HgsServer](https://technet.microsoft.com/library/mt652182.aspx) .
Este cmdlet realizará los siguientes cambios en el servidor en el que se ejecuta:

- Anula el registro de los servicios de atestación y protección de claves.
- Quita el punto de conexión de administración de JEA "Microsoft. Windows. HGS"
- Quita el equipo local del clúster de conmutación por error de HGS.
- Degrada el controlador de Dominio de Active Directory, si está configurado

Si el servidor es el último nodo de HGS del clúster, también se destruirán el dominio, el clúster de conmutación por error y el recurso de nombre de red distribuida del clúster.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Una vez que se ha completado la operación de desinstalación y se ha reiniciado el equipo, puede volver a instalar ADDC y HGS mediante [install-hgsserver](https://technet.microsoft.com/library/mt652169.aspx) o unir el equipo a un dominio e inicializar el servidor HGS en ese dominio con [Initialize-hgsserver](https://technet.microsoft.com/library/mt652185.aspx).

Si ya no desea usar el equipo como nodo HGS, puede quitar el rol de Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```