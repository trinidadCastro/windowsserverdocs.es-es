---
title: Solución de problemas con la herramienta de diagnóstico de tejido protegido
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: c69fc70282ff61ecce25f6413244d7ba3a5ba3bc
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265827"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Solución de problemas con la herramienta de diagnóstico de tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se describe el uso de la herramienta de diagnóstico de tejido protegido para identificar y corregir errores comunes en la implementación, la configuración y el funcionamiento continuo de la infraestructura de tejido protegido. Esto incluye el servicio de protección de host (HGS), todos los hosts protegidos y servicios de soporte técnico como DNS y Active Directory. La herramienta de diagnóstico se puede usar para realizar un primer paso en la clasificación de un tejido protegido con errores, lo que proporciona a los administradores un punto de partida para resolver las interrupciones e identificar los activos mal configurados. La herramienta no sustituye al funcionamiento de un tejido protegido y solo sirve para comprobar rápidamente los problemas más comunes que se producen durante las operaciones cotidianas.

La documentación completa de los cmdlets que se usan en este artículo se puede encontrar en la [Referencia del módulo HgsDiagnostics](https://docs.microsoft.com/powershell/module/hgsdiagnostics/?view=win10-ps).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)]

## <a name="quick-start"></a>Inicio rápido

Puede diagnosticar un host protegido o un nodo HGS llamando a lo siguiente desde una sesión de Windows PowerShell con privilegios de administrador local:

```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```

Esto detectará automáticamente el rol del host actual y diagnosticará cualquier problema relevante que se pueda detectar automáticamente.  Todos los resultados generados durante este proceso se muestran debido a la presencia del modificador `-Detailed`.

En el resto de este tema se proporcionará un tutorial detallado sobre el uso avanzado de `Get-HgsTrace` para realizar acciones como el diagnóstico de varios hosts a la vez y la detección de una configuración de uninode compleja entre nodos.

## <a name="diagnostics-overview"></a>Información general sobre diagnósticos

Los diagnósticos de tejido protegido están disponibles en cualquier host con características y herramientas relacionadas con máquinas virtuales blindadas instaladas, incluidos los hosts que ejecutan Server Core.  Actualmente, los diagnósticos se incluyen con las siguientes características o paquetes:

1. Rol de servicio de protección de host
2. Compatibilidad de Hyper-V con protección de host
3. Herramientas de blindaje de máquinas virtuales para la administración de tejidos
4. Herramientas de administración remota del servidor (RSAT)

Esto significa que las herramientas de diagnóstico estarán disponibles en todos los hosts protegidos, nodos de HGS, determinados servidores de administración de tejido y las estaciones de trabajo de Windows 10 con [rsat](https://www.microsoft.com/download/details.aspx?id=45520) instaladas.  Los diagnósticos se pueden invocar desde cualquiera de las máquinas anteriores con la intención de diagnosticar cualquier host protegido o nodo de HGS en un tejido protegido; con los destinos de seguimiento remoto, los diagnósticos pueden localizar y conectarse a los hosts que no sean el equipo que ejecuta el diagnóstico.

Cada host de destino se denomina "destino de seguimiento".  Los destinos de seguimiento se identifican por sus nombres de host y roles.  Los roles describen la función que realiza un destino de seguimiento determinado en un tejido protegido.  Actualmente, los destinos de seguimiento admiten `HostGuardianService` y `GuardedHost` roles.  Tenga en cuenta que es posible que un host ocupe varios roles a la vez y que también sea compatible con el diagnóstico; sin embargo, esto no debería realizarse en entornos de producción.  Los hosts HGS y Hyper-V deben mantenerse separados y distintos en todo momento.

Los administradores pueden iniciar cualquier tarea de diagnóstico mediante la ejecución de `Get-HgsTrace`.  Este comando realiza dos funciones distintas en función de los modificadores proporcionados en tiempo de ejecución: colección y diagnóstico de seguimiento.  Estos dos combinados componen la totalidad de la herramienta de diagnóstico de tejido protegido.  Aunque no se requiere explícitamente, los diagnósticos más útiles requieren seguimientos que solo se pueden recopilar con credenciales de administrador en el destino de seguimiento.  Si el usuario que ejecuta la colección de seguimiento contiene privilegios insuficientes, se producirá un error en los seguimientos que requieran elevación mientras se pasarán todos los demás.  Esto permite el diagnóstico parcial en el caso de que un operador con privilegios mínimos realice la evaluación de prioridades. 

### <a name="trace-collection"></a>Colección de seguimiento

De forma predeterminada, `Get-HgsTrace` solo recopilará los seguimientos y los guardará en una carpeta temporal.  Los seguimientos adoptan la forma de una carpeta, con el nombre del host de destino, rellenado con archivos con formato especial que describen cómo se configura el host.  Los seguimientos también contienen metadatos que describen cómo se invocaron los diagnósticos para recopilar los seguimientos.  Los diagnósticos usan estos datos para rehidratar la información sobre el host al realizar el diagnóstico manual.

Si es necesario, los seguimientos se pueden revisar manualmente.  Todos los formatos son legibles para el usuario (XML) o se pueden inspeccionar fácilmente mediante las herramientas estándar (por ejemplo, certificados X509 y las extensiones del shell de cifrado de Windows).  Sin embargo, tenga en cuenta que los seguimientos no están diseñados para el diagnóstico manual y siempre son más eficaces para procesar los seguimientos con las funciones de diagnóstico de `Get-HgsTrace`.

Los resultados de la ejecución de la colección de seguimiento no hacen ninguna indicación del estado de un host determinado.  Simplemente indican que los seguimientos se recopilaron correctamente.  Es necesario usar los mecanismos de diagnóstico de `Get-HgsTrace` para determinar si los seguimientos indican un entorno con errores.

Con el parámetro `-Diagnostic`, puede restringir la recopilación de seguimiento solo a los seguimientos necesarios para operar el diagnóstico especificado.  Esto reduce la cantidad de datos recopilados, así como los permisos necesarios para invocar diagnósticos.

### <a name="diagnosis"></a>Diagnóstico

Los seguimientos recopilados se pueden diagnosticar facilitando `Get-HgsTrace` la ubicación de los seguimientos a través del parámetro `-Path` y especificando el modificador `-RunDiagnostics`.  Además, `Get-HgsTrace` puede realizar la recopilación y el diagnóstico en un solo paso proporcionando el modificador de `-RunDiagnostics` y una lista de destinos de seguimiento.  Si no se proporciona ningún destino de seguimiento, el equipo actual se usa como destino implícito, y su rol se deduce mediante la inspección de los módulos de Windows PowerShell instalados.

El diagnóstico proporcionará los resultados en un formato jerárquico que muestre qué destinos de seguimiento, conjuntos de diagnósticos y diagnósticos individuales son responsables de un error determinado.  Los errores incluyen recomendaciones de corrección y resolución si se puede determinar la acción que debe realizarse a continuación.  De forma predeterminada, se ocultan los resultados de paso y irrelevantes.  Para ver todo lo que prueban los diagnósticos, especifique el modificador `-Detailed`.  Esto hará que todos los resultados aparezcan independientemente de su estado.

Es posible restringir el conjunto de diagnósticos que se ejecutan mediante el parámetro `-Diagnostic`.  Esto le permite especificar qué clases de diagnóstico se deben ejecutar en los destinos de seguimiento y suprimir todas las demás.  Entre los ejemplos de clases de diagnóstico disponibles se incluyen las funciones de red, los procedimientos recomendados y el hardware del cliente.  Consulte la [documentación del cmdlet](https://technet.microsoft.com/library/mt718831.aspx) para encontrar una lista actualizada de los diagnósticos disponibles.

> [!WARNING]
> Los diagnósticos no sustituyen a una canalización segura de supervisión y respuesta a incidentes.  Hay un paquete de System Center Operations Manager disponible para supervisar tejidos protegidos, así como varios canales de registro de eventos que se pueden supervisar para detectar problemas pronto.  Los diagnósticos se pueden usar para evaluar rápidamente estos errores y establecer una línea de acción.

## <a name="targeting-diagnostics"></a>Destinar diagnósticos

`Get-HgsTrace` funciona con destinos de seguimiento.  Un destino de seguimiento es un objeto que se corresponde con un nodo HGS o un host protegido dentro de un tejido protegido.  Puede considerarse como una extensión de una `PSSession` que incluye la información que solo requiere diagnóstico como el rol del host en el tejido.  Los destinos se pueden generar implícitamente (por ejemplo, diagnóstico local o manual) o explícitamente con el comando `New-HgsTraceTarget`.

### <a name="local-diagnosis"></a>Diagnóstico local

De forma predeterminada, `Get-HgsTrace` se destinará al host local (es decir, donde se invoca el cmdlet).  Esto se conoce como el destino local implícito.  El destino local implícito solo se usa cuando no se proporciona ningún destino en el parámetro `-Target` y no se encuentra ningún seguimiento existente previamente en el `-Path`.

El destino local implícito usa la inferencia de roles para determinar qué rol desempeña el host actual en el tejido protegido.  Esto se basa en los módulos de Windows PowerShell instalados que corresponden aproximadamente a las características que se han instalado en el sistema.  La presencia del módulo `HgsServer` hará que el destino del seguimiento adopte el rol `HostGuardianService` y la presencia del módulo `HgsClient` hará que el destino del seguimiento adopte el rol `GuardedHost`.  Es posible que un host determinado tenga ambos módulos presentes, en cuyo caso se tratará como un `HostGuardianService` y un `GuardedHost`.

Por lo tanto, la invocación predeterminada de diagnósticos para recopilar seguimientos localmente:

```PowerShell
Get-HgsTrace
```

... es equivalente a lo siguiente:

```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```

> [!TIP]
> `Get-HgsTrace` puede aceptar destinos a través de la canalización o directamente a través del parámetro `-Target`.  No hay ninguna diferencia entre ambas operaciones.

### <a name="remote-diagnosis-using-trace-targets"></a>Diagnóstico remoto con destinos de seguimiento

Es posible diagnosticar de forma remota un host mediante la generación de destinos de seguimiento con información de conexión remota.  Lo único que se necesita es el nombre de host y un conjunto de credenciales capaces de conectarse mediante la comunicación remota de Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
En este ejemplo se generará un símbolo del sistema para recopilar las credenciales del usuario remoto y, a continuación, los diagnósticos se ejecutarán con el host remoto en `hgs-01.secure.contoso.com` para completar la recopilación de seguimiento.  Los seguimientos resultantes se descargan en el host local y después se diagnostican.  Los resultados del diagnóstico se presentan de la misma forma que cuando se realiza el [diagnóstico local](#local-diagnosis).  Del mismo modo, no es necesario especificar un rol, ya que se puede inferir en función de los módulos de Windows PowerShell instalados en el sistema remoto.

El diagnóstico remoto emplea la comunicación remota de Windows PowerShell para todos los accesos al host remoto.  Por lo tanto, es un requisito previo que el destino de seguimiento tenga habilitada la comunicación remota de Windows PowerShell (consulte [Habilitar PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) y que el host local esté configurado correctamente para iniciar conexiones con el destino.

> [!NOTE]
> En la mayoría de los casos, solo es necesario que el localhost forme parte del mismo bosque Active Directory y que se use un nombre de host DNS válido.  Si su entorno usa un modelo de Federación más complicado o desea usar direcciones IP directas para la conectividad, es posible que deba realizar una configuración adicional, como la configuración de los [hosts de confianza](https://technet.microsoft.com/library/ff700227.aspx)de WinRM.

Puede comprobar que se ha creado una instancia correcta de un destino de seguimiento y que se ha configurado para aceptar conexiones mediante el cmdlet `Test-HgsTraceTarget`:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Este comando devolverá `$True` si `Get-HgsTrace` podría establecer una sesión de diagnóstico remoto con el destino de seguimiento.  En caso de error, este cmdlet devolverá información de estado relevante para solucionar el problema de la conexión remota de Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenciales IMPLÍCITAS

Cuando se realiza el diagnóstico remoto desde un usuario con privilegios suficientes para conectarse de forma remota al destino de seguimiento, no es necesario proporcionar credenciales a `New-HgsTraceTarget`.  El cmdlet `Get-HgsTrace` volverá a usar automáticamente las credenciales del usuario que invocó el cmdlet al abrir una conexión.

> [!WARNING]
> Algunas restricciones se aplican a la reutilización de credenciales, especialmente al realizar lo que se conoce como "segundo salto".  Esto sucede cuando se intenta reutilizar las credenciales de una sesión remota en otra máquina.  Es necesario [configurar CredSSP](https://technet.microsoft.com/library/hh849872.aspx) para que admita este escenario, pero esto está fuera del ámbito de administración y solución de problemas de los tejidos protegidos.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Uso de la administración suficiente de Windows PowerShell (JEA) y diagnósticos

El diagnóstico remoto admite el uso de puntos de conexión de Windows PowerShell con restricción de JEA. De forma predeterminada, los destinos de seguimiento remoto se conectarán mediante el punto de conexión de `microsoft.powershell` predeterminado.  Si el destino de seguimiento tiene el rol `HostGuardianService`, también intentará usar el punto de conexión de `microsoft.windows.hgs` que se configura cuando se instala HGS.

Si desea utilizar un punto de conexión personalizado, debe especificar el nombre de la configuración de sesión mientras construye el destino de seguimiento mediante el parámetro `-PSSessionConfigurationName`, como se muestra a continuación:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnóstico de varios hosts

Puede pasar varios destinos de seguimiento a `Get-HgsTrace` a la vez.  Esto incluye una combinación de destinos locales y remotos.  Se realizará un seguimiento de cada destino a su vez y, a continuación, los seguimientos de cada destino se diagnosticarán simultáneamente.  La herramienta de diagnóstico puede usar el mayor conocimiento de la implementación para identificar configuraciones incompletas interactivas entre nodos que de otro modo no serían detectables.  El uso de esta característica solo requiere proporcionar seguimientos de varios hosts simultáneamente (en el caso de diagnóstico manual) o dirigirse a varios hosts al llamar a `Get-HgsTrace` (en el caso del diagnóstico remoto).

Este es un ejemplo del uso de diagnósticos remotos para evaluar un tejido compuesto por dos nodos HGS y dos hosts protegidos, donde se usa uno de los hosts protegidos para iniciar `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> No es necesario que diagnostique todo el tejido protegido al diagnosticar varios nodos.  En muchos casos, es suficiente incluir todos los nodos que pueden estar implicados en una condición de error determinada.  Normalmente es un subconjunto de los hosts protegidos y un número de nodos del clúster de HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnóstico manual mediante seguimientos guardados

En ocasiones, es posible que quiera volver a ejecutar diagnósticos sin recopilar seguimientos de nuevo, o puede que no disponga de las credenciales necesarias para diagnosticar de forma remota todos los hosts del tejido simultáneamente.  El diagnóstico manual es un mecanismo por el que todavía se puede realizar una evaluación de un tejido completo mediante `Get-HgsTrace`, pero sin utilizar la colección de seguimientos remotos.

Antes de realizar el diagnóstico manual, debe asegurarse de que los administradores de cada host del tejido que se van a evaluar estén listos y dispuestos a ejecutar comandos en su nombre.  La salida del seguimiento de diagnóstico no expone ninguna información que, por lo general, es confidencial; no obstante, depende del usuario para determinar si es seguro exponer esta información a otros usuarios.

> [!NOTE]
> Los seguimientos no se anónimos y revelan la configuración de red, la configuración de PKI y otras configuraciones que a veces se consideran información privada.  Por lo tanto, los seguimientos solo se deben transmitir a entidades de confianza dentro de una organización y nunca publicarse públicamente.

Los pasos para realizar un diagnóstico manual son los siguientes:

1. Solicite que cada administrador de host ejecute `Get-HgsTrace` especificar un `-Path` conocido y la lista de diagnósticos que desea ejecutar en los seguimientos resultantes.  Por ejemplo:

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```

2. Solicite a cada administrador de host que empaquete la carpeta de seguimientos resultante y la envíe a usted.  Este proceso puede controlarse a través del correo electrónico, a través de recursos compartidos de archivos o cualquier otro mecanismo basado en las directivas y los procedimientos operativos establecidos por su organización.

3. Combinar todos los seguimientos recibidos en una sola carpeta, sin contenido ni carpetas.

    * Por ejemplo, supongamos que tenía los administradores que le enviaron seguimientos recopilados de cuatro equipos denominados HGS-01, HGS-02, RR1N2608-12 y RR1N2608-13.  Cada administrador le habría enviado una carpeta con el mismo nombre.  Ensamblaría una estructura de directorio que aparece de la manera siguiente:

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. Ejecute los diagnósticos y proporcione la ruta de acceso a la carpeta de seguimiento ensamblada en el parámetro `-Path` y especifique el modificador `-RunDiagnostics` así como los diagnósticos para los que solicitó a los administradores que recopilen seguimientos.  El diagnóstico asumirá que no puede acceder a los hosts que se encuentran dentro de la ruta de acceso y, por lo tanto, intentará usar solo los seguimientos recopilados previamente.  Si faltan seguimientos o están dañados, el diagnóstico solo producirá un error en las pruebas afectadas y continuará normalmente.  Por ejemplo:

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Mezclar seguimientos guardados con destinos adicionales

En algunos casos, es posible que tenga un conjunto de seguimientos recopilados previamente que desea aumentar con seguimientos de host adicionales.  Es posible mezclar seguimientos previamente recopilados con destinos adicionales que se rastrearán y diagnosticarán en una única llamada de diagnósticos.

Siga las instrucciones para recopilar y ensamblar una carpeta de seguimiento especificada anteriormente, llamar a `Get-HgsTrace` con destinos de seguimiento adicionales que no se encuentran en la carpeta de seguimiento recopilada previamente:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

El cmdlet de diagnóstico identificará todos los hosts previamente recopilados y el otro host que aún debe seguirse y realizará el seguimiento necesario.  Se diagnosticará la suma de todos los seguimientos recopilados previamente y recopilados de nuevo.  La carpeta de seguimiento resultante contendrá los seguimientos antiguos y nuevos.

## <a name="known-issues"></a>Problemas conocidos

El módulo diagnósticos de tejido protegido tiene limitaciones conocidas cuando se ejecutan en Windows Server 2019 o Windows 10, versión 1809 y versiones de sistema operativo más recientes.
El uso de las siguientes características puede producir resultados erróneos:

* Atestación de clave de host
* Configuración de HGS de solo atestación (para escenarios de SQL Server Always Encrypted)
* Uso de artefactos de directiva V1 en un servidor HGS en el que el valor predeterminado de la Directiva de atestación es V2

Un error en `Get-HgsTrace` cuando se usan estas características no indica necesariamente que el servidor de HGS o el host protegido estén mal configurados.
Use otras herramientas de diagnóstico como `Get-HgsClientConfiguration` en un host protegido para probar si un host ha pasado la atestación.
