# SQL-DCL
* DCL은 데이터의 보안, 무결성, 회복, 병행 제어 등을 정의하는 데 사용하는 언어이다.
* DCL은 데이터베이스 관리자(DBA)가 데이터 관리를 목적으로 사용한다.
* DCL의 종류
  * COMMIT :
    명령에 의해 수행된 결과를 실제 물리적 디스크로 저장하고, 데이터베이스 조작 작업이 정상적으로 완료되었음을 관리자에게 알려줌
  * ROLLBACK :
    데이터베이스 조작 작접이 비정상적으로 종료되었을 때 원래의 상태로 복구함
  * GRANT :
    데이터베이스 사용자에게 사용 권한을 부여함
  * REVOKE :
    데이터베이스 사용자의 사용 권한을 취소함

## GRANT / REVOKE
* 데이터베이스 관리자가 데이터베이스 사용자에게 권한을 부여하거나 취소하기 위한 명령어이다.
* GRANT : 권한 부여를 위한 명령어
* REVOKE : 권한 취소를 위한 명령어
* 사용자등급 지정 및 해제
 > GRANT 사용자등급 TO 사용자_ID_리스트 [IDENTIFIED BY 암호]; <BR>
 > REVOKE 사용자등급 FROM 사용자_ID_리스트;
* 테이블 및 속성에 대한 권한 부여 및 취소
 > GRANT 권한_리스트 ON 개체 TO 사용자 [WITH GRANT OPTION]; <BR>
 > INVOKE [GRANT OPTION FOR] 권한_리스트 ON 개체 FROM 사용자 [CASCADE]
 
* 권한 종류 : ALL, SELECT, INSERT, DELETE, UPDATE 등
* WITH GRANT OPTION : 부여받은 권한을 다른 사용자에게 다시 부여할 수 있는 권한을 부여함
* GRANT OPTION FOR : 다른 사용자에게 권한을 부여할 수 있는 권한을 취소함
* CASCADE : 권한 취소 시 권한을 부여받았던 사용자가 다른 사용자에게 부여한 권한도 연쇄적으로 취소함

## COMMIT
* COMMIT은 트랜잭션 처리가 정상적으로 완료된 후 트랜잭션이 수행한 내용을 데이터베이스에 반영하는 명령이다.
* COMMIT 명령을 실행하지 않아도 DML문이 성공적으로 완료되면 자동으로 COMMIT되고, DML이 실패하면 자동으로 ROLLBACK이 되도록 Auto Commit 기능을 설정할 수 있다.

## ROLLBACK
* ROLLBACK은 변경되었으나 아직 COMMIT되지 않은 모든 내용들을 취소하고 데이터베이스를 이전 상태로 되돌리는 명령어이다.
* 트랜잭션 전체가 성공적으로 끝나지 못하면 일부 변경된 내용만 데이터베이스에 반영되는 비일관성(Inconsistency) 상태가 될 수 있기 때문에 일부분만 완료된 트랜잭션은 롤백(ROLLBACK)되어야 한다.

## SAVEPOINT
* SAVEPOINT는 트랜잭션 내에 ROLLBACK 할 위치인 저장점을 지정하는 명령어이다.
* 저장점을 지정할 때는 이름을 부여한다.
* ROLLBACK 할 때 지정된 저장점까지의 트랜잭션 처리 내용이 모두 취소된다.
