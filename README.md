# 불나비 PX4
본 레포는 불나비의 PX4 개발을 위해 제작되었으며, PX4-AutoPilot의 v1.14.0 버전을 그대로 가져온 것이다. PX4 코드를 직접 변형할 예정이라면 아래 가이드라인을 꼭 따라야 한다.
#### 개발 환경
> 1.Ubuntu 20.04  
> 2. ROS2 foxy
## 기본 설치 가이드라인
사용을 위해서는, 다음 과정을 거친다.
1. 현재 있는 Bulnabi-SNU repository를 fork 버튼을 이용해 본인 계정으로 fork한다.
2. 본인 계정 repository로 fork를 했다면, 본인의 local computer 디렉토리에서 git clone https://github.com/본인이름!!!/Bulnabi_PX4.git 명령어를 활용해 본인의 local computer로 clone한다. (초록 Code 라는 버튼 누르면 https 주소가 나옴)
3. clone을 했다면 본인 컴퓨터 폴더에 Bulnabi_PX4라는 폴더가 생성되었을 것이다.
4. 
```
cd Bulnabi_PX4
```
명령어를 사용해 Bulnabi_PX4 폴더 안으로 들어간다.
5. 여기서 잘 연동이 되었는지 git remote -v 라는 명령어를 사용해 확인해 볼 수 있을 것이다. origin 뒤에 본인 계정의 주소가 뜰 것이다. 이제, 협업을 위해 Bulnabi-SNU/Bulnabi_PX4를 upstream으로 추가할 것이다. upstream은 불나비의 공식 PX4가 있는 곳이라고 생각하면 된다.
```
git remote add upstream https://github.com/Bulnabi-SNU/Bulnabi_PX4.git
```
Bulnabi 공식 레포가 upstream으로 잘 등록되었다면, git remote -v라는 명령어를 실행하면 다음과 같이 출력될 것이다.
```
origin	https://github.com/jangminhyuk/Bulnabi_PX4.git (fetch)
origin	https://github.com/jangminhyuk/Bulnabi_PX4.git (push)
upstream	https://github.com/Bulnabi-SNU/Bulnabi_PX4.git (fetch)
upstream	https://github.com/Bulnabi-SNU/Bulnabi_PX4.git (push)
```
6. 이제 upstream의 tag를 가져온다.
```
git fetch --tags upstream
```
7. 여기까지 잘 따라왔다면, 현재 있는 폴더 (Bulnabi_PX4)에서
```
bash .Tools/setup/ubuntu.sh
```
를 실행한다. 조금 시간이 걸린 뒤에 완료가 되면, ubuntu를 로그아웃했다가 다시 접속하면 된다.
8. 다시 접속한 뒤에 cd Bulnabi_PX4라는 명령어를 통해 다시 Bulnabi_PX4 폴더로 들어온다. 이후, 시뮬레이션 피피티 나온 대로 잘 따라해서, 
```
make px4_sitl gazebo-classic
```
위 명령이 문제 없이 실행되면 된다.

## 협업 가이드라인

이제 본인 컴퓨터에서 코딩 등 작업을 마친 뒤에는 git add 어쩌구, git commit -m "어쩌구", git push origin release/1.14(또는 본인 branch 명) 등의 과정을 거쳐 본인 계정의 레포에 업로드를 하게 된다. 업로드 뒤에는 본인 레포에 들어가면 Pull-Request라는 버튼이 활성화 되는데, 이때 리뷰어를 지정해서 올리고, 리뷰어가 ok하면 본인의 작업물을 Bulnabi-SNU/Bulnabi_PX4에 merge하게 된다.

Git 관련 사항 (Conflict)등은 인터넷에서 꼭 충분히 학습을 하도록 한다. 이후에는 시뮬레이션 세미나 ppt 나온 그대로 따라해보면 좋을 것 같다.


### 자주 쓰는 명령어
git branch
git checkout
git log -5 (개수)
git status


