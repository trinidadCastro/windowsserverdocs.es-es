---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planear la ubicación del rol de maestro de operaciones
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870896"
---
# <a name="planning-operations-master-role-placement"></a>Planear la ubicación del rol de maestro de operaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) admite la replicación con varios maestros de datos de directorio, lo que significa que cualquier controlador de dominio puede aceptar los cambios de directorio y replicar los cambios en todos los demás controladores de dominio. Sin embargo, ciertos cambios, como las modificaciones de esquema son imposibles de realizar de forma con varios maestros. Por este motivo ciertos controladores de dominio, conocidos como maestros de operaciones, mantenga roles responsable de aceptar las solicitudes de ciertos cambios específicos.  
  
> [!NOTE]  
> Los titulares de función de maestro de operaciones deben ser capaces de escribir información en la base de datos de Active Directory. Dada la naturaleza de solo lectura de la base de datos de Active Directory en un controlador de dominio de solo lectura (RODC), **los RODC no pueden actuar como los titulares de función de maestro de operaciones**.  
  
Roles de maestro de operaciones de tres (operaciones de maestro únicas también conocido como flexible o FSMO) existen en cada dominio:  
  
- El maestro de operaciones de emulador PDC (controlador) de dominio principal procesa todas las actualizaciones de contraseña.  

- El maestro de operaciones de Id. (RID) relativo mantiene el grupo RID global para el dominio y asigna grupos de RID locales en todos los controladores de dominio para asegurarse de que todas las entidades de seguridad creadas en el dominio tienen un identificador único.  
- El maestro de operaciones de infraestructura para un determinado dominio mantiene una lista de las entidades de seguridad de otros dominios que son miembros de grupos dentro de su dominio.  

Además de los roles de maestro de operaciones de nivel de dominio tres roles de maestro de operaciones de dos existen en cada bosque:  
  
- El maestro de operaciones de esquema rige los cambios del esquema.  
- El maestro de operaciones de nomenclatura de dominio agrega y quita otras particiones de directorio (por ejemplo, las particiones de aplicación de sistema de nombres de dominio (DNS)) y dominios a y desde el bosque.  
  
Coloque los controladores de dominio hospeda estos roles de maestro de operaciones en las áreas donde la confiabilidad de la red es alta y asegúrese de que el emulador de PDC y el maestro de RID están disponibles de forma coherente.  
  
Los titulares de función de maestro de operaciones se asignan automáticamente cuando se crea el primer controlador de dominio en un dominio determinado. Los dos roles de nivel de bosque (maestro de esquema y maestro de nombres de dominio) se asignan al primer controlador de dominio creado en un bosque. Además, los tres roles de nivel de dominio (emulador de PDC, maestro de infraestructura y maestro de RID) se asignan al primer controlador de dominio creado en un dominio.  
  
> [!NOTE]  
> Asignaciones de titular de la función de maestro de operaciones automáticas se realizan solo cuando se crea un nuevo dominio y cuando se degrada un titular de la función actual. Todos los demás cambios a los propietarios del rol deben ser iniciado por un administrador.  
  
Estas asignaciones de roles de maestro de operaciones automáticas pueden provocar un uso de CPU muy alto en el primer controlador de dominio creado en el bosque o dominio. Para evitar este problema, asignar operaciones (transferencia) roles de maestro a varios controladores de dominio en su dominio o bosque. Coloque los controladores de dominio que funciones en las áreas donde la red es confiable del maestro de operaciones de host y donde se pueden tener acceso a los maestros de operaciones por todos los otros controladores de dominio del bosque.  
  
También deberá designar las operaciones de espera (alternativo) funciones de maestro de patrones para todas las operaciones. Los maestros de operaciones en modo de espera son controladores de dominio al que pudo transferir los roles de maestro de operaciones en caso de los titulares de función original producirá un error. Asegúrese de que los maestros de operaciones en modo de espera son asociados de replicación directa de los maestros de operaciones real.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planear la ubicación del emulador PDC

El emulador de PDC procesa los cambios de contraseña de cliente. Solo un controlador de dominio actúa como el emulador de PDC en cada dominio del bosque.  
  
Incluso si se actualizan todos los controladores de dominio a Windows 2000, Windows Server 2003 y Windows Server 2008 y el dominio está funcionando en el nivel funcional de Windows 2000 nativo, el emulador de PDC recibe replicación preferente de los cambios de contraseña realizados otros controladores de dominio en el dominio. Si una contraseña se cambió recientemente, ese cambio tiene tiempo para replicarse en cada controlador de dominio en el dominio. Si se produce un error en la autenticación de inicio de sesión en otro controlador de dominio debido a una contraseña incorrecta, ese controlador de dominio reenvía la solicitud de autenticación al emulador de PDC antes de decidir si aceptar o rechazar el intento de inicio de sesión.  
  
