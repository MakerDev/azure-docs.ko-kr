---
title: Linux VM이 Grub 복구로 부팅 됨
description: 가상 머신이 복구 콘솔에 들어간 가상 머신을 부팅 하지 못했습니다.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/28/2019
ms.author: tiag
ms.openlocfilehash: 5a2fd7fcfdae8559bfb39bffff7c73c7082a86aa
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543285"
---
# <a name="linux-vm-boots-to-grub-rescue"></a>Linux VM이 Grub 복구로 부팅 됨

VM (가상 컴퓨터)에서 복구 콘솔을 입력 한 것으로 확인 되었습니다. 이 문제는 Linux VM이 최근에 커널 업그레이드와 같은 커널 변경 내용을 적용 했 고 부팅 프로세스 중 커널 오류로 인해 더 이상 제대로 시작 되지 않을 때 발생 합니다. 부팅 프로세스 중에 부팅 로더에서 Linux 커널을 찾아 부팅 제어를 해제 하려고 하면 전달에 실패 하는 경우 VM이 복구 콘솔에 들어갑니다.

나중에 VM에 연결할 수 없는 경우 Azure Portal의 부트 진단 블레이드를 사용 하 여 VM의 스크린샷을 볼 수 있습니다. 이렇게 하면 문제를 진단하고 유사한 부트 오류가 원인인지 확인하는 데 도움이 됩니다.

## <a name="recommended-steps"></a>권장되는 단계

수신 하는 오류에 따라 아래의 완화 단계를 수행 합니다.

### <a name="error---unknown-filesystem"></a>오류-알 수 없는 파일 시스템

* **알 수 없는**오류 파일 시스템을 가져오는 경우이 오류는 부팅 파티션의 파일 시스템 손상 또는 잘못 된 커널 구성으로 인해 발생할 수 있습니다.

   * 파일 시스템 문제에 대 한 자세한 내용은 [Linux 복구: 파일 시스템 오류로 인해 LINUX VM에 SSH 할 수 없음 (fsck, inode)](/archive/blogs/linuxonazure/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes)문서의 단계를 따르세요.
   * 커널 문제에 대 한 자세한 내용은 [커널 관련 부팅 문제에서 Azure linux 가상 머신을 복구 하는 방법](https://support.microsoft.com/help/4091524/how-recover-azure-linux-vm-from-kernel-related-boot-related-issues)또는 [Linux 복구: chroot를 사용 하 여 커널 문제와 관련 된 부팅 이외의 문제 해결](http://linuxonazure.azurewebsites.net/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/)문서의 단계를 따르세요.
   
### <a name="error---file-not-found"></a>오류-파일을 찾을 수 없음

* 오류 **15: 파일을 찾을 수 없거나 초기 RAM 디스크 또는 초기 RAM 디스크** 를 **찾을 수 없는**경우 다음 단계를 수행 합니다.

    * 누락 된 파일의 `/boot/grub2/grub.cfg` 경우 `initrd/initramfs` 다음 프로세스를 진행 합니다.

    1. `/etc/default/grub`존재 하 고 올바른 설정이 있는지 확인 하십시오. 기본 설정에 대해 잘 모르는 경우 작동 하는 VM으로 확인할 수 있습니다.

    2. 그런 다음, 다음 명령을 실행 하 여 구성을 다시 생성 합니다.`grub2-mkconfig -o /boot/grub2/grub.cfg`

   * 파일이 없는 경우 `/boot/grub/menu.lst` 이 오류는 이전 OS 버전 (**RHEL**, **Centos** , **14.04 및 Ubuntu**)에 대 한 것 이므로 명령이 다를 수 있습니다. 이전 서버를 실행 하 고 올바른 명령이 제공 되는지 테스트 해야 합니다.

### <a name="error---no-such-partition"></a>오류-해당 파티션 없음

* **이러한 파티션이 없는**오류를 발생 하는 경우 [OS 드라이브를 확장 한 후 VM을 시작 하는 동안 "해당 파티션 없음" 오류가](/archive/blogs/shwetanayak/case-scenario-no-such-partition-error-while-trying-to-start-the-vm-after-attempting-to-extend-the-os-drive)발생 하는 경우를 참조 하세요.

### <a name="error---grubcfg-file-not-found"></a>오류-grub. cfg 파일을 찾을 수 없습니다.

* **/Boot/grub2/grub.cfg 파일을 찾을 수 없는**오류가 발생 하는 경우 다음 단계를 수행 합니다.

    * 누락 된 파일의 `/boot/grub2/grub.cfg` 경우 `initrd/initramfs` 다음 프로세스를 진행 합니다.

    1. `/etc/default/grub`존재 하 고 올바른 설정이 있는지 확인 하십시오. 기본 설정에 대해 잘 모르는 경우 작동 하는 VM으로 확인할 수 있습니다.

    2. 그런 다음, 다음 명령을 실행 하 여 구성을 다시 생성 `grub2-mkconfig -o /boot/grub2/grub.cfg` 합니다..

   * 파일이 없는 경우 `/boot/grub/menu.lst` 이 오류는 이전 OS 버전 (**RHEL**, **Centos** , **14.04 및 Ubuntu**)에 대 한 것 이므로 명령이 지연 될 수 있습니다. 이전 서버를 실행 하 고 테스트 하 여 올바른 명령이 제공 되는지 확인 합니다.

## <a name="next-steps"></a>다음 단계

* [Azure 가상 머신 에이전트 개요](../extensions/agent-windows.md)
* [Windows용 가상 머신 확장 및 기능](../extensions/features-windows.md)
