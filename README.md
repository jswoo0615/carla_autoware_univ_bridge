# carla_autoware_univ_bridge
현재 ROS Galactic 버전으로 통일하여 진행을 하는중입니다.  
추후 Autoware.Universe ROS Humble 버전으로 진행할 예정입니다.  
## 1. 사용 환경
- OS : Ubuntu 20.04
- ROS2 Verseion : Galactic
- Carla Version : 0.9.13
- Autoware.Universe : Source (Galactic)

## 2. 설치 방법
### 2.1. Carla Simulator
1. [깃허브 페이지](https://github.com/carla-simulator/carla/blob/master/Docs/download.md)에서 Carla 0.9.13 버전을 다운받습니다.  
 
2. 다운 받은 후 종속성 패키지를 설치합니다.

### 2.2. Autoware.Universe
1. [Autoware.Universe 문서 페이지](https://autowarefoundation.github.io/autoware-documentation/galactic/installation/autoware/source-installation/)로 이동하여 설치합니다.
- 주의사항 : Autoware.Universe의 Galactic 버전은 더 이상 업데이트가 되지 않고 있습니다.

### 2.3. Carla-ROS-bridge
1. [Carla_ROS_Bridge 문서 페이지](https://carla.readthedocs.io/projects/ros-bridge/en/latest/ros_installation_ros2/)에서 ROS2 버전 Carla-ROS-Bridge 설치합니다.  
- 문서 페이지에서는 ROS2 Foxy 지원된다 명시되어 있는데, 확인 결과 Galactic 버전에서도 사용 가능합니다.  
2. 일부 변경 사항  
Autoware.Universe의 차량의 `/tf`는 `/base_link`입니다.  
Carla-ROS-Bridge의 차량 `/tf`는 `/ego_vehicle`입니다.  
`carla-ros-bridge-root/src/ros-bridge/autoware_bridge/launch/autoware_bridge_launch.xml` 파일을 열어 일부 수정해줍니다.  
원본은 아래와 같이 `ego_vehicle2base_link` 노드에 `/base_link /ego_vehicle` 순으로 맵핑이 되어 있습니다.
    ```xml
    <node pkg="tf2_ros" exec="static_transform_publisher" name="ego_vehicle2base_link" args=" 0 0 0 0 0 0 /base_link /ego_vehicle"/>   
    ```

    테스트 결과 `/ego_vehicle /base_link` 순으로 맵핑이 되어야 Autoware.Universe 상의 차량 `/tf`와 Carla Simulator 상의 차량 `/tf`가 일치함을 확인했습니다.  
3. `colcon build` 명령어로 빌드 진행 후 
