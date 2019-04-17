---
title: "Recuperación de bosque de AD - recuperación de Windows Server 2003"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: daebbc65b9864554fde839c3865f507ab787e6b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---windows-server-2003-recovery"></a>Recuperación de bosque de AD - recuperación de Windows Server 2003

>Se aplica a: Windows Server 2003

En este tema incluye procedimientos de recuperación de bosque para controladores de dominio (DC) que ejecutan Windows Server 2003. El proceso general para la recuperación de bosque no es diferente con controladores de dominio de Windows Server 2003, pero pueden diferir procedimientos específicos debido a las herramientas diferentes. Por ejemplo, Ntdsutil.exe puede usarse para copias de seguridad y restaurar los controladores de dominio que se ejecutan los controladores de dominio de Windows Server 2003, mientras que la copia de seguridad de Windows Server o Wbadmin.exe se usa para controladores de dominio que ejecutan Windows Server 2008 o posterior.  
  
-   [Copia los datos de estado del sistema](#Backing-up-the-System-State-data)  
  
-   [Realizar una restauración no autorizada](#Performing-a-nonauthoritative restore)  
  
-   [Instalar y configurar el servicio de servidor DNS](#Install-and-configure-the-DNS-Server-service)  
  
  
## <a name="backing-up-the-system-state-data"></a>Copia los datos de estado del sistema  
 Usa el siguiente procedimiento para hacer copia de seguridad de los datos de estado del sistema, junto con cualquier otro dato que seleccionaste para la operación de copia de seguridad actual, de un controlador de dominio que ejecuta Windows Server 2003. Windows Server 2003 incluye la herramienta Ntbackup, que puedes usar para hacer copia de seguridad de los datos de estado del sistema.  
  
 Pertenencia a **administradores** o **operadores de copia de seguridad**, o equivalente, es lo mínimo necesario para hacer copia de seguridad de archivos y carpetas.   
  
 Si se copia los datos de estado del sistema en una cinta y el programa copia de seguridad indica que hay un medio no utilizado, es posible que tienes que usar almacenamiento extraíble. Esto agrega la cinta al grupo de medios libre para que pueda utilizarlo con esa copia de seguridad.  
  
 Solo puede hacer la copia de seguridad de los datos de estado del sistema en un equipo local. Puede realizar la en un equipo remoto.  
  
### <a name="to-back-up-the-system-state-data-on-a-domain-controller-that-runs-windows-server-2003"></a>Para hacer copia de seguridad de los datos de estado del sistema en un controlador de dominio que ejecute Windows Server 2003  
  
1.  Haz clic en **inicio**, elija **todos los programas**, elija **Accesorios**, apunta a **herramientas del sistema**y, a continuación, haz clic en **copia de seguridad**.  
  
2.  En la **bienvenida** página, haz clic en **modo avanzado**.  
  
3.  En la **copia de seguridad**, seleccione la casilla de verificación de cualquier unidad, carpeta o archivo que desees hacer una copia.  
  
4.  Selecciona el **estado del sistema** casilla de verificación.  
  
5.  Haz clic en **iniciar copia de seguridad**.  
  
  
## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autorizada  
 Usa el siguiente procedimiento para realizar una restauración no autorizada de un controlador de dominio que ejecuta Windows Server 2003. Al realizar una restauración no autorizada de Active Directory en Windows Server 2003, realizan automáticamente una restauración no autorizada de SYSVOL. No hay pasos adicionales son necesarios.  
  
> [!NOTE]
>  Si también está reinstalando el sistema operativo Windows Server 2003, puede o no se puede unir el equipo al dominio y puede dar cualquier nombre en el equipo durante la instalación del sistema operativo. No instales Active Directory. Después de reinstalar el sistema operativo, ve directamente al paso 4.  
  
 En los controladores de dominio de Windows Server 2003 donde ha restaurado datos de estado del sistema, debes volver a instalar las aplicaciones de software que se ejecutaban en controladores de dominio antes de recuperación. Restauración de AD DS en el primer controlador de dominio en el dominio también restaura el registro, porque son parte de los datos de estado del sistema. Ten esto en cuenta si tenías las aplicaciones que se ejecutan en estos controladores de dominio y que tenían toda la información almacenada en el registro.  
  
 Para ahorrar tiempo necesario para volver a instalar software, determinar si las aplicaciones que deben instalarse en los controladores de dominio son compatibles con la clonación de DC virtual. Tales aplicaciones se pueden instalar en el DC de origen antes de clonación con el fin de ahorrar el tiempo y esfuerzo necesario para instalarlos en los controladores de dominio virtuales clonados.  
  
#### <a name="to-perform-a-nonauthoritative-restore"></a>Para realizar una restauración no autorizada  
  
1.  Después de iniciar el controlador de dominio, presiona F8 para reiniciar el equipo en modo de restauración de servicios de directorio (DSRM).  
  
2.  Selecciona **modo de restauración de servicios de directorio (solo controladores de dominio de Windows)**.  
  
3.  Selecciona el sistema operativo que quieres iniciar en modo de restauración.  
  
4.  Inicia sesión como administrador (sólo se pueden utilizar una cuenta de equipo local, ninguna opción de inicio de sesión de dominio está disponible).  
  
5.  En un símbolo del sistema, escribe **ntbackup**, y, a continuación, presione ENTRAR.  
  
6.  En la **bienvenida** página, haz clic en **modo avanzado**y, a continuación, selecciona el **restaurar y administrar medios** ficha (no selecciones **Asistente para restaurar**.)  
  
7.  Selecciona el archivo de copia de seguridad adecuado para restaurar desde y asegúrate de que la **disco del sistema** y **estado del sistema** casillas están activadas.  
  
8.  Haz clic en **Iniciar restauración**.  
  
9. Una vez completada la operación de restauración, reinicie el equipo.  
  
 Usa el siguiente procedimiento para realizar una restauración (también conocida como principal) de SYSVOL en un controlador de dominio que ejecuta Windows Server 2003. Realizar este procedimiento solo en el primer controlador de dominio de Windows Server 2003a la que se restaura en el dominio.  
  
### <a name="to-perform-an-authoritative-restore-of-sysvol"></a>Para realizar una restauración de SYSVOL  
  
1.  Realiza los pasos 1a 8 en el procedimiento anterior.  
  
2.  En la **Confirmar restauración** cuadro de diálogo, haz clic en **avanzadas**.  
  
3.  Para realizar una restauración de SYSVOL, selecciona la casilla de verificación **al restaurar conjuntos de datos replicados, marcar los datos restaurados como los datos principales para todas las réplicas**.  
  
    > [!NOTE]
    >  Marcar los datos restaurados como los datos principales de la copia de seguridad están equivalentes a establecer el **BurFlags** entrada a D4 bajo la siguiente subclave del registro:  
    >   
    >  **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NtFrs\Parameters\Cumulative réplica Sets\\***GUID*  
  
4.  Una vez completada la operación de restauración, reinicie el equipo.  
  
 
## <a name="install-and-configure-the-dns-server-service"></a>Instalar y configurar el servicio de servidor DNS  
 Si el controlador de dominio que restaura desde la copia de seguridad ejecuta Windows Server 2003, puede instalar el servidor DNS sin necesidad de conectar el controlador de dominio a una red.  
  
### <a name="to-install-and-configure-the-dns-server-service"></a>Para instalar y configurar el servicio de servidor DNS  
  
1.  Abre el Asistente de componentes de Windows. Para abrir al asistente:  
  
    -   Haz clic en **inicio**, haz clic en **Panel de Control**y, a continuación, haz clic en **agregar o quitar programas**.  
  
    -   Haz clic en **agregar o quitar componentes de Windows**.  
  
2.  En **componentes**, selecciona el **servicios de red** y, a continuación, haz clic en **detalles**.  
  
3.  En **subcomponentes de servicios de red**, selecciona el **sistema de nombres de dominio (DNS)** casilla de verificación, haz clic en **Aceptar**y, a continuación, haz clic en **siguiente**.  
  
4.  Si se le pide, en **copiar archivos desde**, escribe la ruta de acceso completa de los archivos de distribución y, a continuación, haz clic en **Aceptar**.  
  
     Tras la instalación, completa los siguientes pasos para configurar el servidor DNS.  
  
5.  Haz clic en **inicio**, elija **todos los programas**, elija **herramientas administrativas**y, a continuación, haz clic en **DNS**.  
  
6.  Crear las zonas DNS para los mismos nombres de dominio DNS que se hospedan en los servidores DNS antes de la crítica error de funcionamiento. Para obtener más información, vea Agregar una zona de búsqueda hacia delante ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).  
  
7.  Configurar los datos DNS, tal y como era antes de la crítica error de funcionamiento. Por ejemplo:  
  
    -   Configurar las zonas DNS almacenarse en AD DS. Para obtener más información, vea Cambiar el tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).  
  
    -   Configurar la zona DNS que está autorizada para los registros de recursos de ubicador DC de controlador de dominio permitir que la actualización dinámica segura. Para obtener más información, consulta Permitir solo seguro actualizaciones dinámicas ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).  
  
8.  Asegúrese de que la zona DNS principal contiene registros de recursos de la delegación (nombre del servidor (NS) y pegar recursos de host (A) registros) para la zona secundaria que está hospedado en este servidor DNS. Para obtener más información, consulta crear una delegación de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).  
  
9. Después de configurar DNS, en el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
     **Net stop netlogon**  
  
10. Escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
     **Comando Net start netlogon**  
  
    > [!NOTE]
    >  Net Logon registrará los registros de recursos de ubicador de DC en DNS para este controlador de dominio. Si vas a instalar el servicio de servidor DNS en un servidor en el dominio secundario, este controlador de dominio no podrá registrar sus registros de inmediato. Esto es porque está aislado en la actualidad como parte del proceso de recuperación y de su servidor DNS principal es el servidor DNS de raíz de bosque. Configurar el equipo con la misma dirección IP que tenía antes del desastre para evitar errores de búsqueda del servicio de controlador de dominio.

## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
