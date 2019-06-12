---
title: Solución de problemas mediante la herramienta de diagnóstico de tejido protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0fb257f693cc27c0bc6dd18fc89e8dc6328ee638
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447342"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>Solución de problemas mediante la herramienta de diagnóstico de tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Este tema describe el uso de la protegido Fabric herramienta de diagnóstico para identificar y solucionar errores comunes en la implementación, configuración y operación en curso de la infraestructura de tejido protegido. Esto incluye el servicio de protección de Host (HGS), todos los hosts protegidos y los servicios de soporte técnico como DNS y Active Directory. La herramienta de diagnóstico se puede usar para realizar un primer paso en la clasificación de un tejido protegido de errores, proporciona a los administradores un punto de partida para resolver las interrupciones e identificar recursos mal configurados. La herramienta no es un reemplazo para un sonido comprensión del funcionamiento de un tejido protegido y sólo sirve para verificar rápidamente los problemas más comunes encontrados durante las operaciones diarias.

Documentación de los cmdlets usados en este tema puede encontrarse en [TechNet](https://technet.microsoft.com/library/mt718834.aspx).

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>Inicio rápido

Puede diagnosticar un host protegido o un nodo HGS mediante una llamada a lo siguiente desde una sesión de Windows PowerShell con privilegios de administrador local:
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
Esto automáticamente detectará el rol del host actual y diagnosticar cualquier problema pertinente que pueda detectarse automáticamente.  Se muestran todos los resultados generados durante este proceso debido a la presencia de la `-Detailed` cambie.

El resto de este tema proporciona un tutorial detallado sobre el uso avanzado de `Get-HgsTrace` para hace cosas como diagnosticar varios hosts al mismo tiempo y detectar errores de configuración entre nodos complejos.

## <a name="diagnostics-overview"></a>Información general sobre diagnóstico
Diagnósticos de tejido protegido están disponibles en cualquier host de máquina virtual blindada relacionados con las herramientas y características instaladas, incluidos hosts que ejecutan Server Core.  Actualmente, diagnósticos se incluyen con los paquetes y las características siguientes:

1. Rol de servicio de protección de host
2. Compatibilidad de Hyper-V con protección de host
3. Herramientas de blindaje de máquinas virtuales para la administración de tejidos
4. Herramientas de administración remota del servidor (RSAT)

Esto significa que las herramientas de diagnóstico estará disponibles en todos los hosts protegidos, HGS nodos, determinados servidores de administración de tejido y las estaciones de trabajo de Windows 10 con [RSAT](https://www.microsoft.com/download/details.aspx?id=45520) instalado.  Diagnósticos se pueden invocar desde cualquiera de las máquinas con la intención de diagnosticar cualquier host protegido o el nodo HGS en un tejido protegido; anteriores uso de destinos de seguimiento remoto, diagnóstico puede localizar y conectarse a los hosts que no sean de la máquina que ejecuta diagnósticos.

Todos los hosts de destino de diagnóstico se conocen como un "destino de seguimiento".  Destinos de seguimiento se identifican por sus nombres de host y los roles.  Roles describen la función que realiza un destino de seguimiento determinado en un tejido protegido.  Actualmente, el seguimiento tiene como destino soporte `HostGuardianService` y `GuardedHost` roles.  Tenga en cuenta es posible que un host ocupar varios roles a la vez y esto también es compatible con diagnósticos, sin embargo, esto no debería realizarse en entornos de producción.  Los hosts de Hyper-V y HGS deben mantenerse separados y distintos en todo momento.

Los administradores pueden comenzar las tareas de diagnóstico mediante la ejecución de `Get-HgsTrace`.  Este comando realiza dos funciones distintas en función de los modificadores proporcionados en tiempo de ejecución: seguimiento de diagnóstico y la colección.  Estos dos combinarlas constituyen la totalidad de la herramienta de diagnóstico de tejido protegido.  Aunque no explícitamente necesario, diagnósticos más útiles requieren los seguimientos que solo se pueden recopilar con credenciales de administrador en el destino de seguimiento.  Si no tiene privilegios suficientes mantenidos por el usuario que ejecuta la recopilación de seguimiento, se producirá un error en los seguimientos que requieren elevación mientras se pasarán todos los demás.  Esto permite que el diagnóstico parcial en caso de que un operador de exceso de privilegios está realizando la evaluación de prioridades. 

### <a name="trace-collection"></a>Colección de seguimiento
De forma predeterminada, `Get-HgsTrace` solo recopilará seguimientos y guardarlos en una carpeta temporal.  Los seguimientos adoptan la forma de una carpeta, con el nombre del host de destino, rellenado con los archivos con formato especial que describen cómo se configura el host.  Los seguimientos también contienen metadatos que describen cómo se han invocado los diagnósticos para recopilar los seguimientos.  Estos datos se usan por los diagnósticos para rehidratar información acerca del host al realizar un diagnóstico más detallado manual.

Si es necesario, los seguimientos se pueden revisar manualmente.  Todos los formatos son cualquier lenguaje natural (XML) o se puede inspeccionar fácilmente mediante las herramientas estándar (por ejemplo, X509 certificados y las extensiones de Shell de criptografía de Windows).  Observe, sin embargo, que los seguimientos no están diseñados para el diagnóstico manual y siempre es más eficaz para procesar los seguimientos con las herramientas de diagnóstico de `Get-HgsTrace`.

Los resultados de la colección de seguimiento de ejecución no hace ninguna indicación sobre el estado de un host determinado.  Simplemente indican que los seguimientos se recopilaron correctamente.  Es necesario usar las funciones de diagnóstico de `Get-HgsTrace` para determinar si los seguimientos de indican un entorno con errores.

Mediante el `-Diagnostic` parámetro, puede limitar la recopilación de seguimiento a solo esos seguimientos necesarias para usar el diagnóstico especificados.  Esto reduce la cantidad de datos recopilados, así como los permisos necesarios para invocar el diagnóstico.

### <a name="diagnosis"></a>Diagnóstico más detallado
Se pueden diagnosticar seguimientos recopilados por siempre `Get-HgsTrace` la ubicación de los seguimientos a través de la `-Path` parámetro y especificar el `-RunDiagnostics` cambie.  Además, `Get-HgsTrace` puede realizar un diagnóstico más detallado y colección en un único paso proporcionando la `-RunDiagnostics` conmutador y una lista de destinos de seguimiento.  Si no se proporciona ningún destino de seguimiento, la máquina actual se utiliza como un destino implícito, con su rol deduce mediante la inspección de los módulos de Windows PowerShell instalados.

Diagnóstico proporcionará los resultados en un formato jerárquico que muestre qué destinos de seguimiento, los conjuntos de diagnóstico y diagnóstico individual es responsable de un error en particular.  Errores incluyen actualizaciones y recomendaciones de resolución si se puede establecer una determinación sobre qué acción debe realizarse siguiente.  De forma predeterminada, se ocultan los resultados irrelevantes y pasando.  Para ver todos los elementos probado por los diagnósticos, especifique el `-Detailed` cambie.  Esto hará que todos los resultados que aparezcan independientemente de su estado.

Es posible restringir el conjunto de diagnósticos que se ejecutan utilizando el `-Diagnostic` parámetro.  Esto le permite especificar qué clases de diagnóstico deben ejecutarse en los destinos de seguimiento y suprimir todos los demás.  Ejemplos de clases de diagnóstico disponibles incluyen redes, prácticas recomendadas y cliente hardware.  Consulte la [documentación del cmdlet](https://technet.microsoft.com/library/mt718831.aspx) para encontrar una lista actualizada de diagnóstico disponible.

> [!WARNING]
> Diagnósticos no son un reemplazo para una supervisión seguros y la canalización de respuesta a incidentes.  Hay un paquete de System Center Operations Manager para supervisar los tejidos protegidos, así como diversos canales de registro de eventos que pueden supervisarse para detectar problemas de tiempo.  Diagnósticos, a continuación, pueden utilizarse para evaluar estos errores y establecer un curso de acción rápidamente.

## <a name="targeting-diagnostics"></a>Diagnósticos de destinatarios

`Get-HgsTrace` funciona para destinos de seguimiento.  Un destino de seguimiento es un objeto que corresponde a un nodo HGS o un host protegido dentro de un tejido protegido.  Se puede considerar como una extensión a una `PSSession` que incluye la información requerida solo por el diagnóstico, como el rol del host en el tejido.  Los destinos pueden ser generado implícitamente (por ejemplo, el diagnóstico local o manual) o explícitamente con la `New-HgsTraceTarget` comando.

### <a name="local-diagnosis"></a>Diagnóstico local

De forma predeterminada, `Get-HgsTrace` tendrá como destino el host local (es decir, donde el cmdlet se invoca).  Esto se conoce como el destino local implícito.  El destino local implícito solo se usa cuando no se proporciona ningún destino en el `-Target` ningún seguimiento preexistente y parámetro se encuentra en el `-Path`.

El destino local implícito usa la inferencia de rol para determinar qué rol desempeña el host actual en el tejido protegido.  Esto se basa en los módulos instalados de Windows PowerShell que corresponden aproximadamente a las características que están instaladas en el sistema.  La presencia de la `HgsServer` módulo hará que el destino de seguimiento tomar el rol `HostGuardianService` y la presencia de la `HgsClient` módulo hará que el destino de seguimiento tomar el rol `GuardedHost`.  Es posible que un host dado que ambos módulos presentes en cuyo caso se tratará como un `HostGuardianService` y un `GuardedHost`.

Por lo tanto, la invocación de forma predeterminada de diagnósticos para recopilar seguimientos de forma local:
```PowerShell
Get-HgsTrace
```
... es equivalente a la siguiente:
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` puede aceptar los destinos a través de la canalización o directamente a través de la `-Target` parámetro.  No hay ninguna diferencia entre los dos operativamente hablando.

### <a name="remote-diagnosis-using-trace-targets"></a>Destinos de seguimiento de diagnóstico remoto utilizando

Es posible diagnosticar remotamente un host mediante la generación de los destinos de seguimiento con información de conexión remota.  Todo lo necesario es el nombre de host y un conjunto de credenciales puede conectarse mediante la comunicación remota de Windows PowerShell.
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
Este ejemplo generará un símbolo del sistema para recopilar las credenciales de usuario remoto y, a continuación, los diagnósticos se ejecutarán con el host remoto en `hgs-01.secure.contoso.com` para completar la recopilación de seguimiento.  Los seguimientos resultantes se descargan en el host local y, a continuación, diagnostican.  Los resultados del diagnóstico se presentan el mismo que al realizar [diagnóstico local](#local-diagnosis).  De forma similar, no es necesario especificar un rol como puede deducirse basándose en los módulos de Windows PowerShell instalados en el sistema remoto.

Diagnóstico remoto utiliza la comunicación remota de Windows PowerShell para todos los accesos al host remoto.  Por lo tanto, es un requisito previo que el destino de seguimiento tiene acceso remoto a Windows PowerShell habilitado (consulte [PSRemoting habilitar](https://technet.microsoft.com/library/hh849694.aspx)) y que el host local está correctamente configuran para iniciar conexiones con el destino.

> [!NOTE]
> En la mayoría de los casos, solo es necesario que el host local formar parte del mismo bosque de Active Directory y que se usa un nombre de host DNS válido.  Si su entorno usa un modelo de federación más complicado o si quiere usar direcciones IP directas para la conectividad, es posible que deba realizar una configuración adicional, como establecer el WinRM [hosts de confianza](https://technet.microsoft.com/library/ff700227.aspx).

Puede comprobar que un destino de seguimiento crea una instancia y correctamente configurado para aceptar las conexiones mediante el uso de la `Test-HgsTraceTarget` cmdlet:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
Este comando devolverá `$True` si y solo si `Get-HgsTrace` sería capaz de establecer una sesión de diagnóstico remota con el destino de seguimiento.  En caso de error, este cmdlet devolverá información de estado importante para solucionar problemas de la conexión de comunicación remota de Windows PowerShell.

#### <a name="implicit-credentials"></a>Credenciales implícitas

Al realizar un diagnóstico remoto de un usuario con privilegios suficientes para conectarse de forma remota en el destino de seguimiento, no es necesario proporcionar credenciales para `New-HgsTraceTarget`.  El `Get-HgsTrace` cmdlet automáticamente volverá a usar las credenciales del usuario que invoca el cmdlet al abrir una conexión.

> [!WARNING]
> Algunas restricciones se aplican a la reutilización de credenciales, especialmente cuando se realiza lo que se conoce como un "segundo salto".  Esto se produce cuando se intenta volver a usar credenciales desde dentro de una sesión remota en otro equipo.  Es necesario [configurar CredSSP](https://technet.microsoft.com/library/hh849872.aspx) admitir este escenario, pero esto está fuera del ámbito de solución de problemas y administración de tejido protegido.

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>Uso de Windows PowerShell Just Enough Administration (JEA) y diagnósticos

Diagnóstico remoto admite el uso de puntos de conexión de JEA con limitaciones de Windows PowerShell. De forma predeterminada, los destinos de seguimiento remoto se conectarán con el valor predeterminado `microsoft.powershell` punto de conexión.  Si el destino de seguimiento tiene la `HostGuardianService` rol, también intentará usar el `microsoft.windows.hgs` punto de conexión que se configura cuando HGS está instalado.

Si desea usar un punto de conexión personalizado, debe especificar el nombre de la configuración de sesión al construir el seguimiento de destino mediante el `-PSSessionConfigurationName` parámetro, como a continuación:

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>Diagnóstico de varios Hosts

Puede pasar varios destinos de seguimiento para `Get-HgsTrace` a la vez.  Esto incluye una combinación de destinos locales y remotos.  Cada destino serán objetos de seguimiento a su vez y, a continuación, los seguimientos de cada destino se diagnostican simultáneamente.  La herramienta de diagnóstico puede utilizar el mayor conocimiento de la implementación para identificar configuraciones incorrectas entre nodos complejos que podrían no estar detectables.  Sólo con esta característica requiere que proporciona los seguimientos de varios hosts al mismo tiempo (en el caso de diagnóstico manual) o varios destinatarios hosts al llamar a `Get-HgsTrace` (en el caso de diagnóstico remoto).

Este es un ejemplo del uso de diagnóstico remoto a un tejido se compone de dos nodos HGS de evaluación de prioridades y dos hosts protegidos, donde uno de los hosts protegidos se emplean para iniciar `Get-HgsTrace`.

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> No es necesario diagnosticar al tejido protegido todo al diagnóstico de varios nodos.  En muchos casos es suficiente incluir todos los nodos que pueden estar implicados en una condición de error determinado.  Esto suele ser un subconjunto de los hosts protegidos y un número de nodos del clúster HGS.

## <a name="manual-diagnosis-using-saved-traces"></a>Diagnóstico manual mediante seguimientos guardados

A veces es posible que desea volver a ejecutar diagnósticos sin recopilar seguimientos de nuevo, o puede que no tenga las credenciales necesarias para que diagnostique de manera remota todos los hosts en el tejido de forma simultánea.  El diagnóstico manual es un mecanismo por el que puede realizar una evaluación de prioridades de fabric entero utilizando `Get-HgsTrace`, pero sin utilizar la colección de seguimiento remoto.

Antes de realizar un diagnóstico más detallado manual, deberá asegurarse de que los administradores de cada host en el tejido que se deben ser evaluado están preparado y dispuesto a ejecutar comandos en su nombre.  Salida de seguimiento de diagnóstico no expone toda la información que se ve generalmente como confidencial, aunque no se corresponde en el usuario para determinar si es seguro exponer esta información a otros usuarios.

> [!NOTE]
> Los seguimientos no son anónimas y mostrar configuración de red, configuración de PKI y otra configuración que a veces se considera información privada.  Por lo tanto, los seguimientos solo deben transmitir a las entidades de confianza dentro de una organización y nunca publicadas.

Pasos para realizar un diagnóstico manual son los siguientes:

1. Solicitud que se ejecutan cada administrador host `Get-HgsTrace` especificando un conocido `-Path` y la lista de diagnósticos que se va a ejecutar en los seguimientos resultantes.  Por ejemplo:

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. Solicitar que cada administrador del host de la carpeta de seguimientos resultante del paquete y lo envíe de nuevo.  Este proceso se puede controlar a través de correo electrónico, a través de recursos compartidos de archivos, o cualquier otro mecanismo según las directivas y procedimientos establecidos por la organización de funcionamiento.

3. Combinar todos los seguimientos recibidos en una sola carpeta, con ningún otro contenido o las carpetas.

    * Por ejemplo, suponga que tenía el envío de los administradores que realiza un seguimiento recopilados de cuatro máquinas denominadas HGS-01, 02 de HGS, RR1N2608-12 y 13 de RR1N2608.  Cada administrador habría enviado una carpeta con el mismo nombre.  Podría ensamblar una estructura de directorios que aparece como sigue:

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

4. Ejecutar diagnósticos, que proporciona la ruta de acceso a la carpeta de seguimiento montado en el `-Path` parámetro y especificar el `-RunDiagnostics` cambiar, así como los diagnósticos para que le pide que los administradores para recopilar seguimientos.  Diagnósticos supondrá no puede obtener acceso a los hosts que se encuentra dentro de la ruta de acceso y, por tanto, se intentará usar solo los seguimientos recopilados previamente.  Si los seguimientos que faltan o están dañados, diagnósticos fallará solo las pruebas afectadas y continuar con normalidad.  Por ejemplo:

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>Mezclar guarda los seguimientos con destinos adicionales

En algunos casos, puede tener un conjunto de seguimientos recopilados previamente que desea aumentar con seguimientos de host adicionales.  Es posible mezclar los seguimientos recopilados previamente con destinos adicionales que se realiza un seguimiento y diagnosticar en una sola llamada de diagnóstico.

Siga las instrucciones para recopilar y montar una carpeta de seguimiento especificada anteriormente, llame a `Get-HgsTrace` con destinos de seguimiento adicionales no se encuentra en la carpeta de seguimiento recopilados previamente:

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

El cmdlet de diagnóstico identificará todos los hosts previamente recopilados y el host adicional uno que todavía se necesita para realizar un seguimiento y llevará a cabo el seguimiento necesario.  A continuación, se le diagnosticar la suma de todos los seguimientos recopilados previamente y recién recopilados.  La carpeta de seguimiento resultante contendrá los seguimientos antiguos y nuevos.
