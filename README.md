# hadoop_note
hadoop - data set transforming & analyzing + cluster of computer 에서 동작

Hortonworks docker sandbox를 virtual box로 올린 뒤, localhost:8888로 접속하면 browser interface인 ambari 를 이용가능. Dashboard id, passwd는 maria_dev
Hive view – upload table 이동, CSV 설정 (Field Delimiter 에서 Tab 구분이면 9 TAB), import
Table name과 index name을 적은 후 upload table

SELECT movie_id , count(movie_id) as ratingCount
FROM ratings
GROUP BY movie_id
ORDER BY ratingCount
DESC

== movie id에 따른 rating 수를 정렬하고 

SELECT name 
FROM movie_names
WHERE move_id =50


//6 
Hadoop distributed file system
- data access
- larger file manage
- file을 block 단위로 나눔. (128mb)
- 여러 컴퓨터에 분산해서 나눔. = 물리적으로 가까움 == 접근이 더 빠름.
- name node가 block을 tracking
- data node가 서로 정보를 전달하며 data 관리
- data를 읽을 때 name node에게 요청하여 block 위치를 찾고 data node에 요청
- single node가 종료되어도 backup copy 가 존재하여 복구 

- data write시 name node게 요청하고 data node에게 전달하면 data node 들이 확인. 확인, 저장 후 acknowledgement 보냄.
- 다시 name node에게 log를 보내고 이를 name node가 기록.
//single name node가 없어졌을 경우를 위해…
1. NFS로 backup파일을 따로 저장.
2. secondary name node (edit log를 primary name node로부터 copy하고 merge)
3. HDFS federation
- HDFS은 large file을 다루기 위해 최적화되어 많은 small file 들을 다루기엔 한계점이 있다.
- namespace volume으로 subdirectory를 나누어 name node들에게 나눔. 
- data 전부를 잃지 안는다.

//HDFS high availability 
- hot standby name node (shared edit log를 저장하여 back up 가능)
- zookeeper ( 어떤 name node가 active중인지 track)
- 단 하나의 name node만 사용하게 extreme measure 도입
//Using HDFS
- Ambari (UI)
- Command – Line interface
- HRRP / HDFS Proxies (web)
- java interface
- NFS Gate way – remote file system on server
