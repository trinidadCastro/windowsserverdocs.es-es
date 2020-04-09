---
title: Comandos de Windows
description: Comandos de Windows
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/26/2019
ms.prod: windows-server
ms.openlocfilehash: 9d68e2becbf9c6522be7e1ff6e6742d44f3a8247
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829238"
---
# <a name="windows-commands"></a>Comandos de Windows

Todas las versiones compatibles de Windows (servidor y cliente) tienen un conjunto de comandos de consola Win32 integrados.

En este conjunto de documentación se describen los comandos de Windows que se pueden usar para automatizar tareas mediante scripts o herramientas de scripting.

Para obtener información acerca de un comando específico, en el siguiente menú A-Z, haga clic en la letra con la que comienza el comando y, a continuación, haga clic en el nombre del comando.

[Un](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) | 
[G](#g) | 
[H](#h) | 
[i](#i) |
[J](#j) | 
[K](#k) | 
[L](#l) | 
[M](#m) | 
[N](#n) | 
[O](#o) | 
[p](#p) | 
[Q](#q) | 
[R](#r) | 
[S](#s) | 
[t](#t) | 
[U](#u) | 
[V](#v) | 
[W](#w) | 
[X](#x) | S | Z

## <a name="prerequisites"></a>Requisitos previos

La información contenida en este tema se aplica a:

-   Windows Server 2019
-   Windows Server (Canal semianual)
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="command-shell-overview"></a>Información general del shell de comandos

El shell de comandos era el primer Shell integrado en Windows para automatizar las tareas rutinarias, como la administración de cuentas de usuario o las copias de seguridad nocturnas, con archivos de Batch (. bat). Con Windows Script Host, podía ejecutar scripts más sofisticados en el shell de comandos. Para obtener más información, vea [cscript](cscript.md) o [Wscript](wscript.md). Puede realizar operaciones de forma más eficaz mediante el uso de scripts que puede usar la interfaz de usuario. Los scripts aceptan todos los comandos que están disponibles en la línea de comandos.

Windows tiene dos shells de comandos: el shell de comandos y [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6). Cada Shell es un programa de software que proporciona comunicación directa entre usted y el sistema operativo o la aplicación, lo que proporciona un entorno para automatizar las operaciones de ti.

PowerShell se diseñó para ampliar las funcionalidades del shell de comandos para ejecutar comandos de PowerShell denominados cmdlets. Los cmdlets son similares a los comandos de Windows, pero proporcionan un lenguaje de scripting extensible. Puede ejecutar comandos de Windows y cmdlets de PowerShell en PowerShell, pero el shell de comandos solo puede ejecutar comandos de Windows y no cmdlets de PowerShell.

Para la automatización de Windows más sólida y actualizada, se recomienda usar PowerShell en lugar de comandos de Windows o Windows Script Host para la automatización de Windows. 
> [!NOTE]
>También puede descargar e instalar [PowerShell Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6), la versión de código abierto de PowerShell. 

> [!CAUTION]
> Una modificación incorrecta del Registro puede provocar daños graves en el sistema. Antes de realizar los siguientes cambios en el registro, debe realizar una copia de seguridad de los datos valiosos del equipo.

> [!NOTE]
> Para habilitar o deshabilitar la finalización de nombres de archivos y directorios en el shell de comandos de un equipo o una sesión de inicio de sesión de usuario, ejecute **regedit. exe** y establezca el siguiente **valor de reg_DWOrd**:
> 
> HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\completionChar\ reg_DWOrd
> 
> Para establecer el valor de **reg_DWOrd** , use el valor hexadecimal de un carácter de control para una función determinada (por ejemplo, **0 9** es Tab y **0 08** es el retroceso). La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

## <a name="command-line-reference-a-z"></a>Referencia a-Z de la línea de comandos

Para obtener información acerca de un comando específico de Windows, en el siguiente menú A-Z, haga clic en la letra con la que comienza el comando y, a continuación, haga clic en el nombre del comando.

[Un](#a) |
[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) | 
[G](#g) | 
[H](#h) | 
[i](#i) |
[J](#j) | 
[K](#k) | 
[L](#l) | 
[M](#m) | 
[N](#n) | 
[O](#o) | 
[p](#p) | 
[Q](#q) | 
[R](#r) | 
[S](#s) | 
[t](#t) | 
[U](#u) | 
[V](#v) | 
[W](#w) | 
[X](#x) | S | Z

### <a name="a"></a>A
-   [append](append.md)
-   [arp](arp.md)
-   [assoc](assoc.md)
-   [at](at.md)
-   [atmadm](atmadm.md)
-   [attrib](attrib.md)
-   [auditpol](auditpol.md)
-   [autochk](autochk.md)
-   [autoconv](autoconv.md)
-   [autofmt](autofmt.md)

### <a name="b"></a>B
- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
- [bitsadmin](bitsadmin.md)
  -   [bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [bitsadmin cancel](bitsadmin-cancel.md)
  -   [bitsadmin complete](bitsadmin-complete.md)
  -   [bitsadmin create](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [bitsadmin geterror](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [bitsadmin obtener prioridad](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [bitsadmin gettype](bitsadmin-gettype.md)
  -   [bitsadmin help](bitsadmin-help.md)
  -   [bitsadmin info](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [bitsadmin monitor](bitsadmin-monitor.md)
  -   [bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [bitsadmin reset](bitsadmin-reset.md)
  -   [bitsadmin resume](bitsadmin-resume.md)
  -   [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [bitsadmin suspend](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [Transmisión de bitsadmin](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [bootcfg copy](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [bootcfg debug](bootcfg-debug.md)  
  -   [bootcfg default](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [bootcfg ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C
- [cacls](cacls_1.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  -   [change logon](change-logon.md)
  -   [change port](change-port.md)
  -   [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir_1.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [Cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [convert](convert.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [cscript](cscript.md)

### <a name="d"></a>D
-   [date](date.md)
-   [dcgpofix](dcgpofix.md)
-   [defrag](defrag.md)
-   [del](del.md)
-   [dfsrmig](dfsrmig.md)
-   [diantz](diantz.md)
-   [dir](dir.md)
-   [diskcomp](diskcomp.md)
-   [diskcopy](diskcopy.md)
-   [diskpart](diskpart.md)
-   [diskperf](diskperf.md)
-   [diskraid](diskraid.md)
-   [diskshadow](diskshadow.md)
-   [dispdiag](dispdiag.md)
-   [DNSCmd](Dnscmd.md)
-   [doskey](doskey.md)
-   [driverquery](driverquery.md)

### <a name="e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="f"></a>F
- [fc](fc.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [fsutil behavior](fsutil-behavior.md) 
  -   [fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [fsutil repair](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [fsutil resource](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [fsutil tiering](fsutil-tiering.md)
  -   [fsutil transaction](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
- [FTP](ftp.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>N
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="j"></a>J
-   [jetpack](jetpack.md)

### <a name="k"></a>K
- [klist](klist.md)
- [ksetup](ksetup.md)
  -   [ksetup: setrealm](ksetup-setrealm.md)
  -   [ksetup: mapuser](ksetup-mapuser.md)
  -   [ksetup: addkdc](ksetup-addkdc.md)
  -   [ksetup: delkdc](ksetup-delkdc.md)
  -   [ksetup: addkpasswd](ksetup-addkpasswd.md)
  -   [ksetup: delkpasswd](ksetup-delkpasswd.md)
  -   [ksetup: servidor](ksetup-server.md)
  -   [ksetup: setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [ksetup: removerealm](ksetup-removerealm.md)
  -   [ksetup: dominio](ksetup-domain.md)
  -   [ksetup: ChangePassword](ksetup-changepassword.md)
  -   [ksetup: listrealmflags](ksetup-listrealmflags.md)
  -   [ksetup: setrealmflags](ksetup-setrealmflags.md)
  -   [ksetup: addrealmflags](ksetup-addrealmflags.md)
  -   [ksetup: delrealmflags](ksetup-delrealmflags.md)
  -   [ksetup: dumpstate](ksetup-dumpstate.md)
  -   [ksetup: addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [ksetup: delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [ksetup: setenctypeattr](ksetup-setenctypeattr.md)
  -   [ksetup: getenctypeattr](ksetup-getenctypeattr.md)
  -   [ksetup: addenctypeattr](ksetup-addenctypeattr.md)
  -   [ksetup: delenctypeattr](ksetup-delenctypeattr.md) 
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L
- [label](label.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  -   [logman create](logman-create.md)
  -   [logman query](logman-query.md)
  -   [Logman Start & 124; detener](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [logman update](logman-update.md)
  -   [Logman Import & 124; Portada](logman-import-export.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M
- [macfile](macfile.md)
- [makecab](makecab.md)
- [manage-bde](manage-bde.md)
  -   [Manage-BDE: estado](manage-bde-status.md)
  -   [Manage-BDE: activado](manage-bde-on.md)
  -   [Manage-BDE: desactivado](manage-bde-off.md)
  -   [Manage-BDE: PAUSE](manage-bde-pause.md)
  -   [Manage-BDE: resume](manage-bde-resume.md)
  -   [Manage-BDE: bloqueo](manage-bde-lock.md)
  -   [Manage-BDE: Unlock](manage-bde-unlock.md)
  -   [Manage-BDE: desbloqueo automático](manage-bde-autounlock.md)
  -   [Manage-BDE: protectores](manage-bde-protectors.md)
  -   [Manage-BDE: TPM](manage-bde-tpm.md)
  -   [Manage-BDE: setidentifier](manage-bde-setidentifier.md)
  -   [Manage-BDE: ForceRecovery](manage-bde-forcerecovery.md)
  -   [Manage-BDE: ChangePassword](manage-bde-changepassword.md)
  -   [Manage-BDE: changepin](manage-bde-changepin.md)
  -   [Manage-BDE: changekey](manage-bde-changekey.md)
  -   [Manage-BDE: KeyPackage](manage-bde-keypackage.md)
  -   [Manage-BDE: actualización](manage-bde-upgrade.md)
  -   [Manage-BDE: WipeFreeSpace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [Md](Md.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N
- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [Net print](net-print.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  -   [comando de salida de nslookup](nslookup-exit-command.md)
  -   [comando Nslookup Finger](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup set all](nslookup-set-all.md)
  -   [nslookup set class](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [nslookup set debug](nslookup-set-debug.md)
  -   [nslookup set domain](nslookup-set-domain.md)
  -   [nslookup set port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nslookup set retry](nslookup-set-retry.md)
  -   [nslookup set root](nslookup-set-root.md)
  -   [nslookup set search](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [nslookup set timeout](nslookup-set-timeout.md)
  -   [nslookup set type](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O
-   [openfiles](openfiles.md)

### <a name="p"></a>P
-   [pagefileconfig](pagefileconfig.md)
-   [path](path.md)
-   [pathping](pathping.md)
-   [pause](pause.md)
-   [pbadmin](pbadmin.md)
-   [pentnt](pentnt.md)
-   [perfmon](perfmon.md)
-   [ping](ping.md)
-   [pnpunattend](pnpunattend.md)
-   [pnputil](pnputil.md)
-   [popd](popd.md)
-   [PowerShell](PowerShell.md)
-   [PowerShell_ise](PowerShell_ise.md)
-   [print](print.md)
-   [prncnfg](prncnfg.md)
-   [prndrvr](prndrvr.md)
-   [prnjobs](prnjobs.md)
-   [prnmngr](prnmngr.md)
-   [prnport](prnport.md)
-   [prnqctl](prnqctl.md)
-   [prompt](prompt.md)
-   [pubprn](pubprn.md)
-   [pushd](pushd.md)
-   [pushprinterconnections](pushprinterconnections.md)

### <a name="q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [consulta](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="r"></a>R
- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [reg](reg.md)
  -   [REG Add](reg-add.md)
  -   [REG-comparar](reg-compare.md)
  -   [copiar reg](reg-copy.md)
  -   [Eliminar registro](reg-delete.md)
  -   [REG Export](reg-export.md)
  -   [REG Import](reg-import.md)
  -   [carga de reg](reg-load.md)
  -   [REG (consulta)](reg-query.md)
  -   [REG restore](reg-restore.md)
  -   [Guardar registro](reg-save.md)
  -   [descarga de reg](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem](rem.md)
- [ren](ren.md)
- [rename](rename.md)
- [repair-bde](repair-bde.md)
- [replace](replace.md)
- [reset session](reset-session.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route_ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S
- [schtasks](schtasks.md)
- [scwcmd](Scwcmd.md)
  -   [scwcmd: analizar](scwcmd-analyze.md)
  -   [scwcmd: configurar](scwcmd-configure.md)
  -   [scwcmd: registrar](scwcmd-register.md) 
  -   [scwcmd: Rollback](scwcmd-rollback.md) 
  -   [scwcmd: transformación](scwcmd-transform.md) 
  -   [scwcmd: ver](scwcmd-view.md) 
- [secedit](secedit.md)
  -   [secedit: ANALYZE](secedit-analyze.md)
  -   [secedit: configurar](secedit-configure.md)
  -   [secedit: Export](secedit-export.md)
  -   [secedit: generaterollback](secedit-generaterollback.md)
  -   [secedit: importar](secedit-import.md)
  -   [secedit: Validate](secedit-validate.md)
- [serverceipoptin](serverceipoptin.md)
- [Servermanagercmd](Servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [set](set_1.md)
- [setlocal](setlocal.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shutdown](shutdown.md)
- [sort](sort.md)
- [start](start.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)

### <a name="t"></a>T
-   [takeown](takeown.md)
-   [tapicfg](tapicfg.md)
-   [taskkill](taskkill.md)
-   [tasklist](tasklist.md)
-   [tcmsetup](tcmsetup.md)
-   [telnet](telnet.md)
-   [tftp](tftp.md)
-   [time](time.md)
-   [timeout](timeout_1.md)
-   [title](title_1.md)
-   [tlntadmn](tlntadmn.md)
-   [tpmvscmgr](tpmvscmgr.md)
-   [tracerpt](tracerpt_1.md)
-   [tracert](tracert.md)
-   [tree](tree.md)
-   [tscon](tscon.md)
-   [tsdiscon](tsdiscon.md)
-   [tsecimp](tsecimp_1.md)
-   [tskill](tskill.md)
-   [tsprof](tsprof.md)
-   [type](type.md)
-   [typeperf](typeperf.md)
-   [tzutil](tzutil.md)

### <a name="u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="w"></a>W
- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  -   [Wbadmin habilitar copia de seguridad](wbadmin-enable-backup.md)
  -   [Wbadmin deshabilitar copia de seguridad](wbadmin-disable-backup.md)
  -   [copia de seguridad de inicio de Wbadmin](wbadmin-start-backup.md)
  -   [trabajo de detención de Wbadmin](wbadmin-stop-job.md)
  -   [versiones de Wbadmin get](wbadmin-get-versions.md)
  -   [elementos get de Wbadmin](wbadmin-get-items.md)
  -   [recuperación de inicio de Wbadmin](wbadmin-start-recovery.md)
  -   [Estado de Wbadmin get](wbadmin-get-status.md)
  -   [obtener discos de Wbadmin](wbadmin-get-disks.md)
  -   [Inicio de systemstaterecovery de Wbadmin](wbadmin-start-systemstaterecovery.md)
  -   [Inicio de systemstatebackup de Wbadmin](wbadmin-start-systemstatebackup.md)
  -   [Wbadmin Delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [Wbadmin Start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [Wbadmin restore Catalog](wbadmin-restore-catalog.md)
  -   [Wbadmin Delete Catalog](wbadmin-delete-catalog.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [wmic](wmic.md)
- [wscript](wscript.md)

### <a name="x"></a>X
-   [xcopy](xcopy.md)
