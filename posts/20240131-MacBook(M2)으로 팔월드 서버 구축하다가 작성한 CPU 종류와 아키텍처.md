# 들어가며

요즘 Steam에 Early Access로 출시된 팔월드(또는 팰월드, Palworld)가 핫한 게임인 듯하다. 디아블로4 출시 당시 game-porting-toolkit을 통해 MacBook(M2)에서 플레이한 기억을 떠올려 팔월드 또한 맥북에서 실행할 수 있지 않을까?라는 생각으로 여러 시도를 하였으나, 전부 수포가 되었다.

> 결국 오랫동안 잠자고 있던 다른 노트북을 꺼내 Windows 11로 업그레이드 하고 팔월드를 설치 후 플레이하였다.

주변에도 팔월드 플레이어가 하나둘씩 늘어가기 시작했고, 멀티 플레이를 하기 위해 팔월드 서버를 구축하고자 하였다. 맥북에서는 팔월드를 플레이할 수 없으니 맥북으로 팔월드 서버를 구축하고자 Docker Ubuntu22.04 이미지를 활용하여 다양한 시도를 하였으나, 이 또한 수포로 돌아갔다.

> GitHub에 ARM에서 SteamCMD를 실행시키는 방법 있긴 하나, 스스로 해결해보고 싶어서 시도는 해보지 않았다. 누군가에게 필요할 수도 있으니 아래에 링크를 남겨놓겠다.