Coloque el emulador de PDC en una ubicación que contiene un gran número de usuarios de ese dominio contraseña reenvío operaciones si es necesario. Además, asegúrese de que la ubicación también está conectada a otras ubicaciones para minimizar la latencia de replicación.  
  
Para que una hoja de cálculo que le ayudarán a documentar la información sobre donde piensa colocar los emuladores PDC y el número de usuarios para cada dominio que se representa en cada ubicación, consulte trabajo ayudas para Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abrir ubicación del controlador de dominio (DSSTOPO_4.doc).  
  
Debe hacer referencia a la información acerca de las ubicaciones en el que tiene que colocar los emuladores PDC al implementar dominios regionales. Para obtener más información sobre cómo implementar dominios regionales, vea [implementar Windows Server 2008 dominios regionales](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para la colocación de maestro de infraestructura  

El maestro de infraestructura actualiza los nombres de las entidades de seguridad de otros dominios que se agregan a grupos en su propio dominio. Por ejemplo, si un usuario de un dominio es un miembro de un grupo en un segundo dominio y se cambia el nombre del usuario en el primer dominio, el segundo dominio no se notifica que se debe actualizar el nombre del usuario en la lista de miembros del grupo. Dado que los controladores de dominio en un dominio no replican a las entidades de seguridad para controladores de dominio en otro dominio, el segundo dominio nunca llega a ser consciente del cambio en la ausencia del maestro de infraestructura.  
  
El maestro de infraestructura constantemente monitores pertenencias a grupos, busca las entidades de seguridad de otros dominios. Si encuentra uno, comprueba con el dominio de la entidad de seguridad para comprobar que la información se actualiza. Si la información no está actualizada, el maestro de infraestructura realiza la actualización y, a continuación, replica el cambio en los otros controladores de dominio en su dominio.  
  
Se aplican dos excepciones a esta regla. En primer lugar, si todos los controladores de dominio son servidores de catálogo global, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante, porque los catálogos globales de replican la información actualizada independientemente del dominio al que pertenecen. En segundo lugar, si el bosque tiene un único dominio, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante, ya que no existen entidades de seguridad de otros dominios.  
  
No coloque al maestro de infraestructura en un controlador de dominio que también es un servidor de catálogo global. Si el maestro de infraestructura y el catálogo global se encuentran en el mismo controlador de dominio, el maestro de infraestructura no funcionará. El maestro de infraestructura no encontrará nunca datos que no están actualizados; por lo tanto, nunca se replicarán los cambios a otros controladores de dominio en el dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Selección de ubicación para las redes del maestro de operaciones con conectividad limitada

Tenga en cuenta que si su entorno tiene una ubicación central o sitio del concentrador en el que puede colocar los titulares de función de maestro de operaciones, determinadas operaciones del controlador de dominio que dependen de la disponibilidad de esas operaciones maestro podrían afectar a los titulares de función.  
  
Por ejemplo, suponga que una organización crea sitios A, B, C, y vínculos a sitios D. existen entre A y B, entre B y C y entre C y D. conectividad de red refleja exactamente la conectividad de red de los vínculos de sitios. En este ejemplo, todas las operaciones de roles de maestro se colocan en el sitio A y la opción **enlazar todos los vínculos de sitio** no está seleccionada.  
  
Aunque esta configuración da como resultado una replicación correcta entre todos los sitios, las funciones de la función de maestro de operaciones tienen las siguientes limitaciones:  
  
- Controladores de dominio en sitios de C y D no tiene acceso el emulador de PDC en el sitio para actualizar una contraseña o para buscar una contraseña que se ha actualizado recientemente.  
- Controladores de dominio en sitios de C y D no tiene acceso al maestro de RID en el sitio para obtener un grupo RID inicial después de la instalación de Active Directory y para grupos de RID se actualiza a medida que agotarse.  
- Controladores de dominio en sitios de C y D no se pueden agregar o quitar directory, DNS o las particiones de aplicación personalizada.  
- Controladores de dominio en sitios de C y D no pueden realizar cambios de esquema.  
  
Para que una hoja de cálculo que le ayudarán a planear la ubicación de rol de maestro de operaciones, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abrir Ubicación del controlador de dominio (DSSTOPO_4.doc).  
  
Deberá consultar esta información cuando se creación el dominio raíz del bosque y dominios regionales. Para obtener más información sobre cómo implementar el dominio raíz del bosque, vea implementar un [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Para obtener más información sobre cómo implementar dominios regionales, vea [implementar Windows Server 2008 dominios regionales](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Pasos siguientes

Información adicional sobre la ubicación de los roles FSMO puede encontrarse en el tema de soporte técnico [FSMO ubicación y optimización en los controladores de dominio de Active Directory](https://support.microsoft.com/help/223346)