### 기체에 펌웨어를 업로드 하는 방법
기존에는 QGround Control을 연결한 뒤, Vehicle Setting -> Firmware에 들어간 뒤, USB를 뺐다가 다시 끼면 Stable한 px4 버전이 자동으로 픽스호크 보드에 업로드가 되었다.
이제는, Bulnabi_px4를 사용할 것이기 때문에 위 방법 대신 아래와 같은 방법을 사용한다.
우선
```
cd Bulnabi_PX4
```
터미널에서 위 명령어를 통해 Bulnabi_PX4 폴더로 이동한다.
그 뒤에, 픽스호크 보드 제품 버전에 맞는 명령어를 실행한다. https://docs.px4.io/main/en/dev_setup/building_px4.html
```
make px4_fmu-v6x_default
```
build가 완료되면,
```
make px4_fmu-v6x_default upload
```
그럼 bootloader 어쩌고라는 말이 뜰 텐데, 그 다음 usb를 뺐다가 끼면 기체에 우리의 Bulnabi_PX4 펌웨어가 업로드되게 된다.
그 이후는 기존에 QGroundControl 사용 방법과 동일하다. (펌웨어는 이미 업로드했으므로, 그 부분을 제외하고)


## Tip
현재 git을 처음 설치한 상태면, 현재 내가 어떤 branch에 있는지를 매번 git branch 명령어를 통해 확인해야한다. 아래 링크를 참고해서 bashrc에 추가를 하면 항상 현재 브랜치 명을 표시해줘서 편리하다.
https://siyoon210.tistory.com/7
혹시 gedit(그냥 메모장 같은 것)이 설치되어 있지 않으면
```
sudo apt install gedit
```
```
sudo gedit ~/.bashrc
```
맨 아래에 아래 내용을 추가한다.
```
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
```
터미널을 재시작하고, git이 연동되어 있는 폴더에 들어가면 자동으로 현재 브랜치 명을 표시해줄 것이다.
# PX4 Drone Autopilot

