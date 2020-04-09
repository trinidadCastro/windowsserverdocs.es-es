---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planear la ubicación del rol de maestro de operaciones
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 990f93d44189a6061653d5e190a176b049a280c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822108"
---
# <a name="planning-operations-master-role-placement"></a>Planear la ubicación del rol de maestro de operaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) admite la replicación con varios maestros de datos de directorio, lo que significa que cualquier controlador de dominio puede aceptar cambios de directorio y replicar los cambios en todos los demás controladores de dominio. Sin embargo, algunos cambios, como las modificaciones de esquema, no resultan prácticos para un modo multimaestro. Por este motivo, ciertos controladores de dominio, conocidos como maestros de operaciones, tienen roles responsables de aceptar solicitudes para determinados cambios específicos.  
  
> [!NOTE]  
> Los titulares de la función de maestro de operaciones deben poder escribir información en la base de datos Active Directory. Debido a la naturaleza de solo lectura de la base de datos Active Directory en un controlador de dominio de solo lectura (RODC), los **RODC no pueden actuar como titulares**de la función de maestro de operaciones.  
  
En cada dominio existen tres roles de maestro de operaciones (también conocidos como operaciones de maestro único flexible o FSMO):  
  
- El maestro de operaciones del emulador del controlador de dominio principal (PDC) procesa todas las actualizaciones de contraseñas.  

- El maestro de operaciones de ID. relativo (RID) mantiene el grupo de RID global para el dominio y asigna los grupos de RID locales a todos los controladores de dominio para asegurarse de que todas las entidades de seguridad creadas en el dominio tienen un identificador único.  
- El maestro de operaciones de infraestructura de un dominio determinado mantiene una lista de las entidades de seguridad de otros dominios que son miembros de grupos dentro de su dominio.  

Además de las tres funciones de maestro de operaciones de nivel de dominio, existen dos roles de maestro de operaciones en cada bosque:  
  
- El maestro de operaciones de esquema rige los cambios en el esquema.  
- El maestro de operaciones de nomenclatura de dominios agrega y quita dominios y otras particiones de directorio (por ejemplo, particiones de aplicación del sistema de nombres de dominio (DNS)) en el bosque y desde él.  
  
Coloque los controladores de dominio que hospedan estas funciones de maestro de operaciones en áreas en las que la confiabilidad de la red sea alta y asegúrese de que el emulador de PDC y el maestro de RID están disponibles de forma coherente.  
  
Los titulares de la función de maestro de operaciones se asignan automáticamente cuando se crea el primer controlador de dominio de un dominio determinado. Los dos roles de nivel de bosque (maestro de esquema y maestro de nomenclatura de dominios) se asignan al primer controlador de dominio creado en un bosque. Además, los tres roles de nivel de dominio (maestro RID, maestro de infraestructura y emulador de PDC) se asignan al primer controlador de dominio creado en un dominio.  
  
> [!NOTE]  
> Las asignaciones automáticas del titular del rol de maestro de operaciones solo se realizan cuando se crea un nuevo dominio y cuando se degrada el titular de la función actual. Todos los demás cambios en los propietarios de roles deben ser iniciados por un administrador.  
  
Estas asignaciones automáticas de roles de maestro de operaciones pueden provocar un uso de CPU muy elevado en el primer controlador de dominio creado en el bosque o en el dominio. Para evitar esto, asigne (transferir) roles de maestro de operaciones a varios controladores de dominio de su bosque o dominio. Coloque los controladores de dominio que hospedan los roles de maestro de operaciones en áreas en las que la red sea confiable y donde todos los demás controladores de dominio del bosque puedan tener acceso a los maestros de operaciones.  
  
También debe designar maestros de operaciones en espera (alternativos) para todas las funciones de maestro de operaciones. Los maestros de operaciones en espera son controladores de dominio a los que se pueden transferir los roles de maestro de operaciones en caso de que se produzca un error en los titulares de roles originales. Asegúrese de que los maestros de operaciones en espera son asociados de replicación directos de los maestros de operaciones reales.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planeación de la ubicación del emulador de PDC

El emulador de PDC procesa los cambios de contraseña de cliente. Solo un controlador de dominio actúa como el emulador de PDC en cada dominio del bosque.  
  
Incluso si todos los controladores de dominio se actualizan a Windows 2000, Windows Server 2003 y Windows Server 2008, y el dominio está funcionando en el nivel funcional nativo de Windows 2000, el emulador de PDC recibe la replicación preferencial de los cambios de contraseña realizados por otros controladores de dominio en el dominio. Si se ha cambiado una contraseña recientemente, ese cambio tarda en replicarse en todos los controladores de dominio del dominio. Si se produce un error de autenticación de inicio de sesión en otro controlador de dominio debido a una contraseña incorrecta, el controlador de dominio reenvía la solicitud de autenticación al emulador de PDC antes de decidir si aceptar o rechazar el intento de inicio de sesión.  
  
