# Oracle 19.3.0 도커 이미지 빌드 및 컨테이너 실행 가이드
> `dockerfiles/<version>` 폴더에 LINUX.X64_193000_db_home.zip 같은 파일이 있어야 함
> * 다운로드 경로 : OS 와 버전에 맞는 파일 다운로드
>   * https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#db_ee

## 이미지 빌드

### none-cdb 전용 이미지
**feature/only-none-cdb 브랜치에서 실행**
```shell
$ sh buildContainerImage.sh -v 19.3.0 -t oracle/database:19.3.0.0-ee-none-cdb -e
```

### cdb/pdb 빌드 이미지
**feature/original-cdb 브랜치에서 실행**
```shell
$ sh buildContainerImage.sh -v 19.3.0 -t oracle/database:19.3.0.0-ee -e
```

## 컨테이너 실행

### none-cdb 버전 설치
```shell
$ docker run -d --name oracle19-ee-none-cdb \
 --ulimit nofile=1024:65536 --ulimit nproc=2047:16384 --ulimit stack=10485760:33554432 --ulimit memlock=3221225472 \
 -p 1521:1521 -p 5500:5500 \
 -e ORACLE_SID=ORCLCDB \
 -e ORACLE_PWD=Guseo12# \
 -e ENABLE_TCPS=true \
 -v ~/.oradata:/opt/oracle/oradata \
oracle/database:19.3.0.0-ee-none-cdb
```

### cdb/pdb 버전 설치
#### 직접 빌드 이미지로 설치
```shell
$ docker run -d --name oracle19-ee \
 --ulimit nofile=1024:65536 --ulimit nproc=2047:16384 --ulimit stack=10485760:33554432 --ulimit memlock=3221225472 \
 -p 1521:1521 -p 5500:5500 \
 -e CREATE_CDB=true \
 -e ORACLE_SID=ORCLCDB \
 -e ORACLE_PDB=ORCLPDB1 \
 -e ORACLE_PWD=Guseo12# \
 -e ENABLE_TCPS=true \
 -v ~/.oradata:/opt/oracle/oradata \
oracle/database:19.3.0.0-ee
```

#### 오라클 공식 이미지로 설치
```shell
$ docker run -d --name oracle19-ee \
 --ulimit nofile=1024:65536 --ulimit nproc=2047:16384 --ulimit stack=10485760:33554432 --ulimit memlock=3221225472 \
 -p 1521:1521 -p 5500:5500 \
 -e ORACLE_SID=ORCLCDB \
 -e ORACLE_PDB=ORCLPDB1 \
 -e ORACLE_PWD=Guseo12# \
 -e ENABLE_TCPS=true \
 -v ~/.oradata_cdb:/opt/oracle/oradata \
container-registry.oracle.com/database/enterprise:19.3.0.0
```