[![Releases](https://img.shields.io/github/release/PX4/PX4-Autopilot.svg)](https://github.com/PX4/PX4-Autopilot/releases) [![DOI](https://zenodo.org/badge/22634/PX4/PX4-Autopilot.svg)](https://zenodo.org/badge/latestdoi/22634/PX4/PX4-Autopilot)

[![Nuttx Targets](https://github.com/PX4/PX4-Autopilot/workflows/Nuttx%20Targets/badge.svg)](https://github.com/PX4/PX4-Autopilot/actions?query=workflow%3A%22Nuttx+Targets%22?branch=master) [![SITL Tests](https://github.com/PX4/PX4-Autopilot/workflows/SITL%20Tests/badge.svg?branch=master)](https://github.com/PX4/PX4-Autopilot/actions?query=workflow%3A%22SITL+Tests%22)

[![Discord Shield](https://discordapp.com/api/guilds/1022170275984457759/widget.png?style=shield)](https://discord.gg/dronecode)

This repository holds the [PX4](http://px4.io) flight control solution for drones, with the main applications located in the [src/modules](https://github.com/PX4/PX4-Autopilot/tree/main/src/modules) directory. It also contains the PX4 Drone Middleware Platform, which provides drivers and middleware to run drones.

PX4 is highly portable, OS-independent and supports Linux, NuttX and MacOS out of the box.

* Official Website: http://px4.io (License: BSD 3-clause, [LICENSE](https://github.com/PX4/PX4-Autopilot/blob/main/LICENSE))
* [Supported airframes](https://docs.px4.io/main/en/airframes/airframe_reference.html) ([portfolio](https://px4.io/ecosystem/commercial-systems/)):
  * [Multicopters](https://docs.px4.io/main/en/frames_multicopter/)
  * [Fixed wing](https://docs.px4.io/main/en/frames_plane/)
  * [VTOL](https://docs.px4.io/main/en/frames_vtol/)
  * [Autogyro](https://docs.px4.io/main/en/frames_autogyro/)
  * [Rover](https://docs.px4.io/main/en/frames_rover/)
  * many more experimental types (Blimps, Boats, Submarines, High altitude balloons, etc)
* Releases: [Downloads](https://github.com/PX4/PX4-Autopilot/releases)


## Building a PX4 based drone, rover, boat or robot

The [PX4 User Guide](https://docs.px4.io/main/en/) explains how to assemble [supported vehicles](https://docs.px4.io/main/en/airframes/airframe_reference.html) and fly drones with PX4.
See the [forum and chat](https://docs.px4.io/main/en/#getting-help) if you need help!


## Changing code and contributing

This [Developer Guide](https://docs.px4.io/main/en/development/development.html) is for software developers who want to modify the flight stack and middleware (e.g. to add new flight modes), hardware integrators who want to support new flight controller boards and peripherals, and anyone who wants to get PX4 working on a new (unsupported) airframe/vehicle.

Developers should read the [Guide for Contributions](https://docs.px4.io/main/en/contribute/).
See the [forum and chat](https://docs.px4.io/main/en/#getting-help) if you need help!


### Weekly Dev Call

The PX4 Dev Team syncs up on a [weekly dev call](https://docs.px4.io/main/en/contribute/).

> **Note** The dev call is open to all interested developers (not just the core dev team). This is a great opportunity to meet the team and contribute to the ongoing development of the platform. It includes a QA session for newcomers. All regular calls are listed in the [Dronecode calendar](https://www.dronecode.org/calendar/).


## Maintenance Team

Note: This is the source of truth for the active maintainers of PX4 ecosystem.

| Sector | Maintainer |
|---|---|
| Founder | [Lorenz Meier](https://github.com/LorenzMeier) |
| Architecture | [Daniel Agar](https://github.com/dagar) / [Beat Küng](https://github.com/bkueng)|
| State Estimation | [Mathieu Bresciani](https://github.com/bresch) / [Paul Riseborough](https://github.com/priseborough) |
| OS/NuttX | [David Sidrane](https://github.com/davids5) |
| Drivers | [Daniel Agar](https://github.com/dagar) |
| Simulation | [Jaeyoung Lim](https://github.com/Jaeyoung-Lim) |
| ROS2 | [Beniamino Pozzan](https://github.com/beniaminopozzan) |
| Community QnA Call | [Ramon Roche](https://github.com/mrpollo) |
| [Documentation](https://docs.px4.io/main/en/) | [Hamish Willee](https://github.com/hamishwillee) |

| Vehicle Type | Maintainer |
|---|---|
| Multirotor | [Matthias Grob](https://github.com/MaEtUgR) |
| Fixed Wing | [Thomas Stastny](https://github.com/tstastny) |
| Hybrid VTOL | [Silvan Fuhrer](https://github.com/sfuhrer) |
| Boat | x |
| Rover | x |

See also [maintainers list](https://px4.io/community/maintainers/) (px4.io) and the [contributors list](https://github.com/PX4/PX4-Autopilot/graphs/contributors) (Github). However it may be not up to date.

## Supported Hardware

Pixhawk standard boards and proprietary boards are shown below (discontinued boards aren't listed).

For the most up to date information, please visit [PX4 user Guide > Autopilot Hardware](https://docs.px4.io/main/en/flight_controller/).

### Pixhawk Standard Boards

These boards fully comply with Pixhawk Standard, and are maintained by the PX4-Autopilot maintainers and Dronecode team

* FMUv6X and FMUv6C
  * [CUAV Pixahwk V6X (FMUv6X)](https://docs.px4.io/main/en/flight_controller/cuav_pixhawk_v6x.html)
  * [Holybro Pixhawk 6X (FMUv6X)](https://docs.px4.io/main/en/flight_controller/pixhawk6x.html)
  * [Holybro Pixhawk 6C (FMUv6C)](https://docs.px4.io/main/en/flight_controller/pixhawk6c.html)
  * [Holybro Pix32 v6 (FMUv6C)](https://docs.px4.io/main/en/flight_controller/holybro_pix32_v6.html)
* FMUv5 and FMUv5X (STM32F7, 2019/20)
  * [Pixhawk 4 (FMUv5)](https://docs.px4.io/main/en/flight_controller/pixhawk4.html)
  * [Pixhawk 4 mini (FMUv5)](https://docs.px4.io/main/en/flight_controller/pixhawk4_mini.html)
  * [CUAV V5+ (FMUv5)](https://docs.px4.io/main/en/flight_controller/cuav_v5_plus.html)
  * [CUAV V5 nano (FMUv5)](https://docs.px4.io/main/en/flight_controller/cuav_v5_nano.html)
  * [Auterion Skynode (FMUv5X)](https://docs.auterion.com/avionics/skynode)
* FMUv4 (STM32F4, 2015)
  * [Pixracer](https://docs.px4.io/main/en/flight_controller/pixracer.html)
  * [Pixhawk 3 Pro](https://docs.px4.io/main/en/flight_controller/pixhawk3_pro.html)
* FMUv3 (STM32F4, 2014)
  * [Pixhawk 2](https://docs.px4.io/main/en/flight_controller/pixhawk-2.html)
  * [Pixhawk Mini](https://docs.px4.io/main/en/flight_controller/pixhawk_mini.html)
  * [CUAV Pixhack v3](https://docs.px4.io/main/en/flight_controller/pixhack_v3.html)
* FMUv2 (STM32F4, 2013)
  * [Pixhawk](https://docs.px4.io/main/en/flight_controller/pixhawk.html)

### Manufacturer supported

These boards are maintained to be compatible with PX4-Autopilot by the Manufacturers.

* [ARK Electronics ARKV6X](https://docs.px4.io/main/en/flight_controller/arkv6x.html)
* [CubePilot Cube Orange+](https://docs.px4.io/main/en/flight_controller/cubepilot_cube_orangeplus.html)
* [CubePilot Cube Orange](https://docs.px4.io/main/en/flight_controller/cubepilot_cube_orange.html)
* [CubePilot Cube Yellow](https://docs.px4.io/main/en/flight_controller/cubepilot_cube_yellow.html)
* [Holybro Durandal](https://docs.px4.io/main/en/flight_controller/durandal.html)
* [Airmind MindPX V2.8](http://www.mindpx.net/assets/accessories/UserGuide_MindPX.pdf)
* [Airmind MindRacer V1.2](http://mindpx.net/assets/accessories/mindracer_user_guide_v1.2.pdf)
* [Holybro Kakute F7](https://docs.px4.io/main/en/flight_controller/kakutef7.html)

### Community supported

These boards don't fully comply industry standards, and thus is solely maintained by the PX4 publc community members.

### Experimental

These boards are nor maintained by PX4 team nor Manufacturer, and is not guaranteed to be compatible with up to date PX4 releases.

* [Raspberry PI with Navio 2](https://docs.px4.io/main/en/flight_controller/raspberry_pi_navio2.html)
* [Bitcraze Crazyflie 2.0](https://docs.px4.io/main/en/complete_vehicles/crazyflie2.html)

## Project Roadmap

**Note: Outdated**

A high level project roadmap is available [here](https://github.com/orgs/PX4/projects/25).

## Project Governance

The PX4 Autopilot project including all of its trademarks is hosted under [Dronecode](https://www.dronecode.org/), part of the Linux Foundation.

<a href="https://www.dronecode.org/" style="padding:20px" ><img src="https://mavlink.io/assets/site/logo_dronecode.png" alt="Dronecode Logo" width="110px"/></a>
<a href="https://www.linuxfoundation.org/projects" style="padding:20px;"><img src="https://mavlink.io/assets/site/logo_linux_foundation.png" alt="Linux Foundation Logo" width="80px" /></a>
<div style="padding:10px">&nbsp;</div>
