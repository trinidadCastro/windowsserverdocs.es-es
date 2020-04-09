---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: Administrar la emisión de RID
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: aabb48643c58019339b96e9a4c54df8e1d66893c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823248"
---
# <a name="managing-rid-issuance"></a>Administrar la emisión de RID

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema, se explica el cambio al rol FSMO del maestro de RID. Incluye la nueva funcionalidad de emisión y supervisión del maestro de RID y explica cómo analizar la emisión de RID y solucionar los problemas de la emisión de RID.  
  
-   [Administrar la emisión de RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Solución de problemas de emisión de RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Puede encontrar más información en el [blog de preguntar a servicios](https://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="managing-rid-issuance"></a><a name="BKMK_Manage"></a>Administrar la emisión de RID  
De forma predeterminada, cada dominio tiene capacidad para, aproximadamente, mil millones de entidades de seguridad, como usuarios, grupos y equipos. Obviamente, no hay ningún dominio en el que se utilicen activamente tantos objetos. Sin embargo, el servicio de asistencia al cliente de Microsoft encontró casos en los que:  
  
-   El software de aprovisionamiento o los scripts administrativos acumulan accidentalmente los usuarios, grupos y equipos creados.  
  
-   Los usuarios delegados crearon muchos grupos de distribución y seguridad que no se utilizan.  
  
-   Se disminuyeron de nivel o se restauraron muchos controladores de dominio, o se limpiaron sus metadatos.  
  
-   Se realizaron recuperaciones de bosques.  
  
-   La operación InvalidateRidPool se llevó a cabo con frecuencia.  
  
-   El valor del Registro correspondiente al tamaño de los bloques de RID se incrementó de forma incorrecta.  
  
Todas estas situaciones consumen RID innecesariamente, a menudo por error. Tras muchos años, algunos entornos se quedaron sin RID y esto los obligó a migrar a un nuevo dominio o realizar recuperaciones de bosques.  
  
Windows Server 2012 se ocupa de los problemas relacionados con la asignación de RID que aparecieron con los años y la extensión de Active Directory. Incluyen un mejor registro de eventos, límites más apropiados y la capacidad de en caso de emergencia: doblar el tamaño total del espacio global de RID para un dominio.  
  
### <a name="periodic-consumption-warnings"></a>Advertencias periódicas de consumo  
Windows Server 2012 agrega un seguimiento de eventos relacionados con el espacio global de RID que proporciona advertencias anticipadas cuando se alcanzan los hitos principales. El modelo calcula la marca del diez (10) por ciento utilizado del grupo global y, cuando se llega a esa marca, registra un evento. Luego, calcula el siguiente diez por ciento utilizado del resto y el ciclo de eventos continúa. A medida que se agote el espacio global de RID, los eventos se producirán cada vez con más frecuencia, dado que el diez por ciento se alcanza antes en el grupo si este va disminuyendo (pero la limitación del registro de eventos impedirá que haya más de una entrada por hora). El registro de eventos del sistema de cada controlador de dominio escribirá el evento de advertencia de Directory-Services-SAM 16658.  
  
En un espacio global de RID de 30 bits predeterminado, el primer evento se registrará al asignar el grupo que contenga el RID número 107.374.182<sup></sup>. La frecuencia de los eventos se acelerará, como es natural, hasta el último punto de control, de 100.000, con 110 eventos generados en total. El comportamiento es similar con un espacio global de RID de 31 bits desbloqueado: se inicia en el número 214.748.365 y finaliza en 117 eventos.  
  
> [!IMPORTANT]  
> No se espera que aparezca este evento: si se produce, investiga enseguida los procesos de creación de usuarios, equipos y grupos del dominio. No es muy normal que se creen más de cien millones de objetos de AD DS.  
  
![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Eventos de invalidación de grupos de RID  
Hay nuevas alertas de eventos que indican que se descartó un grupo de RID del controlador de dominio local. Son informativas y no es raro que aparezcan, especialmente con la nueva funcionalidad de controladores de dominio virtuales. Para ver los detalles del evento, consulta la lista de eventos que se incluye a continuación.  
  
### <a name="rid-block-size-limit"></a><a name="BKMK_RIDBlockMaxSize"></a>Límite de tamaño de bloque de RID  
Generalmente, los controladores de dominio solicitan asignaciones de RID en bloques de 500 RID cada uno. Puedes reemplazar esta configuración predeterminada con el siguiente valor de REG_DWORD del Registro de un controlador de dominio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
En las versiones anteriores a Windows Server 2012, no se exigía ningún valor máximo en esa clave del Registro, excepto el máximo implícito de DWORD (que tiene el valor 0xffffffff o 4294967295). Este valor es considerablemente mayor que el espacio global de RID total. A veces, los administradores configuraban por error el tamaño de los bloques de RID con valores que agotaban el espacio global de RID a una velocidad desmesurada.  
  
En Windows Server 2012, no se puede configurar este valor del Registro con un valor mayor de 15.000 decimal (0x3A98 hexadecimal). Así, se evita que se asignen muchos RID sin querer.  
  
Si defines un valor *superior* a 15.000, el valor se tratará como si fuera 15.000 y el controlador de dominio registrará el evento 16653 en el registro de eventos de los Servicios de directorio con cada reinicio hasta que se corrija el valor.  
  
### <a name="global-rid-space-size-unlock"></a><a name="BKMK_GlobalRidSpaceUnlock"></a>Desbloqueo de tamaño de espacio global de RID  
En las versiones anteriores a Windows Server 2012, el espacio global de RID estaba limitado a 2<sup>30</sup> (o 1.073.741.823) RID en total. Cuando se alcanzaba esta cifra, solo se podían crear más SID con una migración del dominio o una recuperación del bosque a un período de tiempo anterior: una recuperación ante desastres, fuera como fuera. Desde Windows Server 2012, se puede desbloquear el bit 2<sup>31</sup> para aumentar el grupo global a 2.147.483.648 RID.  
  
AD DS almacena esta configuración en un atributo especial oculto llamado **SidCompatibilityVersion** del contexto RootDSE de todos los controladores de dominio. Este atributo no se puede leer con ADSIEdit, LDP ni otras herramientas. Para ver un aumento del espacio global de RID, examina el registro de eventos del sistema hasta encontrar el evento de advertencia 16655 de Directory-Services-SAM o utiliza el siguiente comando de Dcdiag:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Si incrementas el grupo global de RID, el grupo disponible cambiará a 2.147.483.647 en lugar de al valor predeterminado de 1.073.741.823. Por ejemplo:  
  
![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Este desbloqueo se diseñó *únicamente* para evitar que se agoten los RID y se debe utilizar *únicamente* si también se exige un límite superior de RID (consulta la siguiente sección). No lo configures “para prevenir” en los entornos en los que queden millones de RID y donde el crecimiento sea lento: podrían aparecer problemas de compatibilidad de aplicaciones con los SID generados desde el grupo de RID desbloqueado.  
>   
> Esta operación de desbloqueo no se puede revertir ni quitar, excepto si el bosque se recupera por completo a copias de seguridad anteriores.  
  
#### <a name="important-caveats"></a>Advertencias importantes  
Los controladores de dominio de Windows Server 2003 y Windows Server 2008 no pueden emitir RID cuando está desbloqueado el 31.º bit del grupo global de RID<sup></sup>. Los controladores de dominio de Windows Server 2008 R2 *pueden* usar RID<sup>de 31 bits</sup> , *pero solo si* tienen instalada la revisión [KB 2642658](https://support.microsoft.com/kb/2642658) . Los controladores de dominio no admitidos y sin la revisión consideran que el grupo global de RID está agotado cuando está desbloqueado.  
  
Ningún nivel funcional del dominio exige esta característica: ten mucho cuidado y asegúrate de que el dominio solo contiene controladores de dominio de Windows Server 2012 o Windows Server 2008 R2 actualizados.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementación del espacio global de RID desbloqueado  
Para desbloquear el grupo de RID con el 31.º bit<sup></sup> después de recibir la alerta de límite superior de RID (más información, a continuación), sigue estos pasos:  
  
1.  Comprueba que el rol de maestro de RID se está ejecutando en un controlador de dominio de Windows Server 2012. Si no es así, transfiérelo a un controlador de dominio de Windows Server 2012.  
  
2.  Ejecuta LDP.exe.  
  
3.  Haz clic en el menú **Conexión** y, luego, haz clic en **Conectar** en el maestro de RID de Windows Server 2012 en el puerto 389. Después, haz clic en **Enlazar** como administrador de dominio.  
  
4.  Haz clic en el menú **Examinar** y en **Modificar**.  
  
5.  Asegúrate de que **DN** está vacío.  
  
6.  En **Editar atributo de entrada**, escriba:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  En **Valores**, escribe:  
  
    ```  
    1  
    ```  
  
8.  Comprueba que está seleccionado **Agregar** en **Operación** y haz clic en **Introducir**. Se actualizará la **Lista de entradas**.  
  
9. Activa las opciones **Sincrónico** y **Extendido** y, luego, haz clic en **Ejecutar**.  
  
    ![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Si finaliza correctamente, la ventana de salida de LDP mostrará:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Confirma que se aumentó el grupo global de RID: examina el registro de eventos del sistema de ese controlador de dominio y busca en él el evento informativo de Directory-Services-SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>Cumplimiento del límite superior de RID  
Para poder aplicar una medida de protección e informar mejor a los administradores, Windows Server 2012 incorpora un límite superior artificial en el intervalo global de RID: un diez (10) por ciento de RID restantes en el espacio global. Cuando se esté en el uno (1) por ciento del límite superior artificial, los controladores de dominio que soliciten grupos de RID escribirán el evento de advertencia de Directory-Services-SAM 16656 en su registro de eventos del sistema. Al alcanzar el límite superior del diez por ciento en el FSMO del maestro de RID, este escribirá el evento de Directory-Services-SAM 16657 en su registro de eventos del sistema y no asignará más grupos de RID hasta que se invalide el límite superior. Así, tendrás que evaluar el estado del maestro de RID del dominio y solucionar la posible asignación descontrolada de RID. Además, esto protege a los dominios para que no agoten todo el espacio de RID.  
  
El límite superior está codificado de forma rígida en el diez por ciento restante del espacio de RID disponible. Es decir, el límite superior se activa cuando el maestro de RID asigna un grupo que incluye el RID correspondiente al noventa (90) por ciento del espacio global de RID.  
  
-   En los dominios predeterminados, el primer punto en el que se desencadena es 2<sup>30</sup>–1 * 0,90 = 966.367.640 (o 107.374.183 RID restantes).  
  
-   En los dominios con espacio de RID desbloqueado de 31 bits, el punto en el que se desencadena es 2<sup>31</sup>–1 * 0,90 = 1.932.735.282 RID (o 214.748.365 RID restantes).  
  
Cuando se desencadena, el maestro de RID configura el atributo de Active Directory **msDS-RIDPoolAllocationEnabled** (nombre común: **ms-DS-RID-Pool-Allocation-Enabled**) como FALSE en el objeto:  
  
CN = RID Manager $, CN = System, DC = *<domain>*  
  
De este modo, se escribe el evento 16657 y se impide que se emitan más bloques de RID a todos los controladores de dominio. Los controladores de dominio seguirán consumiendo los grupos de RID pendientes que ya se emitieron para ellos.  
  
Para quitar el bloque y permitir que continúe la asignación de grupos de RID, configura ese valor como TRUE. En la siguiente asignación de RID que lleve a cabo el maestro de RID, el atributo volverá al valor predeterminado de NOT SET. Después de esto, no habrá más límites superiores y, finalmente, el espacio global de RID se agotará y será necesario realizar una recuperación del bosque o una migración del dominio.  
  
#### <a name="removing-the-ceiling-block"></a>Eliminación del bloque del límite superior  
Para quitar el bloque después de alcanzar el límite superior artificial, sigue estos pasos:  
  
1.  Comprueba que el rol de maestro de RID se está ejecutando en un controlador de dominio de Windows Server 2012. Si no es así, transfiérelo a un controlador de dominio de Windows Server 2012.  
  
2.  Ejecuta LDP.exe.  
  
3.  Haz clic en el menú **Conexión** y, luego, haz clic en *Conectar* en el maestro de RID de Windows Server 2012 en el puerto 389. Después, haz clic en **Enlazar** como administrador de dominio.  
  
4.  Haz clic en el menú **Ver** y, luego, haz clic en **Árbol**. Después, en **DN base**, selecciona el contexto de nomenclatura del dominio propio del maestro de RID. Haga clic en **Aceptar**.  
  
5.  En el panel de navegación, explora en profundidad hasta entrar en el contenedor **CN=System** y haz clic en el objeto **CN=RID Manager$** . Haz clic con el botón secundario en él y, luego, haz clic en **Modificar**.  
  
6.  En Editar atributo de entrada, escribe:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  En **Valores**, escribe (en mayúsculas):  
  
    ```  
    TRUE  
    ```  
  
8.  Selecciona **Reemplazar** en **Operación** y haz clic en **Introducir**. Se actualizará la **Lista de entradas**.  
  
9. Habilita las opciones **Sincrónico** y **Extendido** y, luego, haz clic en **Ejecutar**:  
  
    ![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Si finaliza correctamente, la ventana de salida de LDP mostrará:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Emisión de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Otras correcciones de RID  
Los sistemas operativos de Windows Server anteriores tenían una pérdida de grupos de RID cuando faltaba el atributo rIDSetReferences. Para resolver este problema en los controladores de dominio que ejecutan Windows Server 2008 R2, instale la revisión desde [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemas de RID sin corregir  
Desde hace tiempo, hay una pérdida de RID cuando se produce un error al crear una cuenta: al crear la cuenta, el error sigue consumiendo un RID. El ejemplo habitual consiste en crear un usuario con una contraseña que no cumple los requisitos de complejidad.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correcciones de RID para versiones anteriores de Windows Server  
Para todos los cambios y las correcciones que indicamos, se publicaron revisiones de Windows Server 2008 R2. Actualmente no hay ninguna revisión de Windows Server 2008 planeada ni en curso.  
  
## <a name="troubleshooting-rid-issuance"></a><a name="BKMK_Tshoot"></a>Solución de problemas de emisión de RID  
  
### <a name="introduction-to-troubleshooting"></a>Introducción a la solución de problemas  
La solución de problemas de emisión de RID requiere un método lógico y lineal. A menos que estés supervisando atentamente los registros de eventos de las advertencias y los errores desencadenados por RID, lo más probable es que los primeros indicios que observes de que existe un problema sean errores en la creación de cuentas. La clave para solucionar problemas de emisión de RID es comprender en qué situaciones se espera o no que aparezca un síntoma. Muchos problemas de emisión de RID pueden afectar solamente a un controlador de dominio y no tener nada que ver con las mejoras en los componentes. Este sencillo diagrama sirve para aclarar las decisiones:  
  
![Emisión de RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opciones de solución de problemas  
  
#### <a name="logging-options"></a>Opciones de registro  
Todo el registro de la emisión de RID se produce en el registro de eventos del sistema, en el origen Directory-Services-SAM. De forma predeterminada, el registro está habilitado y configurado con el máximo nivel de detalle. Si no hay entradas registradas correspondientes a los cambios de componentes nuevos de Windows Server 2012, trata el problema como un problema de emisión clásico (es decir, heredado, anterior a Windows Server 2012) observado en Windows 2008 R2 o sistemas operativos anteriores.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilidades y comandos para la solución de problemas  
Para solucionar los problemas que no se expliquen en los registros indicados más arriba (especialmente, los problemas de emisión de RID anteriores), utiliza esta lista de herramientas como punto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Monitor de red 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodología general para la solución de problemas de configuración de controladores de dominio  
  
1.  ¿La causa del error es un problema sencillo de permisos o disponibilidad de los controladores de dominio?  
  
    1.  ¿Estás tratando de crear una entidad de seguridad sin los permisos necesarios? Examina la salida y busca en ella errores de acceso denegado.  
  
    2.  ¿Está disponible un controlador de dominio? Examina los mensajes de disponibilidad del controlador de dominio, LDAP o el error devuelto.  
  
2.  ¿El error devuelto menciona expresamente los RID y es lo suficientemente específico como para poder usarlo de guía? Si es así, sigue las indicaciones.  
  
3.  ¿El error devuelto menciona expresamente los RID pero no es específico? Por ejemplo, “Windows no puede crear el objeto porque el servicio de directorio no pudo asignar un identificador relativo”.  
  
    1.  Examine el registro de eventos del sistema en el controlador de dominio para ver los eventos de RID "heredados" (anteriores a Windows Server 2012) que se detallan en [solicitud de grupo RID](https://technet.microsoft.com/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Examina el evento del sistema del controlador de dominio y el maestro de RID para buscar en él los eventos que indican nuevos bloques y que se detallan más adelante en este tema (16655, 16656 y 16657).  
  
    3.  Valida el mantenimiento de la replicación de Active Directory con Repadmin.exe y la disponibilidad del maestro de RID con **Dcdiag.exe /test:ridmanager /v**. Si estas pruebas no son concluyentes, habilita las capturas de red de doble cara entre el controlador de dominio y el maestro de RID.  
  
### <a name="troubleshooting-specific-problems"></a>Solucionar problemas específicos  
En los controladores de dominio de Windows Server 2012, se registran los siguientes mensajes nuevos en el registro de eventos del sistema. Los sistemas de seguimiento del mantenimiento automatizados de AD, como System Center Operations Manager, deben supervisar estos eventos. Todos son destacables y algunos de ellos señalan problemas críticos en los dominios.  
  
|||  
|-|-|  
|Id. de evento|16653|  
|Origen|Directory-Services-SAM|  
|Severity|advertencia|  
|Mensaje|Un tamaño de grupo para los identificadores de la cuenta (RID) configurado por un administrador es mayor que el máximo admitido. Se usará el valor máximo de %1 cuando el controlador del dominio sea el maestro RID.<p>Para obtener más información, vea el tema sobre [límite de tamaño de los bloques de RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Notas y resolución|Ahora, el valor máximo del tamaño de los bloques de RID es 15000 decimal (3A98 hexadecimal). Un solo controlador de dominio no puede solicitar más de 15.000 RID. Este evento se registra con cada arranque hasta que el valor se configura con un valor que no supere este máximo.|  
  
|||  
|-|-|  
|Id. de evento|16654|  
|Origen|Directory-Services-SAM|  
|Severity|Informativa|  
|Mensaje|Se invalidó un grupo de identificadores de cuenta (RID). Esto puede suceder en los siguientes casos previstos:<p>1. un controlador de dominio se restaura a partir de una copia de seguridad.<p>2. un controlador de dominio que se ejecuta en una máquina virtual se restaura desde una instantánea.<p>3. un administrador invalidó el grupo manualmente.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=226247 para obtener más información.|  
|Notas y resolución|Si este evento no se esperaba, ponte en contacto con los administradores de todos los dominios y averigua cuál de ellos llevó a cabo esta acción. El registro de eventos de los Servicios de directorio también contiene información adicional sobre el momento en el que se realizó uno de estos pasos.|  
  
|||  
|-|-|  
|Id. de evento|16655|  
|Origen|Directory-Services-SAM|  
|Severity|Informativa|  
|Mensaje|El máximo global para identificadores de cuenta (RID) se ha incrementado a %1.|  
|Notas y resolución|Si este evento no se esperaba, ponte en contacto con los administradores de todos los dominios y averigua cuál de ellos llevó a cabo esta acción. Este evento indica que el tamaño total del grupo de RID aumentó hasta superar el valor predeterminado de 2<sup>30</sup>. No aparece de forma automática: solamente por una acción administrativa.|  
  
|||  
|-|-|  
|Id. de evento|16656|  
|Origen|Directory-Services-SAM|  
|Severity|advertencia|  
|Mensaje|El máximo global para identificadores de cuenta (RID) se ha incrementado a %1.|  
|Notas y resolución|Acción requerida. Se asignó un grupo de identificadores de cuenta (RID) a este controlador de dominio. El valor del grupo indica que este dominio consumió una parte considerable del total de identificadores de cuenta disponibles.<p>Se activará un mecanismo de protección cuando el dominio alcance el siguiente umbral de número total de identificadores de cuenta disponibles restantes: %1.  El mecanismo de protección impedirá que se creen más cuentas hasta que vuelvas a habilitar manualmente la asignación de identificadores de cuenta en el controlador de dominio del maestro de RID.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obtener más información.|  
  
|||  
|-|-|  
|Id. de evento|16657|  
|Origen|Directory-Services-SAM|  
|Severity|Error|  
|Mensaje|Acción requerida. Este dominio consumió una parte considerable del total de identificadores de cuenta (RID) disponibles. Se ha activado un mecanismo de protección porque el número total de identificadores de cuenta disponibles restante es inferior a: X% [argumento de techo artificial].<p>El mecanismo de protección impide que se creen más cuentas hasta que vuelvas a habilitar manualmente la asignación de identificadores de cuenta en el controlador de dominio del maestro de RID.<p>Es extremadamente importante que se realicen determinados diagnósticos antes de volver a habilitar la creación de cuentas, para comprobar que este dominio no está consumiendo identificadores de cuenta con una frecuencia inusualmente alta. Todos los problemas que se identifiquen se deben resolver antes de volver a habilitar la creación de cuentas.<p>Si no se diagnostican y se corrigen los problemas subyacentes que provocan el consumo de identificadores de cuenta a una frecuencia inusualmente alta, es posible que se agoten los identificadores de cuenta del dominio. Si esto ocurre, la creación de cuentas quedará deshabilitada permanentemente en este dominio.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obtener más información.|  
|Notas y resolución|Ponte en contacto con los administradores de todos los dominios e infórmales de que no se pueden crear más entidades de seguridad en este dominio hasta que se invalide esta protección. Para más información sobre cómo invalidar la protección y, posiblemente, incrementar el grupo de RID en general, consulta [Desbloqueo del tamaño del espacio global de RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|Id. de evento|16658|  
|Origen|Directory-Services-SAM|  
|Severity|advertencia|  
|Mensaje|Este evento es una actualización periódica sobre la cantidad total restante de identificadores de cuenta disponibles (RID). El número de identificadores de cuenta restantes es aproximadamente: %1.<p>Los identificadores de cuenta se usan a medida que se crean cuentas, cuando se agotan, no se pueden crear nuevas cuentas en el dominio.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228745 para obtener más información.|  
|Notas y resolución|Ponte en contacto con los administradores de todos los dominios e infórmales de que el consumo de RID alcanzó un hito principal. Para averiguar si este comportamiento se esperaba o no, revisa los patrones de creación de elementos de confianza de seguridad. Es muy poco habitual que aparezca este evento: significa que se asignaron, al menos, unos 100 millones de RID.|  
  
## <a name="see-also"></a>Consulta también  
[Administrar la emisión de RID en Windows Server 2012](https://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