- [GitHub/FEX-Emu/FEX](https://github.com/FEX-Emu/FEX)
- [GitHub/TeriyakiGod/steamcmd-docker-arm64](https://github.com/TeriyakiGod/steamcmd-docker-arm64)

팔월드 서버를 구축하려면 steamCMD를 설치해야 했는데, steamCMD가 arm를 지원하지 않는다는 것이 가장 큰 이슈였다. 해당 정보를 접한 후 (정말 뜬금없지만) 문득 CPU 종류, CPU 아키텍처에 대해서 정리해야겠다는 생각이 들어서 본 글을 작성하였다.

# 1. CPU 종류

CPU 아키텍처를 다루기 전에 CPU 종류(제조사) 먼저 알아보자. CPU 종류는 대표적으로 Intel, AMD, ARM이 있으며, 이들은 모두 CPU 제조사명과 동일하다.

| CPU   | 지원 OS 또는 계열                  | 성능                    |
| ----- | ---------------------------------- | ----------------------- |
| Intel | Windows, Linux, Mac, Unix, Solaris | PC, Server, Workstation |
| SPARC | Unix, Solaris                      | PC, Server              |
| AMD   | Windows, Linux, Mac, Unix, Solaris | PC, Server, Workstation |
| ARM   | Linux, Mac                         | 스마트폰, 저전력 PC     |

> SPARC는 대중적으로 이용되지 않으므로 본 글에서 다루지 않았다.

## 1.1. Intel

가장 대중적이고 오랜기간 사용되어 온 만큼 대부분의 소프트웨어는 Intel에 맞춰 개발되어 왔다. x86 아키텍처의 CPU를 제조한다.

## 1.2. AMD

인텥의 x86 호환 CPU를 제조한다. 흔히 암드라고도 불리우며, 인텔과 상호 라이센스를 체결했다. 따라서, 인텔 호환 CPU는 인텔과 AMD만 제조가능하다.

## 1.3. ARM

ARM은 인텔, 암드와는 전혀 다른 아키텍처를 사용한다. 과거에는 저렴한 가격, 저전력, 단순한 아키텍처라는 특성으로 모바일 등의 소형기기에 사용되었으나, 기술이 발전함에 따라 PC에 근접한 수준에 이르렀고 PC용 ARM이 등장하였다. 앞서 언급한 Intel 호환 CPU와는 다르게 AMD 호환 CPU는 기술력만 있다면 누구든 커스텀한 칩을 제조 후 판매 가능하다고 한다.

> 참고 : Apple이 자체 개발한 M1/M2 CPU가 가장 대표적인 사례이다.

# 2. CPU 아키텍처

CPU 아키텍처 종류에는 x86, x86_64(amd64), arm, arm64가 있다. 이들을 호환 가능 여부로 그룹화하면 x86 계열, arm 계열로 구분할 수 있다.

## 2.1. x86 계열

- 32bit, 64bit로 나뉘며, 명칭은 각각 x86, x86_64이다.
- 현존하는 대부분의 소프트웨어는 x86을 지원한다.
- x86_64는 인텔과 상호 라이센스를 체결한 AMD가 제조한 Intel 기반 64bit CPU이며, amd64라고도 한다.
- Windows, Linux, MacOS(BigSur까지) 운영체제를 지원한다.

> 참고 : 인텔이 오랜 기간 이용되어 온 탓에 x86은 32bit CPU의 대표명사처럼 불리운다.

## 2.2. arm 계열

- 32bit, 64bit로 나뉘며, 명칭은 각각 arm, arm64이다.
- x86과는 전혀 다른 아키텍처이므로 x86 계열과 호환이 안 된다.
- Lunux, MacOS(Monterey부터), Android, iOS 운영체제를 지원한다.
- 스마트폰, 공유기 등 작은 기기에서 최적의 퍼포먼스를 내야하는 경우 사용한다.

# 3. 정리

스크립트 언어로 개발된 프로그램을 제외한 모든 프로그램은 컴파일 된 바이너리이며, 컴파일 당시 CPU의 종류에 따라 결과물이 정해진다. 그러므로 x86 기반 머신에서 빌드한 라이브러리나 컴파일한 프로그램은 arm에서 동작하지 않으며, 반대의 경우도 마찬가지이다.

> 참고 : x86_64는 하위 호환을 지원하므로 x86에서 빌드 또는 컴파일 한 결과물은 x86_64에서 동작한다.

모바일 애플리케이션은 arm 기반으로 개발된다. 반면, PC용 애플리케이션은 x86 기반으로 개발된다(Mac M1/M2 제외). 그렇기에 당연히 같은 애플리케이션이라고 하더라도 모바일 버전과 PC 버전이 별개로 존재한다. Mac M1/M2에서 일부 x86 기반 프로그램을 실행할 수 있는 이유는 Rosseta라는 에뮬레이터로 구동되기 때문이다.

```bash
arch -x86_64
```

이는 arm 머신 내부에 x86 가상머신을 만들고, 그 안에서 x86 기반 애플리케이션을 실행하는 방식으로 동작한다. 그렇기에 M1/M2에서 x86 프로그램을 실행시키면 약간의 성능 저하가 발생할 수 있다. 반면, arm 기반으로 개발된 모든 iOS 애플리케이션은 Mac M1/M2에서 에물레이터 없이 네이티브 성능으로 실행 가능하다.

실제 개발하거나 Docker를 이용할때에도 CPU 아키텍처를 확인해야 하는 경우가 종종 있다. 대부분의 프로그래밍 언어는 x86, arm을 지원하나, Node.js로 개발할 때 외부 라이브러리를 사용하는 경우 Mac(M1/M2)에서는 동작하는 코드가 AMD 노트북에서는 동작하지 않는 경우가 있기에 개발하려고 하는 외부 모듈 또는 라이브러리가 있다면 개발환경의 아키텍처에 맞는 라이브러리를 확인 후 개발해야한다.

> 개발자가 결국 컴퓨터를 잘 알아야만 하는 이유가 여기 있는 것 같다.

# 마치며

백수 기간 마지막 주, 며칠 간 밤을 지새우며 팔월드를 해보았다. 개인적으로는 여태 해왔던 생존형 크래프트 게임 중 탑인 것 같다.

> 포켓몬스터, 스톤에이지, 배틀그라운드, 마인크래프트를 전부 섞은 종합 게임인 느낌이 들었다.

아직 얼리엑세스 단계인 탓에 많은 버그가 있지만 그럼에도 시간가는 줄 모르고 즐겼던 것 같다. 언제 정식 출시될 지는 모르겠지만 정식 출시 이후 생각나면 다시 플레이할 의향이 있다.

> 현재는 모든 컨텐츠를 즐겨보았고, 조금 질리기도 한 상태이다. 전설 팰과 장비를 맞추고 펠 강화 이후 4천왕 깨고나면 할 게 없다.

얼마 남지 않은 백수 기간 동안 게임 플레이는 하지 않겠지만 arm에서 steamCMD를 구동시키는 방법에 대해서는 알고 싶기에 조금 더 탐구해보려고 한다.

# 참고자료

- [PalWorld Dedicated Server Guide](https://tech.palworldgame.com/dedicated-server-guide)
- [Developer Community - SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD)
- [Steamcmd on ARM (with box86 and box64) issues](https://www.reddit.com/r/linux_gaming/comments/v6b9n5/steamcmd_on_arm_with_box86_and_box64_issues/)
- [위키백과 - CPU 아키텍처의 종류](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98#CPU_%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EC%9D%98_%EC%A2%85%EB%A5%98)
- [나무위키 - CPU](https://namu.wiki/w/CPU)
- [Velog - 이제는 개발자도 CPU 아키텍처를 구분해야 합니다.](https://velog.io/@480/%EC%9D%B4%EC%A0%9C%EB%8A%94-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%8F%84-CPU-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%EB%A5%BC-%EA%B5%AC%EB%B6%84%ED%95%B4%EC%95%BC-%ED%95%A9%EB%8B%88%EB%8B%A4)
