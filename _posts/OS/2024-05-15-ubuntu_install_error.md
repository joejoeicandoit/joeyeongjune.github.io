---
layout: posts
title: "[Ubunt] hp 중고 노트북에서 부팅이 안되던 문제 해결" 
date: 2024-05-15 02:48:00 +0900
category: os
mermaid: true
author_profile: true
sidebar:
    nav: "saidebar-category"
---

### 배경

linux 공부 해보겠다고 중고로 오래된 노트북(hp EliteBook) 을 하나 구입 해서,  
Rocky linux, Oracle linux, Ubuntu Serve 를 설치 해봤다.  
그런데, 유독 Ubuntu 제대로 부팅이 안되는 현상이 있었다.  

```
--- Ubuntu 설치 후에 부팅 하면 표시되던 메세지 ---

BootDevice Not Found

Plase install on operating system on your hard disk.

Hard Disk - (3F0)

F2 System Diagnostics

For more information, please visit: www.hp.com\go\techcenter\startup
```

Ubuntu 설치 할때 파티션 설정까지 문제 없이 진행 했는데 `BootDevice Not Found` 에러 메세지라니;;  
GPT 에게 물어 봐도 이렇다할 답변을 얻을수 없었다.  

```
--- Chat GPT 가 알려줘던 답변 ---

이러한 오류는 주로 하드 디스크가 올바르게 감지되지 않거나 부팅 가능한 운영 체제가 설치되지 않았음을 나타냅니다. 이 문제를 해결하기 위해 다음 단계를 시도할 수 있습니다.

BIOS 설정 확인: 먼저 컴퓨터를 재부팅하고 BIOS 설정으로 들어가서 하드 디스크가 올바르게 감지되었는지 확인합니다. BIOS에서 하드 디스크가 나타나지 않는다면, 하드웨어 연결이나 하드웨어 자체의 문제일 가능성이 높습니다.

부팅 순서 확인: BIOS 설정에서 부팅 순서를 확인하고 하드 디스크가 올바른 순서로 설정되어 있는지 확인합니다. 하드 디스크가 부팅 순서에서 올바르게 설정되어 있지 않으면, 컴퓨터가 하드 디스크에서 운영 체제를 찾을 수 없을 것입니다.

하드 디스크 연결 확인: 컴퓨터의 하드 디스크가 올바르게 연결되어 있는지 확인합니다. 때때로 하드 디스크가 떨어져 있거나 제대로 연결되지 않은 경우가 있습니다.

다른 부팅 장치로 테스트: USB 플래시 드라이브 등 다른 부팅 가능한 장치를 사용하여 시스템을 부팅해 봅니다. 이렇게 하면 하드 디스크에 문제가 있는지 또는 다른 문제가 있는지 확인할 수 있습니다.

하드 디스크 장애 확인: 하드 디스크의 물리적 또는 논리적인 장애가 있는지 확인할 수 있는 도구를 사용합니다. 예를 들어, Ubuntu Live CD나 USB를 사용하여 시스템을 부팅하고 디스크 유틸리티를 실행하여 하드 디스크의 상태를 확인할 수 있습니다.

재설치: 위의 단계들을 시도한 후에도 문제가 해결되지 않는다면, 하드 디스크에 문제가 있을 수 있으므로 새로운 하드 디스크로 교체하거나 운영 체제를 재설치하는 것이 좋습니다.
```

다른 linux OS 는 문제가 없었기에 일단 Ubuntu는 포기 하고 있었는데  
마인크래프트 서버 구축을 위해 다시 Ubuntu 를 마주해야만 하는 상황이 오고,  
결국은 하루 날 잡아서 트러블 슈팅을 해봤다.  
그러다 한 해외 유저가 남긴 "Ubuntu OS가 CSM을 사용 하는 UEFI 파일 시스템에서 제대로 동작 하지 않는 경험을 했다" 라는 코멘트.  
최종적으로 아래와 같이 작업을 진행했고, 문제 없이 Ubuntu 설치가 되어, 기록을 남겨 놓았다.  


### 재설치 진행 순서

- 1.설치 디스크 작성
  - 1-1.rufus 툴을 사용해, USB에 Ubuntu의 설치 디스크를 작성
  - 1-2.파티션 구성은 GPT 로 설정 한다
  - 1-3.타켓 시스템 UEFI(CSM なし) 로 설정 한다
- 2.노트북 BIOS 설정
  - 2-1.BIOS 설정으로 들어간다. 
  - ※BIOS 설정 화면으로 들어가는 방법은 컴퓨터 마다 조금 다르지만 일반적으론 전원을 켜고, ESC, F9, F12 키중 하나를 눌러주면 될것이다.
  - 2-2.레거시 BIOS 를 UFEI(CSM なし) 로 변경
- 3.Ubuntu 설치
  - 부팅 순서가 하드 디스크를 먼저 읽고 있다면 BIOS에서 USB로 변경 해준다.
  - Ubuntu 설치 화면이 보여지면 나머진 사용자 입맛에 맞게 설정해서 설치를 진행 한다.
  - 혹시, 설정하는걸 잘 모르겠다 한다면 대부분을 디폴트 설정으로 진행 하면 된다.
- 4.재부팅 후 완료