Coloque el emulador de PDC en una ubicación que contenga un gran número de usuarios de ese dominio para operaciones de reenvío de contraseñas, si es necesario. Además, asegúrese de que la ubicación está bien conectada a otras ubicaciones para reducir la latencia de replicación.  
  
En el caso de una hoja de cálculo que le ayude a documentar la información sobre dónde planea colocar los emuladores de PDC y el número de usuarios para cada dominio que se representa en cada ubicación, vea la ayuda del trabajo para el kit de implementación de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir la ubicación del controlador de dominio (DSSTOPO_4. doc).  
  
Debe consultar la información acerca de las ubicaciones en las que necesita colocar emuladores de PDC al implementar dominios regionales. Para obtener más información acerca de la implementación de dominios regionales, consulte [implementación de dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para la colocación del maestro de infraestructura  

El maestro de infraestructura actualiza los nombres de las entidades de seguridad de otros dominios que se agregan a los grupos de su propio dominio. Por ejemplo, si un usuario de un dominio es miembro de un grupo en un segundo dominio y se cambia el nombre del usuario en el primer dominio, el segundo dominio no recibirá una notificación de que el nombre del usuario debe actualizarse en la lista de miembros del grupo. Dado que los controladores de dominio de un dominio no replican las entidades de seguridad en los controladores de dominio de otro dominio, el segundo dominio nunca es consciente del cambio en ausencia del maestro de infraestructura.  
  
El maestro de infraestructura supervisa constantemente la pertenencia a grupos, buscando entidades de seguridad de otros dominios. Si encuentra uno, comprueba el dominio de la entidad de seguridad para comprobar que la información está actualizada. Si la información no está actualizada, el maestro de infraestructura realiza la actualización y, a continuación, replica el cambio en los otros controladores de dominio de su dominio.  
  
Se aplican dos excepciones a esta regla. En primer lugar, si todos los controladores de dominio son servidores de catálogo global, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante, ya que los catálogos globales replican la información actualizada independientemente del dominio al que pertenecen. En segundo lugar, si el bosque solo tiene un dominio, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante porque las entidades de seguridad de otros dominios no existen.  
  
No coloque el maestro de infraestructura en un controlador de dominio que también sea un servidor de catálogo global. Si el maestro de infraestructura y el catálogo global están en el mismo controlador de dominio, el maestro de infraestructura no funcionará. El maestro de infraestructura nunca buscará datos que no estén actualizados; por lo tanto, nunca replicará ningún cambio en los otros controladores de dominio del dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Ubicación del maestro de operaciones para redes con conectividad limitada

Tenga en cuenta que si su entorno tiene una ubicación central o un sitio concentrador en el que puede colocar los titulares de la función de maestro de operaciones, es posible que se vean afectados ciertas operaciones de controlador de dominio que dependen de la disponibilidad de los titulares de la función de maestro de operaciones.  
  
Por ejemplo, supongamos que una organización crea los sitios A, B, C y D. existen vínculos de sitio entre A y B, entre B y C, y entre C y D. la conectividad de red refleja exactamente la conectividad de red de los vínculos a sitios. En este ejemplo, todos los roles de maestro de operaciones se colocan en el sitio A y la opción para **enlazar todos los vínculos a sitios** no está seleccionada.  
  
Aunque esta configuración produce una replicación correcta entre todos los sitios, las funciones de la función de maestro de operaciones tienen las siguientes limitaciones:  
  
- Los controladores de dominio de los sitios C y D no pueden tener acceso al emulador de PDC del sitio a para actualizar una contraseña o para comprobar si la contraseña se ha actualizado recientemente.  
- Los controladores de dominio de los sitios C y D no pueden tener acceso al maestro de RID en el sitio a para obtener un grupo de RID inicial después de la instalación de Active Directory y para actualizar los grupos de RID a medida que se agotan.  
- Los controladores de dominio de los sitios C y D no pueden agregar o quitar particiones de directorio, DNS o aplicaciones personalizadas.  
- Los controladores de dominio de los sitios C y D no pueden realizar cambios en el esquema.  
  
Para ver una hoja de cálculo que le ayude a planear la ubicación de los roles de maestro de operaciones, consulte el [Kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abra la ubicación del controlador de dominio (DSSTOPO_4. doc).  
  
Tendrá que consultar esta información al crear el dominio raíz del bosque y los dominios regionales. Para obtener más información acerca de la implementación del dominio raíz del bosque, vea implementar un [dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Para obtener más información acerca de la implementación de dominios regionales, consulte [implementación de dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar más información sobre la ubicación de los roles de FSMO en el tema de soporte de [Ubicación y optimización de FSMO en Active Directory controladores de dominio](https://support.microsoft.com/help/223346) .
