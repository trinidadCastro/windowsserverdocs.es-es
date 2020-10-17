---
title: verifier
description: Artículo de referencia para el comando Verifier, que ejecuta la utilidad Administrador de Comprobador de controladores.
ms.topic: reference
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: df3a9826463ea4062fb661e04e8fb774275c09d7
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156260"
---
# <a name="verifier"></a>verifier

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comprobador de controladores supervisa controladores de modo kernel de Windows y controladores de gráficos para detectar llamadas o acciones de función no válidas que podrían dañar el sistema. El comprobador de controladores puede sujetar controladores de Windows a una variedad de tensiones y pruebas para encontrar un comportamiento inadecuado. Puede configurar las pruebas que se van a ejecutar, lo que le permite colocar un controlador a través de cargas de estrés intensivas o a través de pruebas más simplificadas. También puede ejecutar el comprobador de controladores en varios controladores simultáneamente o en un controlador cada vez.

> [!IMPORTANT]
> Debe estar en el grupo administradores del equipo para usar el comprobador de controladores.
> Ejecutar el comprobador de controladores puede provocar que el equipo se bloquee, por lo que solo debe ejecutar esta utilidad en los equipos que se usan para probar y depurar.

## <a name="syntax"></a>Sintaxis

```
verifier /standard /all
verifier /standard /driver NAME [NAME ...]
verifier /flags <options> /all
verifier /flags <options> /driver NAME [NAME ...]
verifier /rules [OPTION ...]
verifier /query
verifier /querysettings
verifier /bootmode [persistent | disableafterfail | oneboot]
verifier /reset
verifier /faults [Probability] [PoolTags] [Applications] [DelayMins]
verifier /faultssystematic [OPTION ...]
verifier /log LOG_FILE_NAME [/interval SECONDS]
verifier /volatile /flags <options>
verifier /volatile /adddriver NAME [NAME ...]
verifier /volatile /removedriver NAME [NAME ...]
verifier /volatile /faults [Probability] [PoolTags] [Applications] [DelayMins]
verifier /domain <types> <options> /driver ... [/logging | /livedump]
verifier /logging
verifier /livedump
verifier /?
verifier /help
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /all | Indica a la utilidad del comprobador de controladores que compruebe todos los controladores instalados después del siguiente arranque. |
| /bootmode `[persistent | disableafterfail | oneboot | resetonunusualshutdown]` | Controla si la configuración de la utilidad del comprobador de controladores está habilitada después de un reinicio. Para establecer o cambiar esta opción, debe reiniciar el equipo. Están disponibles los siguientes modos:<ul><li>**persistente** : garantiza que la configuración del comprobador de controladores conserva (se mantiene en vigor) en muchos reinicios. Esta es la configuración predeterminada.</li><li>**disableafterfail** : Si Windows no se inicia, esta configuración deshabilita la utilidad del comprobador de controladores para los reinicios posteriores.</li><li>**oneboot** : solo habilita la configuración del comprobador de controladores para la próxima vez que se inicie el equipo. La utilidad del comprobador de controladores está deshabilitada para los reinicios posteriores.</li><li>**resetonunusualshutdown** : la utilidad del comprobador de controladores se conservará hasta que se produzca un apagado inusual. Se puede usar su abreviatura, ' Rous '.</li></ul> |
| /driver `<driverlist>` | Especifica uno o más controladores que se comprobarán. El parámetro **driverlist** es una lista de controladores por nombre binario, como *driver.sys*. Use un espacio para separar cada nombre de controlador. Los valores de carácter comodín, como `n*.sys` , no se admiten. |
| /driver.exclude `<driverlist>` | Especifica uno o más controladores que se excluirán de la comprobación. Este parámetro solo es aplicable si se seleccionan todos los controladores para la comprobación. El parámetro **driverlist** es una lista de controladores por nombre binario, como *driver.sys*. Use un espacio para separar cada nombre de controlador. Los valores de carácter comodín, como `n*.sys` , no se admiten. |
| /faults | Habilita la característica de **simulación de pocos recursos** en la utilidad del comprobador de controladores. Puede usar **/faults** en lugar de `/flags 0x4` . Sin embargo, no se puede usar `/flags 0x4` con los subparámetros de **/faults** . Puede usar los siguientes subparámetros del parámetro/faults para configurar la simulación de recursos insuficientes:<ul><li>**Probabilidad** : especifica la probabilidad de que la utilidad del comprobador de controladores produzca un error en una asignación determinada. Escriba un número (en formato decimal o hexadecimal) que represente el número de posibilidades de 10.000 de que la utilidad del comprobador de controladores produzca un error en la asignación. El valor predeterminado, 600, significa 600/10000 o 6%.</li><li>**Etiquetas de grupo** : limita las asignaciones que la utilidad de Comprobador de controladores puede no tener en las asignaciones con las etiquetas de grupo especificadas. Puede usar un carácter comodín (*) para representar varias etiquetas de grupo. Para enumerar varias etiquetas de grupo, sepárelas con espacios. De forma predeterminada, se puede producir un error en todas las asignaciones. </li> <li> * * Aplicaciones**: limita las asignaciones que la utilidad de Comprobador de controladores puede no tener en las asignaciones para el programa especificado. Escriba el nombre de un archivo ejecutable. Para enumerar programas, separe los nombres de los programas con espacios. De forma predeterminada, se puede producir un error en todas las asignaciones.</li><li>**DelayMins** : especifica el número de minutos después del arranque durante el cual la utilidad del comprobador de controladores no produce errores de forma intencionada en ninguna asignación. Este retraso permite que los controladores se carguen y el sistema se estabilice antes de que comience la prueba. Escriba un número (en formato decimal o hexadecimal). El valor predeterminado es 7 (minutos).</li></ul> |
| /faultssystematic | Especifica las opciones para la simulación **sistemática de recursos insuficientes** . Use la `0x40000` marca para seleccionar la opción de simulación **sistemática de recursos insuficientes** . Están disponibles las siguientes opciones:<ul><li>**enableboottime** : habilita las inyecciones de errores en los reinicios del equipo.</li><li>**disableboottime** : deshabilita la inserción de errores en los reinicios del equipo (es la configuración predeterminada).</li><li>**recordboottime** : habilita las inyecciones de errores en el modo en que se reinicie el equipo.</li><li>**resetboottime** : deshabilita las inyecciones de errores en los reinicios del equipo y borra la lista de exclusión de la pila.</li><li>**enableruntime** : habilita dinámicamente las inyecciones de errores.</li><li>**disableruntime** : deshabilita dinámicamente las inyecciones de errores.</li><li>**recordruntime** : habilita dinámicamente las inyecciones de errores en el modo if.</li><li>**resetruntime** : deshabilita dinámicamente las inyecciones de errores y borra la lista de pilas con errores anteriormente.</li><li>**querystatistics** : muestra las estadísticas de inyección de errores actuales.</li><li>**incrementcounter** : incrementa el contador de pruebas superadas que se usa para identificar cuándo se insertó un error.</li><li>**contador getstackid** : recupera el identificador de pila insertada indicado.</li><li>**EXCLUDESTACK STACKID** : excluye la pila de la inserción de errores.</li></ul> |
| /Flags `<options>` | Activa las opciones especificadas después del siguiente reinicio. Este número puede escribirse en formato decimal o hexadecimal (con un prefijo 0x). Se permite cualquier combinación de los siguientes valores:<ul><li>**Valor: 1 o 0x1 (bit 0)**  -  [Comprobación de grupo especial](https://docs.microsoft.com/windows-hardware/drivers/devtest/special-pool)</li><li>**Valor: 2 o 0X2 (bit 1)**  -  [Forzar la comprobación de IRQL](https://docs.microsoft.com/windows-hardware/drivers/devtest/force-irql-checking)</li><li>**Valor: 4 o 0x4 (bit 2)**  -  [Simulación de recursos insuficientes](https://docs.microsoft.com/windows-hardware/drivers/devtest/low-resources-simulation)</li><li>**Valor: 8 o 0x8 (bit 3)**  -  [Seguimiento de grupo](https://docs.microsoft.com/windows-hardware/drivers/devtest/pool-tracking)</li><li>**Valor: 16 o 0x10 (bit 4)**  -  [Comprobación de e/s](https://docs.microsoft.com/windows-hardware/drivers/devtest/i-o-verification)</li><li>**Valor: 32 o 0x20 (bit 5)**  -  [Detección de interbloqueos](https://docs.microsoft.com/windows-hardware/drivers/devtest/deadlock-detection)</li><li>**Valor: 64 o 0x40 (bit 6)**  -  [Comprobación de e/s mejorada](https://docs.microsoft.com/windows-hardware/drivers/devtest/enhanced-i-o-verification). Esta opción se activa automáticamente cuando se selecciona la **comprobación de e/s**.</li><li>**Valor: 128 o 0x80 (bit 7)**  -  [Comprobación de DMA](https://docs.microsoft.com/windows-hardware/drivers/devtest/dma-verification)</li><li>**Valor: 256 o 0x100 (bit 8)**  -  [Comprobaciones de seguridad](https://docs.microsoft.com/windows-hardware/drivers/devtest/security-checks)</li><li>**Valor: 512 o 0x200 (bit 9)**  -  [Forzar solicitudes de e/s pendientes](https://docs.microsoft.com/windows-hardware/drivers/devtest/force-pending-i-o-requests)</li><li>**Valor: 1024 o 0x400 (bit 10)**  -  [Registro IRP](https://docs.microsoft.com/windows-hardware/drivers/devtest/irp-logging)</li><li>**Valor: 2048 o 0x800 (bit 11)**  -  [Comprobaciones varias](https://docs.microsoft.com/windows-hardware/drivers/devtest/miscellaneous-checks)</li><li>**Valor: 8192 o 0x2000 (bit 13)**  -  [Comprobación de MDL invariable para la pila](https://docs.microsoft.com/windows-hardware/drivers/devtest/invariant-mdl-checking-for-stack)</li><li>**Valor: 16384 o 0x4000 (bit 14)**  -  [Comprobación de MDL invariable para el controlador](https://docs.microsoft.com/windows-hardware/drivers/devtest/invariant-mdl-checking-for-driver)</li><li>**Valor: 32768 o 0x8000 (bit 15)**  -  [Retrasar el retraso de Power Framework](https://docs.microsoft.com/windows-hardware/drivers/devtest/concurrency-stress-test)</li><li>**Valor: 65536 o 0x10000 (bit 16)** : comprobación de la interfaz de Puerto/minipuerto</li><li>**Valor: 131072 o 0x20000 (bit 17)**  -  [Comprobación de cumplimiento DDI](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking)</li><li>**Valor: 262144 o 0x40000 (bit 18)**  -  [Simulación sistemática de recursos insuficientes](https://docs.microsoft.com/windows-hardware/drivers/devtest/systematic-low-resource-simulation)</li><li>**Valor: 524288 o 0x80000 (bit 19)**  -  [Comprobación de cumplimiento DDI (adicional)](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking)</li><li>**Valor: 2097152 o 0x200000 (bit 21)**  -  [Comprobación de NDIS/WiFi](https://docs.microsoft.com/windows-hardware/drivers/devtest/ndis-wifi-verification)</li><li>**Valor: 8388608 o 0x800000 (bit 23)**  -  [Retraso de sincronización de kernel](https://docs.microsoft.com/windows-hardware/drivers/devtest/kernel-synchronization-delay-fuzzing)</li><li>**Valor: 16777216 o 0x1000000 (bit 24)**  -  [Comprobación del conmutador de máquina virtual](https://docs.microsoft.com/windows-hardware/drivers/devtest/vm-switch-verification)</li><li>**Valor: 33554432 o 0x2000000 (bit 25)** : comprobaciones de integridad de código. No puede usar este método para activar las opciones de comprobación de SCSI o de comprobación de Storport. Para obtener más información, consulte [comprobación de SCSI](https://docs.microsoft.com/windows-hardware/drivers/devtest/scsi-verification) y comprobación de [Storport](https://docs.microsoft.com/windows-hardware/drivers/devtest/dv-storport-verification).</li></ul> |
| /Flags `<volatileoptions>` | Especifica las opciones de la utilidad del comprobador de controladores que se cambian inmediatamente sin necesidad de reiniciar. Este número puede escribirse en formato decimal o hexadecimal (con un prefijo 0x). Se permite cualquier combinación de los siguientes valores:<ul><li>**Valor: 1 o 0x1 (bit 0)** : grupo especial</li><li>**Valor: 2 o 0X2 (bit 1)** : forzar la comprobación de IRQL</li><li>**Valor: 4 o 0x4 (bit 2)** : simulación de recursos insuficientes</li></ul> |
| `<probability>` | Número comprendido entre 1 y 10.000 que especifica la probabilidad de inserción de errores. Por ejemplo, si se especifica 100, se indica una probabilidad de inserción de errores de 1% (100/10000).<p>Si no se especifica este parámetro, se utiliza la probabilidad predeterminada de 6%. |
| `<tags>` | Especifica las etiquetas de grupo que se insertarán con errores, separados por caracteres de espacio. Si no se especifica este parámetro, se puede insertar cualquier asignación de grupo con errores. |
| `<apps>` | Especifica el nombre del archivo de imagen de las aplicaciones que se insertarán con errores, separados por caracteres de espacio. Si no se especifica este parámetro, la simulación de recursos bajos puede tener lugar en cualquier aplicación. |
| `<minutes>` | Un número positivo que especifica la duración del período después del reinicio, en minutos, durante el que no se producirá ninguna inserción de errores. Si no se especifica este parámetro, se usa la longitud predeterminada de *8 minutos* . |
| /iolevel `<level>` | Especifica el nivel de comprobación de e/s. El valor de [LEVEL] puede ser **1** : habilita la comprobación de e/s de nivel 1 (valor predeterminado) o **2** : habilita la comprobación de e/s de nivel 1 y la comprobación de e/s de nivel 2. Si la comprobación de e/s no está habilitada (mediante `/flags 0x10` ), **/iolevel** se omite. |
| /log `<logfilename> [/intervalseconds]` | Crea un archivo de registro con el nombre especificado. La utilidad del comprobador de controladores escribe periódicamente las estadísticas en este archivo, según el intervalo que se establezca opcionalmente. El intervalo predeterminado es de *30 segundos*.<p> Si se escribe un comando Verifier **/log** en la línea de comandos, el símbolo del sistema no devuelve. Para cerrar el archivo de registro y devolver un símbolo del sistema, use la tecla **Ctrl + C** . Tras un reinicio, para crear un registro, debe volver a enviar el comando Verifier **/log** . |
| /rules `<option>` | Opciones de reglas que se pueden deshabilitar, entre las que se incluyen:<ul><li>**consulta** : muestra el estado actual de las reglas controlables.</li><li>**restablecer** : restablece todas las reglas a su estado predeterminado.</li><li>**identificador predeterminado** : establece el identificador de regla en su estado predeterminado. En el caso de las reglas admitidas, el identificador de regla es el valor del parámetro 1 de la comprobación de errores 0xC4 (DRIVER_VERIFIER_DETECTED_VIOLATION).</li><li>**deshabilitar identificador** : deshabilita el identificador de regla especificado. En el caso de las reglas admitidas, el identificador de regla es el valor del parámetro 1 de la comprobación de errores 0xC4 (DRIVER_VERIFIER_DETECTED_VIOLATION).</li></ul> |
| /instancia estándar | Activa las opciones "estándar" o comprobador de controladores predeterminado después del siguiente reinicio. Las opciones estándar son grupo especial, forzar la comprobación de IRQL, seguimiento de grupos, comprobación de e/s, detección de interbloqueos, comprobación de DMA, comprobaciones de seguridad, comprobaciones varias y comprobación de cumplimiento DDI. Es equivalente a `/flags 0x209BB`.<p>[!NOTE] A partir de las versiones de Windows 10 posteriores a 1803, el uso de ya `/flags 0x209BB` no habilitará automáticamente la comprobación de WDF. Use la sintaxis **/instancia estándar** para habilitar las opciones estándar, con la comprobación WDF incluida. |
| /volatile | Cambia la configuración sin reiniciar el equipo. La configuración volátil surte efecto inmediatamente.<p>Puede usar el parámetro **/volatile** con el parámetro **/Flags** para habilitar y deshabilitar algunas opciones sin necesidad de reiniciar. También puede usar **/volatile** con los parámetros **/AddDriver** y **/removedriver** para iniciar o detener la comprobación de un controlador sin necesidad de reiniciar, incluso si la utilidad del comprobador de controladores no se está ejecutando. Para obtener más información, consulte [using volatile Settings](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings). |
| /adddriver `<volatiledriverlist>` | Quita los controladores especificados de la configuración volátil. Para especificar varios controladores, enumere sus nombres, separados por espacios. No se admiten los valores de carácter comodín, como *n.sys*. |
| /removedriver `<volatiledriverlist>` |
| /reset | Borra toda la configuración de la utilidad del comprobador de controladores. Después del siguiente reinicio, no se comprobará ningún controlador. |
| /querysettings | Muestra un resumen de las opciones que se activarán y los controladores que se comprobarán después del siguiente arranque. La visualización no incluye los controladores y las opciones que se agregan mediante el parámetro **/volatile** . Para ver otras formas de ver esta configuración, consulte [ver la configuración del comprobador de controladores](https://docs.microsoft.com/windows-hardware/drivers/devtest/viewing-driver-verifier-settings). |
| /Query | Muestra un resumen de la actividad actual de la utilidad del comprobador de controladores. El campo **nivel** de la pantalla es el valor hexadecimal de las opciones establecidas con el parámetro **/volatile** . Para obtener una explicación de cada estadística, consulte [supervisión de contadores globales](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-global-counters) y [supervisión de contadores individuales](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-individual-counters). |
| /Domain `<types> <options>` | Controla la configuración de la extensión del comprobador. Se admiten los siguientes tipos de extensión de comprobador:<ul><li>**WDM** : habilita la extensión del comprobador para los controladores WDM.</li><li>**NDIS** : habilita la extensión del comprobador para los controladores de red.</li><li>**KS** : habilita la extensión del comprobador para los controladores de streaming en modo kernel.</li><li>**audio** : habilita la extensión del comprobador para los controladores de audio.</li></ul>. Se admiten las siguientes opciones de extensión:<ul><li>**rules. default** : habilita las reglas de validación predeterminadas para la extensión de comprobador seleccionada.</li><li>**rules. All** : habilita todas las reglas de validación para la extensión de comprobador seleccionada.</li></ul> |
| /logging | Habilita el registro de reglas infringidas detectadas por las extensiones de comprobador seleccionadas. |
| /livedump | Habilita la recopilación de volcado de memoria activa para las reglas infringidas detectadas por las extensiones de comprobador seleccionadas. |
| /? | Muestra la ayuda de la línea de comandos. |

### <a name="return-codes"></a>Códigos de retorno

Se devuelven los siguientes valores después de ejecutar el comprobador de controladores:

- 0: EXIT_CODE_SUCCESS

- 1: EXIT_CODE_ERROR

- 2: EXIT_CODE_REBOOT_NEEDED

#### <a name="remarks"></a>Comentarios

- Puede usar el parámetro **/volatile** con algunas de las opciones de utilidad del comprobador de controladores **/Flags** y con **/instancia estándar**. No se puede usar **/volatile** con las opciones **/Flags** para la comprobación de [cumplimiento de DDI](https://docs.microsoft.com/windows-hardware/drivers/devtest/ddi-compliance-checking), la comprobación aleatoria de retraso de [Power Framework](https://docs.microsoft.com/windows-hardware/drivers/devtest/concurrency-stress-test), la comprobación de [Storport](https://docs.microsoft.com/windows-hardware/drivers/devtest/dv-storport-verification)o la [comprobación de SCSI](https://docs.microsoft.com/windows-hardware/drivers/devtest/scsi-verification). Para obtener más información, consulte [using volatile Settings](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Comprobador de controladores](https://docs.microsoft.com/windows-hardware/drivers/devtest/driver-verifier)

- [Controlar el comprobador de controladores](https://docs.microsoft.com/windows-hardware/drivers/devtest/controlling-driver-verifier)

- [Supervisar el comprobador de controladores](https://docs.microsoft.com/windows-hardware/drivers/devtest/monitoring-driver-verifier)

- [Usar la configuración volátil](https://docs.microsoft.com/windows-hardware/drivers/devtest/using-volatile-settings)
