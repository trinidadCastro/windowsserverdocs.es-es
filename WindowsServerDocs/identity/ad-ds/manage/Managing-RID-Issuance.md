---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: "Administración de emisión RID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>Administración de emisión RID

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica el cambio el maestro FSMO RID, incluida la emisión nuevo y supervisar la funcionalidad en el maestro RID y cómo analizar y solucionar problemas de emisión RID.  
  
-   [Administración de emisión RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Solución de problemas de emisión RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Más información está disponible en la [AskDS Blog](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="BKMK_Manage"></a>Administración de emisión RID  
De forma predeterminada, un dominio tiene capacidad de entidades de seguridad de aproximadamente mil millones, como los usuarios, grupos y equipos. Naturalmente, no hay ningún dominio con ese muchos objetos que se usaba activamente. Sin embargo, la asistencia al cliente de Microsoft ha encontrado casos donde:  
  
-   Aprovisionamiento de software o scripts administrativos accidentalmente masiva había creado a los usuarios, grupos y equipos.  
  
-   Muchos de seguridad no usados y grupos de distribución creados por los usuarios delegados  
  
-   Muchos controladores de dominio se degradarán, restaurando, o metadatos limpiar  
  
-   Se realizaron recuperaciones del bosque  
  
-   Se realizó la operación de InvalidateRidPool con frecuencia  
  
-   El valor del registro de tamaño de bloque LIBRARSE aumentaba incorrectamente  
  
Todas estas situaciones usan RID innecesariamente, a menudo por error. Durante muchos años, unas pocos entornos no tiene suficiente RID y esto fuerza a migrar a un dominio nuevo o realizar recuperaciones bosque.  
  
Windows Server 2012 tratan problemas relacionados con la asignación de RID que han quedado solo problemáticas con la edad y universalidad de Active Directory. Estos incluyen un mejor registro de eventos, límites más apropiados y la capacidad de - de emergencia - para duplicar el tamaño general del espacio de RID global para un dominio.  
  
### <a name="periodic-consumption-warnings"></a>Advertencias de consumo periódicas  
Windows Server 2012 agrega global que seguimiento de eventos de espacio RID proporciona advertencias tempranas cuando se cruzan hitos principales. El modelo calcula el porcentaje de diez (10), utiliza la marca en el grupo global y registra un evento cuando llega. A continuación, calcula el porcentaje de diez siguiente utilizado pendiente de y sigue el ciclo de evento. Como se ha agotado el espacio global de RID, eventos acelerará como diez por ciento aciertos con mayor rapidez en un grupo decreciente (pero atenuación de registro de eventos impedirá más de una entrada por hora). El registro de eventos del sistema en cada controlador de dominio escribe eventos de advertencia de servicios de directorio de SAM 16658.  
  
Suponiendo que un predeterminado 30 bits RID espacio global, la primera registros de eventos al asignar el grupo que contiene la 107,374,182<sup>p:</sup> RID. El tipo de evento naturalmente acelera hasta el último punto de control de 100.000, con 110 eventos generados en total. El comportamiento es similar para un espacio RID global de 31 bits desbloqueado: empezando por 214,748,365 y completar de 117 eventos.  
  
> [!IMPORTANT]  
> Este evento no se espera que; investiga el usuario, el equipo y los procesos de creación de grupo inmediatamente en el dominio. Crear objetos de AD DS de más de 100 millones es bastante fuera de lo normal.  
  
![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>ELIMINAR eventos de invalidación de grupo  
Hay nuevos eventos avisa de que se descartó un grupo local de eliminar el controlador de dominio. Estas son informativo y se podrían esperar, especialmente debido a la nueva funcionalidad v CC. Consulta la lista de eventos a continuación para obtener más información sobre el evento.  
  
### <a name="BKMK_RIDBlockMaxSize"></a>ELIMINAR el límite de tamaño de bloque  
Normalmente, un controlador de dominio solicita RID asignaciones en bloques de 500 RID al mismo tiempo. Puedes invalidar este valor predeterminado mediante el siguiente valor REG_DWORD del registro en un controlador de dominio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Antes de Windows Server 2012, no había ningún valor máximo que se aplican en esa clave del registro, excepto el máximo de DWORD implícito (que tiene un valor de 0xffffffff o 4294967295). Este valor es mucho mayor que el espacio total de RID global. Los administradores a veces incorrectamente accidentalmente configuraron o eliminar el tamaño de bloque con valores que agotado RID global a una gran velocidad.  
  
En Windows Server 2012, no puedes establecer este valor del registro superior 15000 (0x3A98 hexadecimal). Esto impide que massive asignación RID no intencionado.  
  
Si estableces el valor *mayor* de 15.000, el valor se trata como 15.000 y el controlador de dominio registra el evento 16653 en el registro de eventos de servicios de directorio en cada reinicio hasta que se corrija el valor.  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>Desbloquear RID global Space Size  
Antes de Windows Server 2012, se limitan a 2 en el espacio global de RID<sup>30</sup> (o 1.073.741.823) total RID. Una vez alcanzado, solo una dominio migración o bosque de recuperación en un período de tiempo anterior permite la creación de SID nuevas - recuperación ante desastres, por cualquier medida. A partir de Windows Server 2012, el 2<sup>31</sup> bits puede desbloquear con el fin de aumentar el grupo global para 2.147.483.648 RID.  
  
AD DS almacena esta configuración en un atributo oculto especial llamado **SidCompatibilityVersion** en el contexto de RootDSE de todos los controladores de dominio. Este atributo no es legible con ADSIEdit, LDP u otras herramientas. Para ver un aumento en el espacio global de RID, examina el registro de eventos del sistema para el evento de advertencia 16655 de SAM de servicios de directorio o usa el siguiente comando Dcdiag:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Si aumentas el conjunto global de RID, el grupo disponible cambiará a 2.147.483.647 en lugar del predeterminado 1.073.741.823. Por ejemplo:  
  
![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Este desbloqueo pretende *solo* para evitar que te quedes sin RID y debe usarse *solo* junto con la aplicación de techo LIBRARSE (consulta la sección siguiente). No "manera preferente" establecer en entornos que tienen millones de restante RID y el crecimiento bajo, como problemas de compatibilidad de aplicaciones potencialmente existen con SID generados desde el conjunto de RID desbloqueado.  
>   
> Esto desbloquear operación no se puede revertir o eliminado, excepto por parte de una recuperación completa del bosque para las copias de seguridad anteriores.  
  
#### <a name="important-caveats"></a>Advertencias importantes  
Windows Server 2003 y controladores de dominio de Windows Server 2008 no pueden emitir RID cuando RID global grupo 31<sup>st</sup> bits se desbloquea. Controladores de dominio de Windows Server 2008 R2 *puede* usar 31<sup>st</sup> bit RID *pero solo si* tienen revisiones [KB 2642658](https://support.microsoft.com/kb/2642658) instalado. Controladores de dominio no compatible y tratan el conjunto global de RID agotado cuando desbloquea.  
  
Esta característica no se aplica por cualquier nivel funcional del dominio; Tenga cuidado excelente que existe solo Windows Server 2012 o controladores de dominio de Windows Server 2008 R2 actualizados en el dominio.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementar espacio LIBRARSE Global desbloqueado  
Para desbloquear el conjunto de RID el 31<sup>st</sup> poco después de recibir el aviso de techo RID (consulta más adelante) realiza los siguientes pasos:  
  
1.  Asegúrate de que el maestro LIBRARSE función se ejecuta en un controlador de dominio de Windows Server 2012. De lo contrario, transferirlo a un controlador de dominio de Windows Server 2012.  
  
2.  Ejecute LDP.exe  
  
3.  Haz clic en el **conexión** menú y haz clic en **conectar** para el patrón de LIBRARSE de Windows Server 2012 en portar 389 y, a continuación, haz clic en **enlazar** como un administrador de dominio.  
  
4.  Haz clic en el **examinar** menú y haz clic en **modificar**.  
  
5.  Asegúrate de que **DN** está en blanco.  
  
6.  En **editar el atributo de entrada**, escribe:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  En **valores**, escribe:  
  
    ```  
    1  
    ```  
  
8.  Asegúrate de que **agregar** esté seleccionado en **operación** y haz clic en **ENTRAR**. Esto actualiza la **lista de entradas**.  
  
9. Selecciona el **sincrónico** y **Extended** opciones, a continuación, haz clic en **ejecutar**.  
  
    ![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Si se realiza correctamente, la LDP había salida ventana de muestra:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Confirma el conjunto global de RID aumentado examinando el registro de eventos del sistema en el controlador de dominio para el evento Informational de servicios de directorio de SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>ELIMINAR la aplicación techo  
Para permitir una medida de protección y elevar reconocimiento administrativa, Windows Server 2012 presenta un límite máximo artificial en el rango RID global a diez (10) % restante RID en el espacio global. Dentro de uno (1) % del límite máximo artificial, controladores de dominio solicitar grupos RID escriben eventos de advertencia de servicios de directorio de SAM 16656 su registro de eventos del sistema. Cuando se alcance el límite máximo de diez por ciento en el FSMO maestro LIBRARSE, escribe eventos SAM de servicios de directorio 16657 su registro de eventos del sistema y no asignará más grupos RID hasta el límite máximo de invalidación. Esto obliga a evaluar el estado de maestro de RID en el dominio y solucionar los posible asignación RID descontrolado; También se protege dominios de agotar todo el espacio RID.  
  
Este límite está codificado de forma rígida en 10% restante del RID espacio disponible. Es decir, el techo activa cuando el maestro RID asigna un conjunto que incluya el RID correspondientes a noventa (90) por ciento del espacio de RID global.  
  
-   Para los dominios de manera predeterminada, el primer punto de desencadenador es 2<sup>30</sup>-1 * 0,90 = 966,367,640 (o 107,374,183 RID restante).  
  
-   Para los dominios con un espacio RID 31 bits desbloqueado, el punto de desencadenador es 2<sup>31</sup>-1 * 0,90 = 1,932,735,282 RID (o 214,748,365 RID restante).  
  
Cuando se active, el maestro RID establece el atributo de Active Directory **msDS RIDPoolAllocationEnabled** (nombre común **ms-DS-RID-Pool-Allocation-Enabled**) "false" en el objeto:  
  
CN = Administrador RID $, CN = sistema, DC =*<domain>*  
  
Esto escribe el evento 16657 y se impide que la emisión de bloque RID en todos los controladores de dominio. Controladores de dominio seguirán consumiendo cualquier grupos RID pendientes ya emitidos a ellos.  
  
Para quitar el bloque y permitir que la asignación de bloque RID continuar, establece ese valor en TRUE. En la siguiente asignación RID realizada el maestro RID, el atributo volverá a su valor predeterminado no conjunto valor. Después, no hay ninguna límites más y más adelante, en el espacio global de RID acabe, la necesidad de migración de recuperación o dominio del bosque.  
  
#### <a name="removing-the-ceiling-block"></a>Quitar el límite máximo bloque  
Para quitar el bloque de una vez que alcanzan el techo artificial, realiza los siguientes pasos:  
  
1.  Asegúrate de que el maestro LIBRARSE función se ejecuta en un controlador de dominio de Windows Server 2012. De lo contrario, transferirlo a un controlador de dominio de Windows Server 2012.  
  
2.  Ejecute LDP.exe.  
  
3.  Haz clic en el **conexión** menú y haz clic en *conectar* para el patrón de LIBRARSE de Windows Server 2012 en portar 389 y, a continuación, haz clic en **enlazar** como un administrador de dominio.  
  
4.  Haz clic en el **vista** menú y haz clic en **árbol**, después de la **DN Base** seleccionar el contexto de nomenclatura de eliminar del patrón propio dominio. Haz clic en **Aceptar**.  
  
5.  En el panel de navegación, profundizar en el **CN = sistema** contenedor y haz clic en el **CN = Administrador RID $** objeto. Derecho haz clic en ella y haz clic en **modificar**.  
  
6.  En el atributo de entrada editar, escribe:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  En **valores**, tipo (en mayúsculas):  
  
    ```  
    TRUE  
    ```  
  
8.  Selecciona **reemplazar** en **operación** y haz clic en **ENTRAR**. Esto actualiza la **lista de entradas**.  
  
9. Habilitar la **sincrónico** y **Extended** opciones, a continuación, haz clic en **ejecutar**:  
  
    ![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Si se realiza correctamente, la LDP había salida ventana de muestra:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![DESHACERSE de emisión](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Otras correcciones RID  
Sistemas operativos de servidor de Windows anteriores tenía un conjunto de RID perder cuando falta el atributo rIDSetReferences. Para resolver este problema en los controladores de dominio que ejecutan Windows Server 2008 R2, instalar la revisión de [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemas de tipo unfixed RID  
Históricamente ha habido una pérdida de RID en caso de error de creación de cuenta; al crear una cuenta, error todavía usa arriba un RID. El ejemplo común es crear un usuario con una contraseña que no se cumpla la complejidad.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>ELIMINAR correcciones para versiones anteriores de Windows Server  
Todas las correcciones y cambios anteriores tienen las revisiones de Windows Server 2008 R2 publicadas. Actualmente no hay ninguna revisión de Windows Server 2008 planeada o en curso.  
  
## <a name="BKMK_Tshoot"></a>Solución de problemas de emisión RID  
  
### <a name="introduction-to-troubleshooting"></a>Introducción a la solución de problemas  
Solución de problemas de emisión DESHACERSE requiere un método lógico y lineal. A menos que se están supervisando los registros de eventos cuidadosamente para errores y advertencias RID desencadenadas, las primera indicaciones de un problema tienen probabilidades de ser creaciones de cuentas error. La clave para solucionar problemas de emisión RID es comprender cuándo se esperan que los síntomas o no. muchos problemas de emisión RID pueden afectar a un único controlador de dominio y tienen nada que ver con las mejoras de componente. Este diagrama simple siguiente te ayuda a tomar las decisiones más clara:  
  
![DESHACERSE de emisión](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opciones de solución de problemas  
  
#### <a name="logging-options"></a>Opciones de registro  
Inicien sesión RID emisión se produce en el registro de eventos de sistema en SAM de servicios de directorio de origen. Registro está habilitado y configurado para el máximo nivel de detalle, de manera predeterminada. Si no hay entradas se registran para que los cambios de componente de nuevo en Windows Server 2012, considera el problema como un clásico (también conocido como heredados, anteriores a Windows Server 2012) problema de emisión RID visto en Windows 2008 R2 o sistemas operativos anteriores.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilidades y los comandos para la solución de problemas  
Para solucionar problemas que no se explica en los registros mencionados anteriormente - problemas de emisión RID especialmente anteriores - usa la siguiente lista de herramientas como punto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 del Monitor de red  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodología general para solucionar problemas de configuración del controlador de dominio  
  
1.  ¿Es el error causado por un simple problema de disponibilidad del controlador de dominio o de permisos?  
  
    1.  ¿Se intenta crear una seguridad principal sin los permisos necesarios? Examina la salida de errores de acceso denegado.  
  
    2.  ¿Es un controlador de dominio disponible? Examine los mensajes de disponibilidad de controlador de dominio o LDAP y error devueltos.  
  
2.  ¿El error devuelto específicamente mencionar RID y es lo suficientemente específico para usar como guía? Si es así, sigue las instrucciones.  
  
3.  ¿El error devuelto específicamente menciona RID pero es lo contrario no específico? Por ejemplo, "Windows no puede crear el objeto porque el servicio de directorio no pudo asignar un identificador relativo".  
  
    1.  Examina el registro de eventos de sistema en el controlador de dominio para "heredado" (anterior a Windows Server 2012) RID eventos detallan en [LIBRARSE grupo solicitar](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Examina el evento del sistema en el controlador de dominio y el patrón de LIBRARSE de nuevos eventos que indica el bloque que se detallan a continuación, en este tema (16655, 16656, 16657).  
  
    3.  Validar el estado de replicación de Active Directory Repadmin.exe y maestro eliminar disponibilidad con **Dcdiag.exe /test:ridmanager /v**. Habilitar capturas de red a doble cara entre el controlador de dominio y el maestro eliminar si estas pruebas son muy concretos.  
  
### <a name="troubleshooting-specific-problems"></a>Solucionar problemas específicos  
Los siguientes mensajes nuevos iniciar sesión en el registro de eventos del sistema en los controladores de dominio de Windows Server 2012. Automatizadas salud de AD sistemas, por ejemplo, System Center Operations Manager, de seguimiento debe controlar estos eventos; todos son más importantes, y otros indicadores de problemas críticos de dominio.  
  
|||  
|-|-|  
|Identificador de evento|16653|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Advertencia|  
|Mensaje|Un tamaño de un grupo de identificadores de cuenta (RID) que se ha configurado un administrador es mayor que el máximo admitido. El valor máximo de %1 se usará cuando el controlador de dominio es el maestro RID.<br /><br />Para obtener más información, consulta [eliminar el límite de tamaño de bloque](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Notas y resolución|El valor máximo para el tamaño de bloque LIBRARSE ahora es 15000 decimal (3A98 hexadecimal). Un controlador de dominio no puede solicitar más de 15.000 RID. Este registros de eventos en cada inicio hasta que el valor se establece en un valor igual o inferior a este máximo.|  
  
|||  
|-|-|  
|Identificador de evento|16654|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Informativo|  
|Mensaje|Se ha invalidado un conjunto de identificadores de cuenta (RID). Esto puede ocurrir en los siguientes casos esperados:<br /><br />1. un controlador de dominio se restaura desde la copia de seguridad.<br /><br />2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde la instantánea.<br /><br />3. un administrador ha invalidado manualmente el grupo.<br /><br />Consulta https://go.microsoft.com/fwlink/?LinkId=226247 para obtener más información.|  
|Notas y resolución|Si este evento es inesperado, ponte en contacto con todos los administradores de dominio y determinar cuál de ellos realiza la acción. El registro de eventos de servicios de directorio también contiene más información sobre cuando alguno de estos pasos se realizó.|  
  
|||  
|-|-|  
|Identificador de evento|16655|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Informativo|  
|Mensaje|El número de máximo global para identificadores de cuenta (RID) se ha aumentado a %1.|  
|Notas y resolución|Si este evento es inesperado, ponte en contacto con todos los administradores de dominio y determinar cuál de ellos realiza la acción. Este evento notas aumenta el número de RID general tamaño máximo predeterminado de 2 de grupo<sup>30</sup>y no se realizará automáticamente; solo por las acciones administrativas.|  
  
|||  
|-|-|  
|Identificador de evento|16656|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Advertencia|  
|Mensaje|El número de máximo global para identificadores de cuenta (RID) se ha aumentado a %1.|  
|Notas y resolución|¡Acción necesaria! Este controlador de dominio se ha asignado un grupo de identificador de cuenta (RID). El valor de grupo se indica en este dominio ha consumido una parte importante de los identificadores de cuenta disponibles totales.<br /><br />Un mecanismo de protección se activará cuando el dominio alcanza el umbral siguiente del totales disponibles-identificadores de cuenta restante: %1.  El mecanismo de protección impedirá la creación de cuentas hasta manualmente volver a habilitar la asignación de identificadores de cuenta en el controlador de dominio principal RID.<br /><br />Consulta https://go.microsoft.com/fwlink/?LinkId=228610 para obtener más información.|  
  
|||  
|-|-|  
|Identificador de evento|16657|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Error|  
|Mensaje|¡Acción necesaria! En este dominio ha consumido una parte importante de la cuenta-identificadores disponibles totales (RID). Un mecanismo de protección se ha activado porque es el totales disponibles-identificadores de cuenta restante inferior: X % [argumento techo artificial].<br /><br />El mecanismo de protección impide la creación de cuenta hasta que vuelva a habilita manualmente asignación de identificadores de cuenta en el controlador de dominio principal RID.<br /><br />Es muy importante que se llevan a cabo ciertas diagnósticos antes de volver a habilitar la cuenta de creación para asegurarte de este dominio no está consumiendo identificadores de cuenta a una velocidad excesiva. Cualquier problema identificado debe resolverse antes de volver a habilitar la creación de cuentas.<br /><br />Agotamiento de identificador de la cuenta en el dominio después de que la creación de cuentas se deshabilitará permanentemente en este dominio pueden provocar errores para diagnosticar y solucionar cualquier problema subyacente causando una excesiva tasa de consumo de identificadores de cuenta.<br /><br />Consulta https://go.microsoft.com/fwlink/?LinkId=228610 para obtener más información.|  
|Notas y resolución|Ponte en contacto con todos los administradores de dominio e informarles de que no se pueden crear ningún entidades de seguridad adicionales en este dominio hasta que se reemplaza esta protección. Para obtener más información acerca de cómo invalidar la protección y posiblemente aumentar el RID general del grupo, consulta [Global LIBRARSE espacio tamaño de desbloqueo](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|Identificador de evento|16658|  
|Origen|Servicios de directorio de SAM|  
|Gravedad|Advertencia|  
|Mensaje|Este evento es una actualización periódica en la cantidad total restante de identificadores de cuenta disponibles (RID). El número de identificadores de cuenta restantes es aproximadamente: %1.<br /><br />Identificadores de cuenta se usan cuando se crean cuentas, cuando estén agotados ninguna cuenta nueva que puede crearse en el dominio.<br /><br />Consulta https://go.microsoft.com/fwlink/?LinkId=228745 para obtener más información.|  
|Notas y resolución|Ponte en contacto con todos los administradores de dominio e informarles de que el consumo de RID ha cruzado un hito importante; determinar si este es el comportamiento esperado o no revisando patrones de creación de confianza de seguridad. Para alguna vez ves este evento sería muy poco común, ya que significa que al menos ~ 100 millones RID se han asignado.|  
  
## <a name="see-also"></a>Consulta también  
[Administración de emisión RID en Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


