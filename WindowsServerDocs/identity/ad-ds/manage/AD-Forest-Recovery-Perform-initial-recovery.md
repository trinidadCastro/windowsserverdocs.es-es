---
title: 'Recuperación de bosque de AD: realizar la recuperación inicial'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 8b0498b30966c22ec8dca267988e109d976f6b69
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067747"
---
# <a name="perform-initial-recovery"></a>Realizar la recuperación inicial

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección incluye los siguientes pasos:

- [Restaurar el primer controlador de dominio grabable en cada dominio](#restore-the-first-writeable-domain-controller-in-each-domain)
- [Volver a conectar cada controlador de dominio grabable restaurado a la red](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)
- [Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar el primer controlador de dominio grabable en cada dominio

A partir de un controlador de dominio grabable en el dominio raíz del bosque, complete los pasos de esta sección para restaurar el primer controlador de dominio. El dominio raíz del bosque es importante porque almacena los grupos administradores de esquema y administradores de organización. También ayuda a mantener la jerarquía de confianza en el bosque. Además, el dominio raíz del bosque normalmente contiene el servidor raíz DNS para el espacio de nombres DNS del bosque. Por lo tanto, la zona DNS integrada Active Directory para ese dominio contiene los registros de recursos de alias (CNAME) de todos los demás controladores de dominio del bosque (que son necesarios para la replicación) y los registros de recursos DNS del catálogo global.

Después de recuperar el dominio raíz del bosque, repita los mismos pasos para recuperar los dominios restantes en el bosque. Puede recuperar más de un dominio simultáneamente. sin embargo, recupere siempre un dominio primario antes de recuperar un elemento secundario para evitar interrupciones en la jerarquía de confianza o en la resolución de nombres DNS.

Para cada dominio que recupere, restaure solo un DC grabable desde la copia de seguridad. Esta es la parte más importante de la recuperación porque el controlador de dominio debe tener una base de datos que no se haya influenciado por el hecho de que se produjera un error en el bosque. Es importante tener una copia de seguridad de confianza que se prueba minuciosamente antes de que se introduzca en el entorno de producción.

Después, lleve a cabo los siguiente pasos. Los procedimientos para realizar determinados pasos se encuentran en [ad Forest Recovery-Procedures](AD-Forest-Recovery-Procedures.md).

1. Si planea restaurar un servidor físico, asegúrese de que el cable de red del controlador de dominio de destino no está conectado y, por lo tanto, no está conectado a la red de producción. En el caso de una máquina virtual, puede quitar el adaptador de red o usar un adaptador de red que esté conectado a otra red donde pueda probar el proceso de recuperación mientras esté aislado de la red de producción.

2. Dado que se trata del primer controlador de dominio grabable en el dominio, debe realizar una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL. La operación de restauración se debe completar con una aplicación de copia de seguridad y restauración compatible con Active Directory, como Copias de seguridad de Windows Server (es decir, no debe restaurar el controlador de dominio mediante métodos no admitidos, como restaurar una instantánea de máquina virtual).
   - Se requiere una restauración autoritativa de SYSVOL porque se debe iniciar la replicación de la carpeta replicada SYSVOL después de recuperarse de un desastre. Todos los controladores de dominio posteriores que se agreguen en el dominio deben volver a sincronizar su carpeta SYSVOL con una copia de la carpeta que se haya seleccionado para que sea autoritativo antes de que se pueda anunciar la carpeta.

   > [!CAUTION]
   > Realice una operación de restauración autoritativa (o primaria) de SYSVOL solo para el primer DC que se va a restaurar en el dominio raíz del bosque. La realización incorrecta de las operaciones de restauración principales de SYSVOL en otros controladores de sesión genera conflictos de replicación de datos de SYSVOL.

   - Hay dos opciones para realizar una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL:
   - Realice una recuperación de servidor completa y, a continuación, fuerce una sincronización autoritativa de SYSVOL. Para ver los procedimientos detallados, consulte [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).
   - Realice una recuperación completa del servidor seguida de una restauración del estado del sistema. Esta opción requiere que se creen ambos tipos de copias de seguridad de antemano: una copia de seguridad completa del servidor y una copia de seguridad del estado del sistema. Para conocer los procedimientos detallados, vea [realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md) y [realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md).

3. Después de restaurar y reiniciar el controlador de dominio grabable, compruebe que el error no afectó a los datos del controlador de dominio. Si los datos del DC están dañados, repita el paso 2 con una copia de seguridad diferente.
   - Si el controlador de dominio restaurado hospeda un rol de maestro de operaciones, es posible que tenga que agregar la siguiente entrada del registro para evitar que AD DS no esté disponible hasta que se complete la replicación de una partición de directorio grabable:

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl realizar sincronizaciones iniciales**

      Cree la entrada con el tipo de datos **REG_DWORD** y un valor de **0** . Después de que el bosque se recupere por completo, puede restablecer el valor de esta entrada en **1** , que requiere un controlador de dominio que se reinicie y conserve que los roles de maestro de operaciones tengan éxito AD DS la replicación entrante y saliente con sus asociados de réplica conocidos antes de que se anuncie como controlador de dominio y empiece a proporcionar servicios a los clientes. Para obtener más información sobre los requisitos de sincronización inicial, consulte el artículo de KB [305476](https://support.microsoft.com/kb/305476).

      Continúe con los pasos siguientes solo después de restaurar y comprobar los datos y antes de unir este equipo a la red de producción.

4. Si sospecha que el error en todo el bosque estaba relacionado con la intrusión de red o un ataque malintencionado, restablezca las contraseñas de cuenta para todas las cuentas administrativas, incluidos los miembros de los grupos administradores de empresas, administradores de dominio, administradores de esquema, operadores de servidor, operadores de cuentas, etc. El restablecimiento de contraseñas de cuentas administrativas debe completarse antes de que se instalen controladores de dominio adicionales durante la siguiente fase de la recuperación del bosque.
5. En el primer DC restaurado del dominio raíz del bosque, asuma todas las funciones de maestro de operaciones para todo el dominio y para todo el bosque. Se necesitan credenciales de administrador de organización y administradores de esquema para asumir roles de maestro de operaciones en todo el bosque.

     En cada dominio secundario, asuma los roles de maestro de operaciones de todo el dominio. Aunque es posible que se conserven las funciones de maestro de operaciones en el controlador de dominio restaurado solo temporalmente, al asumir estos roles se asegurará de que el controlador de dominio los hospeda en este punto del proceso de recuperación del bosque. Como parte del proceso posterior a la recuperación, puede redistribuir los roles de maestro de operaciones según sea necesario. Para obtener más información acerca de cómo asumir roles de maestro de operaciones, consulte [asumir un rol de maestro de operaciones](AD-forest-recovery-seizing-operations-master-role.md). Para obtener recomendaciones sobre dónde colocar roles de maestro de operaciones, consulte [¿Qué son los maestros de operaciones?](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10)).

6. Limpie los metadatos de todos los demás controladores de dominio grabables del dominio raíz del bosque que no va a restaurar desde la copia de seguridad (todos los controladores de dominio que se pueden escribir en el dominio excepto este primer DC). Si usa la versión de Active Directory usuarios y equipos, o Active Directory sitios y servicios incluidos con Windows Server 2008 o posterior o RSAT para Windows Vista o versiones posteriores, la limpieza de los metadatos se realiza automáticamente al eliminar un objeto DC. Además, el objeto de servidor y el objeto de equipo para el controlador de dominio eliminado también se eliminan automáticamente. Para obtener más información, consulte [limpiar metadatos de controladores de DC de escritura eliminados](AD-Forest-Recovery-Cleaning-Metadata.md).

     La limpieza de metadatos impide la duplicación de objetos de configuración NTDS si AD DS está instalado en un controlador de dominio en un sitio diferente. Potencialmente, esto también podría guardar el comprobador de coherencia de la información (KCC) el proceso de creación de vínculos de replicación cuando es posible que los propios DC no estén presentes. Además, como parte de la limpieza de metadatos, los registros de recursos DNS del Ubicador de DC para todos los demás controladores de dominio del dominio se eliminarán del DNS.

     Hasta que se quiten los metadatos de todos los demás controladores de dominio del dominio, este DC, si era un maestro de RID antes de la recuperación, no asumirá el rol de maestro de RID y, por tanto, no podrá emitir nuevos RID. Es posible que vea el ID. de evento 16650 en el registro del sistema en Visor de eventos que indica este error, pero debería ver el ID. de evento 16648 que indica que la operación se realizó correctamente después de limpiar los metadatos.

7. Si tiene zonas DNS almacenadas en AD DS, asegúrese de que el servicio servidor DNS local está instalado y en ejecución en el controlador de dominio que ha restaurado. Si este DC no era un servidor DNS antes de que se produjera un error en el bosque, debes instalar y configurar el servidor DNS.

    > [!NOTE]
    > Si el controlador de dominio restaurado ejecuta Windows Server 2008, debe instalar la revisión en el artículo de KB [975654](https://support.microsoft.com/kb/975654) o conectar el servidor a una red aislada temporalmente para instalar el servidor DNS. La revisión no es necesaria para ninguna otra versión de Windows Server.

     En el dominio raíz del bosque, configure el controlador de dominio restaurado con su propia dirección IP (o una dirección de bucle invertido, como 127.0.0.1) como servidor DNS preferido. Puede configurar esta opción en las propiedades de TCP/IP del adaptador de red de área local (LAN). Este es el primer servidor DNS en el bosque. Para obtener más información, vea [configurar TCP/IP para utilizar DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10)).

     En cada dominio secundario, configure el controlador de dominio restaurado con la dirección IP del primer servidor DNS del dominio raíz del bosque como su servidor DNS preferido. Puede configurar esta opción en las propiedades de TCP/IP del adaptador de LAN. Para obtener más información, vea [configurar TCP/IP para utilizar DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779716(v=ws.10)).

     En el _msdcs y las zonas DNS de dominio, elimine los registros NS de los controladores de dominio que ya no existen después de la limpieza de metadatos. Compruebe si se han quitado los registros SRV de los controladores de seguridad limpiados. Para ayudar a acelerar la eliminación de registros SRV de DNS, ejecute:

    ```
    nltest.exe /dsderegdns:server.domain.tld
    ```

8. Eleva el valor del grupo de RID disponible por 100.000. Para obtener más información, vea [elevar el valor de los grupos de RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md). Si tiene motivos para creer que la elevación del grupo de RID por 100.000 es insuficiente para su situación concreta, debe determinar el aumento más bajo que sigue siendo seguro de usar. Los RID son un recurso finito que no debe usarse innecesariamente.

     Si se crearon nuevas entidades de seguridad en el dominio después del tiempo de la copia de seguridad que utiliza para la restauración, estas entidades de seguridad podrían tener derechos de acceso en determinados objetos. Estas entidades de seguridad ya no existen después de la recuperación porque la recuperación se ha revertido a la copia de seguridad; sin embargo, es posible que los derechos de acceso sigan existiendo. Si el grupo de RID disponible no se inicia después de una restauración, los nuevos objetos de usuario creados después de la recuperación del bosque pueden obtener identificadores de seguridad (SID) idénticos y pueden tener acceso a esos objetos, que no se pretendían originalmente.

     Para ilustrarlo, considere el ejemplo del nuevo empleado denominado Ana que se mencionó en la introducción. El objeto de usuario de Ana ya no existe después de la operación de restauración porque se creó después de la copia de seguridad que se usó para restaurar el dominio. Sin embargo, los derechos de acceso que se asignaron a ese objeto de usuario podrían persistir después de la operación de restauración. Si el SID de ese objeto de usuario se reasigna a un nuevo objeto después de la operación de restauración, el nuevo objeto obtendría esos derechos de acceso.

9. Invalidar el grupo de RID actual. El grupo de RID actual se invalidará después de una restauración del estado del sistema. Pero si no se realizó una restauración de estado del sistema, es necesario invalidar el grupo de RID actual para evitar que el controlador de dominio restaurado vuelva a emitir RID desde el grupo de RID que se asignó en el momento en que se creó la copia de seguridad. Para obtener más información, vea [invalidar el grupo de RID actual](AD-Forest-Recovery-Invaildate-RID-Pool.md).

    > [!NOTE]
    > La primera vez que intente crear un objeto con un SID después de invalidar el grupo de RID, recibirá un error. El intento de crear un objeto desencadena una solicitud para un nuevo grupo RID. El reintento de la operación se realiza correctamente porque se asignará el nuevo grupo RID.

10. Restablezca la contraseña de la cuenta de equipo de este DC dos veces. Para obtener más información, vea [restablecer la contraseña de la cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md).

11. Restablezca la contraseña de krbtgt dos veces. Para obtener más información, vea [restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md).

     Dado que el historial de contraseñas de krbtgt es dos contraseñas, restablezca las contraseñas dos veces para quitar la contraseña original (preerror) del historial de contraseñas.

    > [!NOTE]
    > Si la recuperación del bosque está en respuesta a una infracción de seguridad, también puede restablecer las contraseñas de confianza. Para obtener más información, consulte [restablecer una contraseña de confianza en un lado de la confianza](AD-Forest-Recovery-Reset-Trust.md).

12. Si el bosque tiene varios dominios y el controlador de dominio restaurado era un servidor de catálogo global antes del error, desactive la casilla **catálogo global** en las propiedades de configuración NTDS para quitar el catálogo global del controlador de dominio. La excepción a esta regla es el caso común de un bosque con un solo dominio. En este caso, no es necesario quitar el catálogo global. Para obtener más información, consulte [quitar el catálogo global](AD-Forest-Recovery-Remove-GC.md).

     Al restaurar un catálogo global a partir de una copia de seguridad más reciente que otras copias de seguridad que se usan para restaurar controladores de dominio en otros dominios, puede introducir objetos persistentes. Considere el ejemplo siguiente. En el dominio A, DC1 se restaura a partir de una copia de seguridad tomada en el momento T1. En el dominio B, DC2 se restaura a partir de una copia de seguridad del catálogo global tomada en el momento T2. Suponga que T2 es más reciente que T1 y que se crearon algunos objetos entre T1 y T2. Una vez restaurados estos controladores de dominio, DC2, que es un catálogo global, contiene los datos más recientes de la réplica parcial del dominio A que el dominio A. En este caso, DC2 contiene objetos persistentes porque estos objetos no están presentes en DC1.

     La presencia de objetos persistentes puede dar lugar a problemas. Por ejemplo, es posible que los mensajes de correo electrónico no se entreguen a un usuario cuyo objeto de usuario se haya desplazado entre dominios. Después de volver a poner en línea el controlador de dominio o el servidor de catálogo global no actualizado, aparecen ambas instancias del objeto de usuario en el catálogo global. Ambos objetos tienen la misma dirección de correo electrónico; por lo tanto, no se pueden entregar mensajes de correo electrónico.

     Un segundo problema es que una cuenta de usuario que ya no existe podría aparecer en la lista global de direcciones. Un tercer problema es que un grupo universal que ya no existe puede seguir apareciendo en el token de acceso de un usuario.

     Si restaura un controlador de dominio que era un catálogo global, ya sea por accidente o porque se trata de la copia de seguridad de solitarios de confianza, se recomienda evitar la aparición de objetos persistentes deshabilitando el catálogo global poco después de que se complete la operación de restauración. Al deshabilitar la marca de catálogo global, el equipo perderá todas las réplicas parciales (particiones) y se relegará a su estado de DC normal.

13. Configure el servicio de hora de Windows. En el dominio raíz del bosque, configure el emulador de PDC para sincronizar la hora desde un origen de hora externo. Para obtener más información, vea [configurar el servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731191%28v=ws.10%29).

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Volver a conectar cada controlador de dominio grabable restaurado a una red común

En esta fase debe tener un controlador de dominio restaurado (y los pasos de recuperación realizados) en el dominio raíz del bosque y en cada uno de los dominios restantes. Unir estos controladores de red a una red común que está aislada del resto del entorno y completar los pasos siguientes para validar el estado del bosque y la replicación.

> [!NOTE]
> Al unir los controladores de dispositivo físicos a una red aislada, puede que tenga que cambiar sus direcciones IP. Como resultado, las direcciones IP de los registros DNS serán incorrectas. Dado que un servidor de catálogo global no está disponible, se producirá un error en las actualizaciones dinámicas seguras para DNS. Los controladores de dominio virtuales son más ventajosos en este caso porque pueden unirse a una nueva red virtual sin cambiar sus direcciones IP. Esta es una de las razones por las que se recomiendan los controladores de dominio virtuales como los primeros controladores de dominio que se restaurarán durante la recuperación del bosque.

Después de la validación, una los controladores de red a la red de producción y complete los pasos para comprobar el estado de la replicación del bosque.

- Para corregir la resolución de nombres, cree registros de delegación DNS y configure los reenvíos de DNS y las sugerencias de raíz según sea necesario. Ejecute **repadmin/replsum** para comprobar la replicación entre controladores de DC.
- Si los DC restaurados no son asociados de replicación directos, la recuperación de la replicación será mucho más rápida mediante la creación de objetos de conexión temporales entre ellos.
- Para validar la limpieza de metadatos, ejecute **repadmin/viewlist \\** _ para obtener una lista de todos los controladores de DC del bosque. Ejecute _ *NLTEST/DCList:* *  *<dominio \>* para obtener una lista de todos los controladores de dominio del dominio.
- Para comprobar el estado de los controladores de dominio y DNS, ejecute DCDiag/v para notificar los errores en todos los controladores de dominio del bosque.

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Agregar el catálogo global a un controlador de dominio en el dominio raíz del bosque

Se requiere un catálogo global por estos y otros motivos:

- Para habilitar los inicios de sesión para los usuarios.
- Para habilitar el servicio de inicio de sesión de red que se ejecuta en los controladores de dominio de cada dominio secundario para registrar y quitar registros en el servidor DNS del dominio raíz.

Aunque es preferible que el controlador de dominio raíz del bosque se convierta en un catálogo global, es posible elegir cualquiera de los controladores de dominio restaurados para que se convierta en un catálogo global.

> [!NOTE]
> Un controlador de dominio no se anunciará como un servidor de catálogo global hasta que se haya completado una sincronización completa de todas las particiones de directorio en el bosque. Por lo tanto, se debe forzar la replicación del DC con cada uno de los controladores de dominio restaurados en el bosque.
>
> Supervise el registro de eventos del servicio de directorio en Visor de eventos para el ID. de evento 1119, que indica que este controlador de dominio es un servidor de catálogo global, o Compruebe que la siguiente clave del registro tiene un valor de 1:
>
> **Promoción del catálogo de HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global completada**

Para obtener más información, vea [Agregar el catálogo global](AD-Forest-Recovery-Add-GC.md).

En esta fase debe tener un bosque estable, con un DC para cada dominio y un catálogo global en el bosque. Debe crear una nueva copia de seguridad de cada uno de los controladores de DC que acaba de restaurar. Ahora puede empezar a implementar otros controladores de usuario en el bosque mediante la instalación de AD DS.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
