# Redis Topology
> Redis Architecture 의 종류
* Master-Slave 형태로 데이터를 복제해서 운영할 수 있다
* Master-Slave 간 복제는 Non-Blocking 상태로 이루어진다

<br>

## Redis Topology 와 Redis Cluster
* Cluster는 한 개의 Master에 여러 개의 Slave를 둘 수 있다는 것을 전제로 만들어진 것이다
* Topology는 Master-Slave 형태를 지원한다
* 따라서 Cluster는 Topology의 성질을 이용하여 구현된 것이다