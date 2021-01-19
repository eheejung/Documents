- [Altibase 7.1.0.4.9 Patch Notes](#altibase-71049-patch-notes)
  * [New Features](#new-features)
    + [BUG-47821 iloader에서SRID지원을 위해 Geometry 타입 처리를 WKB -> EWKB 포맷으로 변경해야 합니다.](#bug-47821-iloader--srid-------geometry--------wkb-----ewkb--------------)
    + [BUG-48288 타 제품과 동일한 이름의 힌트를 Altibase 만 인식할 수 있는 방법을 제공합니다.](#bug-48288-------------------altibase---------------------)
    + [BUG-48323 aexport 에서 SRID를 지원해야 합니다](#bug-48323-aexport----srid----------)
    + [BUG-48348 partial CSE 기능 추가](#bug-48348-partial-cse------)
    + [BUG-48357 iloader out 인자에 -WKB 옵션을 추가합니다.](#bug-48357-iloader-out------wkb----------)
  * [Fixed Bugs](#fixed-bugs)
    + [BUG-47870 동일한 디스크 테이블을 가진 이중화 객체 두 개 중 한 개를 삭제하면 이중화가 전송되지 않습니다.](#bug-47870------------------------------------------------------)
    + [BUG-48090 __OPTIMIZER_VIEW_TARGET_ENABLE = 1 에서 WITH 절과 서브쿼리가 사용된 쿼리 수행 시 결과 오류가 발생할 수 있습니다.](#bug-48090---optimizer-view-target-enable---1----with---------------------------------------)
    + [BUG-48179 삼중화 이상의 Altibase 이중화 환경에서 이중화 대상 테이블에 TRUNCATE 수행 시 ERR-11041 : A deadlock situation has been detected. 에러가 발생할 수 있습니다.](#bug-48179---------altibase----------------------truncate------err-11041---a-deadlock-situation-has-been-detected---------------)
    + [BUG-48300 CONNECT BY 절이 포함된 SQL 수행 시 ERR-3111D : There are too many DML statements in the stored procedure, or the SQL query is too long. 에러가 발생할 수 있습니다.](#bug-48300-connect-by--------sql------err-3111d---there-are-too-many-dml-statements-in-the-stored-procedure--or-the-sql-query-is-too-long---------------)
    + [BUG-48333 세션 타임아웃 조건에서도 세션이 종료되지 않아, 무한대기(HANG) 현상이 발생합니다.](#bug-48333---------------------------------hang-----------)
    + [BUG-48337 UNION ALL 에서 서로 다른 데이터 타입의 컬럼이 사용된 경우 Altibase 서버가 비정상 종료합니다.](#bug-48337-union-all-----------------------------altibase--------------)
    + [BUG-48355 AltibaseDatabaseMeta가 생성될 때마다 v$reserved_words를 조회합니다.](#bug-48355-altibasedatabasemeta----------v-reserved-words-------)
    + [BUG-48360 Altibase.jdbc.driver.cm.CmChannel.sendPacket() 에서 예외가 발생한 경우 통신 버퍼 상태 이상으로 프로토콜이 유실될 수 있습니다.](#bug-48360-altibasejdbcdrivercmcmchannelsendpacket-----------------------------------------------)
    + [BUG-48370 DB Link 쿼리를 수행하는 세션에서 TRCLOG_DETAIL_PREDICATE = 1 설정 후 원격 테이블 조회 시 Altibase 서버가 비정상 종료할 수 있습니다.](#bug-48370-db-link---------------trclog-detail-predicate---1------------------altibase-------------------)
    + [BUG-48390 INDEX_BUILD_THREAD_COUNT 프로퍼티의 기본값이 1로 설정되는 문제가 있습니다.](#bug-48390-index--build--thread--count------------1---------------)
    + [BUG-48393 리눅스에서 ST_Transfrom 함수를 사용할 수 없는 문제가 있습니다.](#bug-48393-------st--transfrom----------------------)
  * [Changes](#changes)
    + [Version Info](#version-info)
    + [호환성](#---)
      - [Database binary version](#database-binary-version)
      - [Meta Version](#meta-version)
      - [CM protocol Version](#cm-protocol-version)
      - [Replication protocol Version](#replication-protocol-version)
      - [Sharding Version](#sharding-version)
    + [프로퍼티](#----)
    + [성능 뷰](#----)



Altibase 7.1.0.4.9 Patch Notes
==============================

New Features
------------

### BUG-47821 iloader에서SRID지원을 위해 Geometry 타입 처리를 WKB -> EWKB 포맷으로 변경해야 합니다.

-   **module** : ux-iloader

-   **Category** : Functionality

-   **재현 빈도** : Always

-   **증상** : iloader에서SRID지원을 위해 Geometry 타입 처리를 WKB ->
    EWKB 포맷으로 변경해야 합니다.

- **재현 방법**

  - **재현 절차**

    ```sql
    drop table t1;
    create table t1(i1 geometry(1000) srid 4326);
    insert into t1 values(geometry'POINT(1 1)');
    insert into t1 values(geometry'SRID=4326;POINT(1 1)');
    insert into t1 values(geometry'LINESTRING(1 1, 2 2, 1 1)');
    insert into t1 values(geometry'SRID=4326;LINESTRING(1 1, 2 2, 1 1)');
    set vertical on;
    select * from geometry_columns where f_table_name = 'T1' order by 1, 2, 3;
    select srid(i1), asEWKT(i1,100) from t1;
    aexport -s 127.0.0.1 -u sys -p manager
    sh run_il_out.sh
    clean
    isql -s 127.0.0.1 -u sys -p manager -f ALL_CRT_TBL.sql
    sh run_il_in.sh
    set vertical on;
    select * from geometry_columns where f_table_name = 'T1' order by 1, 2, 3;
    select srid(i1), asEWKT(i1,100) from t1;
    ```

  - **수행 결과**

    ```sql
    iSQL> select * from geometry_columns where f_table_name = 'T1' order by 1, 2, 3;
    F_TABLE_SCHEMA    : SYS
    F_TABLE_NAME      : T1
    F_GEOMETRY_COLUMN : I1
    COORD_DIMENSION   : 2
    SRID              : 0
    1 row selected.
    iSQL> select srid(i1), asEWKT(i1,100) from t1;
    SRID(I1)       : 0
    ASEWKT(I1,100) : SRID=0;POINT(1 1)
    SRID(I1)       : 0
    ASEWKT(I1,100) : SRID=0;POINT(1 1)
    SRID(I1)       : 0
    ASEWKT(I1,100) : SRID=0;LINESTRING(1 1, 2 2, 1 1)
    SRID(I1)       : 0
    ASEWKT(I1,100) : SRID=0;LINESTRING(1 1, 2 2, 1 1)
    4 rows selected.
    ```

  -   **예상 결과**

      ```sql
      iSQL> select * from geometry_columns where f_table_name = 'T1' order by 1, 2, 3;
      GEOMETRY_COLUMNS.F_TABLE_SCHEMA    : SYS
      GEOMETRY_COLUMNS.F_TABLE_NAME      : T1
      GEOMETRY_COLUMNS.F_GEOMETRY_COLUMN : I1
      GEOMETRY_COLUMNS.COORD_DIMENSION   : 2
      GEOMETRY_COLUMNS.SRID              : 4326
      1 row selected.
      iSQL> select srid(i1), asEWKT(i1,100) from t1;
      SRID(I1)       : 0
      ASEWKT(I1,100) : SRID=0;POINT(1 1)
      SRID(I1)       : 4326
      ASEWKT(I1,100) : SRID=4326;POINT(1 1)
      SRID(I1)       : 0
      ASEWKT(I1,100) : SRID=0;LINESTRING(1 1, 2 2, 1 1)
      SRID(I1)       : 4326
      ASEWKT(I1,100) : SRID=4326;LINESTRING(1 1, 2 2, 1 1)
      4 rows selected.
      ```

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48288 타 제품과 동일한 이름의 힌트를 Altibase 만 인식할 수 있는 방법을 제공합니다.

- **module** : qp

- **Category** : Enhancement

- **재현 빈도** : Always

- **증상** :  타 제품과 동일한 이름의 힌트를 Altibase 만 인식할 수 있는 방법을 제공합니다. 

  - 모든 힌트에 ALTI_ 접두사를 사용합니다.  

  -  두 개 이상의 키워드로 구성된 힌트의 경우 공란을 언더바(under bar)로 변경 후 ALTI_ 접두사를 추가합니다.   

    예) NO INDEX -> ALTI_NO_INDEX

- **재현 방법**

-   **재현 절차**
    
-   **수행 결과**
    
- **예상 결과**

- **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48323 aexport 에서 SRID를 지원해야 합니다

-   **module** : ux-aexport

-   **Category** : Functionality

-   **재현 빈도** : Always

-   **증상** : aexport가 생성하는 CREATE TABLE DDL에 SRID를 지원합니다.

- **재현 방법**

  - **재현 절차**

    ```sql
    CREATE TABLE BUG_48323 (i1 INT PRIMARY KEY, i2 GEOMETRY(1000) SRID 4326);
    SET VERTICAL ON;
    SELECT F_GEOMETRY_COLUMN, SRID
    FROM GEOMETRY_COLUMNS
    WHERE F_TABLE_SCHEMA = 'SYS'
    AND F_TABLE_NAME = 'BUG_48323';
    F_GEOMETRY_COLUMN : I2
    SRID              : 4326
    1 row selected.
    $ aexport -s 127.0.0.1 -u sys -p manager -object SYS.BUG_48323
    
    DROP TABLE BUG_48323;
    
    $ is -f SYS_BUG_48323_CRT.sql
    ```

  - **수행 결과**

    ```sql
    SET VERTICAL ON;
    SELECT F_GEOMETRY_COLUMN, SRID
    FROM GEOMETRY_COLUMNS
    WHERE F_TABLE_SCHEMA = 'SYS'
    AND F_TABLE_NAME = 'BUG_48323';
    F_GEOMETRY_COLUMN : I2
    SRID              : 0
    1 row selected.
    ```

  -   **예상 결과**

          SET VERTICAL ON;
          SELECT F_GEOMETRY_COLUMN, SRID
          FROM GEOMETRY_COLUMNS
          WHERE F_TABLE_SCHEMA = 'SYS'
          AND F_TABLE_NAME = 'BUG_48323';
          F_GEOMETRY_COLUMN : I2
          SRID              : 4326
          1 row selected.

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48348 partial CSE 기능 추가

-   **module** : qp-dml-pvo

-   **Category** : Enhancement

-   **재현 빈도** : Always

-   **증상** : partial CSE 기능 추가

- **재현 방법**

  - **재현 절차**

    ```sql
    create table t1 ( i1 int, i2 int);
    alter session set trclog_detail_predicate = 1;
    select  * from t1
    WHERE (i1 = 1 AND i2 = 1) OR
          (i1 = 1 AND i2 = 2) OR
          (i1 = 1 AND i2 = 2);
    ```

  - **수행 결과**

    ```sql
    ALTER SESSION SET __OPTIMIZER_ELIMINATE_COMMON_SUBEXPRESSION = 2;
    [ERR-51214 : The ulnSetConnAttributeForDbc of client-side sharding failed due to the following reason: Attribute is invalid.]
    ```

  -   **예상 결과**

      ```sql
      ALTER SESSION SET __OPTIMIZER_ELIMINATE_COMMON_SUBEXPRESSION = 2;
      Alter success.
      select  * from t1
      WHERE (i1 = 1 AND i2 = 1) OR
            (i1 = 1 AND i2 = 2) OR
            (i1 = 1 AND i2 = 2);
      T1.I1       T1.I2
      ---------------------------
      No rows selected.
      ------------------------------------------------------------
      PROJECT ( COLUMN_COUNT: 2, TUPLE_SIZE: 8, COST: BLOCKED )
       SCAN ( TABLE: SYS.T1, FULL SCAN, ACCESS: 0, COST: BLOCKED )
        [ FILTER ]
        AND
         OR
          I2 = 1
          I2 = 2
         OR
          I1 = 1
          I2 = 2
         OR
          I2 = 1
          I1 = 1
         OR
          I1 = 1
          I1 = 1
      ------------------------------------------------------------
      ```

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48357 iloader out 인자에 -WKB 옵션을 추가합니다.

-   **module** : ux-iloader

-   **Category** : Enhancement

-   **재현 빈도** : Always

-   **증상** : iloader out 인자에 -WKB 옵션을 추가합니다. 이 옵션으로
    Altibase 7.1.0.4.0 이상에서 제공하는 EWKB(Extended Well-Known
    Binary) 형식의 공간 데이터를 WKB(Well-Known Binary) 형식으로
    다운로드할 수 있습니다.

-   **재현 방법**
-   **재현 절차**
    
-   **수행 결과**
    
-   **예상 결과**
    
-   **Workaround**

-   **변경사항**

    -   Performance view

    -   Property

    -   Compile Option

    -   Error Code

Fixed Bugs
----------

### BUG-47870 동일한 디스크 테이블을 가진 이중화 객체 두 개 중 한 개를 삭제하면 이중화가 전송되지 않습니다.

-   **module** : rp

-   **Category** : Testcase Error

-   **재현 빈도** : Always

-   **증상** : 동일한 디스크 파티션 테이블을 가진 이중화 객체 두 개 중
    한 개를 삭제(DROP REPLICATION)한 경우, 이중화 정보 업데이트 오류로
    이중화가 전송되지 않는 문제를 수정했습니다.

-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

    -   **예상 결과**

-   **Workaround**

-   **변경사항**

    -   Performance view

    -   Property

    -   Compile Option

    -   Error Code

### BUG-48090 __OPTIMIZER_VIEW_TARGET_ENABLE = 1 에서 WITH 절과 서브쿼리가 사용된 쿼리 수행 시 결과 오류가 발생할 수 있습니다.

-   **module** : qp-select-pvo

-   **Category** : Functional Error

-   **재현 빈도** : Always

-   **증상** : __OPTIMIZER_VIEW_TARGET_ENABLE = 1 에서 WITH 절과
    서브쿼리가 사용된 쿼리 수행 시 결과 오류가 발생하는 현상을 수정하였습니다.
    
- **재현 방법**

  - **재현 절차**

    ```sql
    DROP TABLE t1;
    DROP TABLE t2;
    CREATE TABLE t1 ( c1 VARCHAR(20) );
    CREATE TABLE t2 ( c1 VARCHAR(20), c2 VARCHAR(20) );
    INSERT INTO t1 VALUES ( 'AT100' );
    INSERT INTO t1 VALUES ( 'OT100' );
    INSERT INTO t1 VALUES ( 'BT100' );
    INSERT INTO t1 VALUES ( 'OT100' );
    INSERT INTO t1 VALUES ( 'CT100' );
    INSERT INTO t2 VALUES ( 'AT100', 'Unexpected result 1' );
    INSERT INTO t2 VALUES ( 'OT100', 'Expected result 1' );
    INSERT INTO t2 VALUES ( 'BT100', 'Unexpected result 2' );
    INSERT INTO t2 VALUES ( 'OT100', 'Expected result 2' );
    INSERT INTO t2 VALUES ( 'CT100', 'Unexpected result 3' );
    var v1 varchar(20);
    exec :v1 := 'N';
    1) 
    PREPARE
    WITH w1 ( c1, c2 )
    AS (
        SELECT ' ' AS c1, 'x' AS c2
        FROM DUAL
        UNION ALL
        SELECT ' ' AS c1, 'x' AS c2
        FROM DUAL
        INNER JOIN w1 ON c2 = w1.c1
        )
        , w2
    AS (
        SELECT ' ' AS c1, 'x' AS c2
        FROM DUAL
        INNER JOIN w1 ON c2 = w1.c1
        UNION
        SELECT c1, 'x' AS c2
        FROM t1
        WHERE c1 NOT IN (
                SELECT c2
                FROM w1
                )
        )
    SELECT /*+ no_plan_cache */ c2
    FROM t2
    WHERE t2.c1 IN (
            SELECT c1
            FROM w2
            )
        AND (
            t2.c1 = 'OT100'
            OR (
                :v1 = 'Y'
                AND t2.c1 IN (
                    SELECT c1
                    FROM w2
                    )
                )
            );
    2) 
    WITH w1
    AS (
        SELECT 'w1c1' w1c1
            ,'w1c2' w1c2
        FROM dual
        )
        ,w2
    AS (
        SELECT w1c1 w2c1
            ,(
                SELECT w1c2
                FROM dual
                ) w2c2
            ,'w2c3' w2c3
        FROM w1
        )
        ,w3
    AS (
        SELECT w2c2 w3c2
        FROM w2
        )
    SELECT (
            SELECT w3.w3c2
            FROM w3 LIMIT 1
            ) AS w3c2
    FROM DUAL;
    3)
    WITH w1
    AS (
        SELECT 'w1c1' w1c1
            ,'w1c2' w1c2
        FROM dual
        )
        ,w2
    AS (
        SELECT w1c1 w2c1
            ,(
                SELECT w1c2
                FROM dual
                ) w2c2
            ,'w2c3' w2c3
        FROM w1
        )
        ,w3
    AS (
        SELECT 'w1c1value' AS w3c1, w2c2 w3c2
        FROM w2
        )
    SELECT w3c2
    FROM w3 WHERE w3c1 NOT IN (SELECT w3.w3c2
            FROM w3);
    4)
    WITH w1
    AS (
        SELECT 'w1c1' w1c1
            ,'w1c2' w1c2
        FROM dual
        )
        ,w2
    AS (
        SELECT w1c1 w2c1
            ,(
                SELECT w1c2
                FROM dual
                ) w2c2
            ,'w2c3' w2c3
        FROM w1
        )
        ,w3
    AS (
        SELECT 'w1c1value' AS w3c1, w2c2 w3c2
        FROM w2
        )
    SELECT w3c2
    FROM w3 WHERE w3c1 IN (SELECT w3.w3c1
            FROM w3);
    ```

  - **수행 결과**

    ```sql
    1) No rows selected.
    2) 1 row selected.
    3) No rows selected.
    4) 1 row selected.
    ```

  -   **예상 결과**

      ```sql
      1) 
      Expected result 1
      Expected result 2
      2 rows selected.
      2)
      w1c2
      1 row selected.
      3)
      w1c2
      1 row selected.
      4)
      w1c2
      1 row selected.
      ```

- **Workaround**

  ```sql
  ALTER SYSTEM SET __OPTIMIZER_VIEW_TARGET_ENABLE = 0;
  ```

- **변경사항**

  -   Performance view

  -   Property

  -   Compile Option

  -   Error Code

### BUG-48179 삼중화 이상의 Altibase 이중화 환경에서 이중화 대상 테이블에 TRUNCATE 수행 시 ERR-11041 : A deadlock situation has been detected. 에러가 발생할 수 있습니다.

-   **module** : rp

-   **Category** : Functional Error

-   **재현 빈도** : Always

-   **증상** : 삼중화 이상의 Altibase 이중화 환경에서 DDL_LOCK_TIMEOUT 프로퍼티를 0 이 아닌 값으로 설정 후 이중화 대상 테이블에 TRUNCATE 수행할 경우 데드락(deadlock) 에러가 발생할 수 있습니다. DDL_LOCK_TIMEOUT 설정에 따라 이중화 동작에 데드락을 유발하는 문제를 수정하였습니다.
    
- **재현 방법**

  - **재현 절차**

    ```sql
    삼중화 환경
    ALTER SYSTEM SET REPLICATION_DDL_ENABLE=1;
    ALTER SYSTEM SET DDL_LOCK_TIMEOUT=60;
    TRUNCATE TABLE T1;
    ```

  - **수행 결과**

    ```sql
    ERR-11041 : A deadlock situation has been detected.
    ```

  -   **예상 결과**

      ```sql
      Alter success.
      ```

- **Workaround**

      아래 2가지 방법 중 한 가지를 선택 적용하면 deadlock 에러를 회피할 수 있습니다.
      1. 이중화를 중지하고 TRUNCATE를 수행한다.
      2. ALTER SYSTEM SET DDL_LOCK_TIMEOUT = 0 설정 후 TRUNCATE 를 수행한다.

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48300 CONNECT BY 절이 포함된 SQL 수행 시 ERR-3111D : There are too many DML statements in the stored procedure, or the SQL query is too long. 에러가 발생할 수 있습니다.

-   **module** : qp-dml-pvo

-   **Category** : Efficiency

-   **재현 빈도** : Always

-   **증상** : 옵티마이저에서 CONNECT BY 절 처리 시 불필요한 자원
    사용으로 ERR-3111D : There are too many DML statements in the stored
    procedure, or the SQL query is too long. 에러가 발생하는 현상을
    수정하였습니다.

-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

    -   **예상 결과**

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48333 세션 타임아웃 조건에서도 세션이 종료되지 않아, 무한대기(HANG) 현상이 발생합니다.

-   **module** : sm-disk-index

-   **Category** : Hang

-   **재현 빈도** : Rare

-   **증상** : 세션 타임아웃 조건에서도 세션이 종료되지 않는 문제가
    있어, timeout을 확인하는 코드를 추가합니다.

-   **재현 방법**
-   **재현 절차**
    
-   **수행 결과**
    
-   **예상 결과**
    
-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48337 UNION ALL 에서 서로 다른 데이터 타입의 컬럼이 사용된 경우 Altibase 서버가 비정상 종료합니다.

-   **module** : qp-dml-execute

-   **Category** : Fatal

-   **재현 빈도** : Always

-   **증상** : UNION ALL 에서 서로 다른 데이터 타입의 컬럼이 사용된 경우
    데이터 타입 변환 과정에서 Altibase 서버가 비정상 종료하는 문제가
    있어 수정합니다.

-   **재현 방법**

    -   **재현 절차**

        ```sql
        drop table IE_IINVOICE_MST_X10005;
        create table IE_IINVOICE_MST_X10005( 
        COMPANY_CD                               VARCHAR(28)                 NOT NULL,
        PARTNER_CD                               VARCHAR(80)                 NOT NULL,
        INVC_NO                               VARCHAR(80)                 NOT NULL,
        INVC_SQ                                VARCHAR(5)                  NOT NULL,
        SHIPNG_QT                               varchar(80)
        ) tablespace sys_tbs_disk_data;
        insert into IE_IINVOICE_MST_X10005( COMPANY_CD, PARTNER_CD, INVC_NO, INVC_SQ ) select level, level, level, level from dual connect by level < 10;
        drop table IE_IINVOICE_DTL_X10005;
        create table IE_IINVOICE_DTL_X10005( 
        COMPANY_CD                               VARCHAR(28)                 NOT NULL,
        PARTNER_CD                               VARCHAR(80)                 NOT NULL,
        INVC_NO                               VARCHAR(80)                 NOT NULL,
        INVC_SQ                                VARCHAR(5)                  NOT NULL,
        SHIPNG_QT                               varchar(80)
        ) tablespace sys_tbs_disk_data;
        insert into IE_IINVOICE_DTL_X10005( COMPANY_CD, PARTNER_CD, INVC_NO, INVC_SQ ) select level, level, level, level from dual connect by level < 10;
        
        SELECT /*+   */ T.COMPANY_CD , SUM(T.LETCRDT_ESTBL_QT) LETCRDT_ESTBL_QT
                   FROM ( 
                         SELECT IIVM.COMPANY_CD,
                                - IIVD.SHIPNG_QT LETCRDT_ESTBL_QT
                         FROM IE_IINVOICE_MST_X10005 IIVM
                              LEFT OUTER JOIN IE_IINVOICE_DTL_X10005 IIVD ON IIVD.COMPANY_CD = IIVM.COMPANY_CD
                         WHERE IIVM.COMPANY_CD = '1'
                            AND IIVM.INVC_NO    = '1'
                         UNION ALL
                         (
                            SELECT  '1' COMPANY_CD,
                                     -100 LETCRDT_ESTBL_QT
                            FROM DUAL
                            UNION ALL
                              SELECT '2' COMPANY_CD,
                                     -200 LETCRDT_ESTBL_QT
                              FROM DUAL
                          )
                          )  T
                    WHERE COALESCE(LETCRDT_ESTBL_QT, 0)  >  0
                    GROUP BY T.COMPANY_CD;
        ```
    
-   **수행 결과**
    
        Altibase 서버 비정상 종료
    
-   **예상 결과**
    
    ```sql
    No rows selected.
    ```
    
- **Workaround**

  ```sql
  SELECT /*+   */ T.COMPANY_CD , SUM(T.LETCRDT_ESTBL_QT) LETCRDT_ESTBL_QT                   FROM (     SELECT IIVM.COMPANY_CD,                                                                   - IIVD.SHIPNG_QT LETCRDT_ESTBL_QT                                                  FROM IE_IINVOICE_MST_X10005 IIVM                                           	             LEFT OUTER JOIN IE_IINVOICE_DTL_X10005 IIVD ON IIVD.COMPANY_CD = IIVM.COMPANY_CD                                      
                 WHERE IIVM.COMPANY_CD = '1' 
                  AND IIVM.INVC_NO    = '1'                                                            UNION ALL                                                                                 SELECT  '1' COMPANY_CD,                                                                -100 LETCRDT_ESTBL_QT                                                                 FROM DUAL
                    UNION ALL                                                                                SELECT '2' COMPANY_CD,                                                                -200 LETCRDT_ESTBL_QT                                                                  FROM DUAL                                    )  T                     WHERE  COALESCE(LETCRDT_ESTBL_QT, 0)  >  0                            
      GROUP BY T.COMPANY_CD;
  ```

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48355 AltibaseDatabaseMeta가 생성될 때마다 v$reserved_words를 조회합니다.

-   **module** : mm-jdbc

-   **Category** : Functionality

-   **재현 빈도** : Always

-   **증상** : AltibaseDatabaseMetadata가 생성될 때마다 v$reserved를 조회하는 경우, 불필요한 자원이 낭비되는 문제가 있어 수정합니다.
    
-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

            AltibaseDatabaseMetadata 객체 생성될 때마다 SELECT keyword FROM v$reserved_words 수행

    -   **예상 결과**

            SELECT keyword FROM v$reserved_words 를 매번 수행하지 않고 AltibaseDatabaseMetadata 최초 객체 생성시 한번만 실행.

-   **Workaround**

-   **변경사항**

    -   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48360 Altibase.jdbc.driver.cm.CmChannel.sendPacket() 에서 예외가 발생한 경우 통신 버퍼 상태 이상으로 프로토콜이 유실될 수 있습니다.

-   **module** : mm-jdbc

-   **Category** : Functional Error

-   **재현 빈도** : Always

-   **증상** : Altibase.jdbc.driver.cm.CmChannel.sendPacket() 에서
    예외가 발생한 경우 통신 버퍼 상태 이상으로 프로토콜이 유실되는
    문제를 수정했습니다. 이 현상이 발생한 경우 비정상적인 데이터 처리로
    Altibase 서버가 비정상 종료할 수 있습니다.

- **재현 방법**

  - **재현 절차**

    ```java
    stmt.executeQuery("SELECT keyword FROM v$reserved_words");stmt.executeQuery("SELECT keyword FROM v$reserved_words");stmt.executeQuery("SELECT keyword FROM v$reserved_words"); // 여기서 소켓 전송시 SocketException 발생stmt.executeQuery("SELECT keyword FROM v$reserved_words");
    ```

  -   **수행 결과**

          4번째 execute를 하면서 버퍼의 상태가 초기화 되지 않아 헤더사이즈(16byte)만큼 유실 발생

  -   **예상 결과**

          소켓 전송 시 예외가 나더라도 프로토콜 유실이 발생하지 말아야 함.

-   **Workaround**

- **변경사항**

  -   Performance view

  -   Property

  -   Compile Option

  -   Error Code

### BUG-48370 DB Link 쿼리를 수행하는 세션에서 TRCLOG_DETAIL_PREDICATE = 1 설정 후 원격 테이블 조회 시 Altibase 서버가 비정상 종료할 수 있습니다.

-   **module** : qp-dml-execute

-   **Category** : Fatal

-   **재현 빈도** : Always

-   **증상** : DB Link 쿼리를 수행하는 세션에서 TRCLOG_DETAIL_PREDICATE = 1 설정 후 원격 테이블 조회 시 Altibase 서버가 비정상 종료하는 문제를 수정합니다. 
    
-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

    -   **예상 결과**

-   **Workaround**

-   **변경사항**
-   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48390 INDEX_BUILD_THREAD_COUNT 프로퍼티의 기본값이 1로 설정되는 문제가 있습니다.

-   **module** : id

-   **Category** : Maintainability

-   **재현 빈도** : Unknown

-   **증상** : CPU코어갯수를 리턴하는 내부함수의 문제로 인해,
    7.1.0.4.5부터 INDEX_BUILD_THREAD_COUNT 프로퍼티의 기본값이 1로
    설정되는 문제가 있습니다. 이로 인해 인덱스 빌드시 쓰레드 갯수가 너무
    작게 설정되어 빌드성능이 나오지 않는 문제가 있었으며, 본패치는 해당문제를 해결합니다.
    
-   **재현 방법**

    -   **재현 절차**

    -   **수행 결과**

    -   **예상 결과**

-   **Workaround**

-   **변경사항**
-   Performance view
    -   Property
    -   Compile Option
    -   Error Code

### BUG-48393 리눅스에서 ST_Transfrom 함수를 사용할 수 없는 문제가 있습니다.

-   **module** : st-spatial

-   **Category** : Compile Error

-   **재현 빈도** : Always

-   **증상** : 7.1.0.4.5 부터 리눅스에서 ST_Transfrom 함수를 사용할 수 없는 문제를 수정하였습니다.
    
-   **재현 방법**
-   **재현 절차**
    
-   **수행 결과**
    
-   **예상 결과**
    
-   **Workaround**

-   **변경사항**
-   Performance view
    -   Property
    -   Compile Option
    -   Error Code

Changes
-------

### Version Info

| altibase version | database binary version | meta version | cm protocol version | replication protocol version | sharding version |
| ---------------- | ----------------------- | ------------ | ------------------- | ---------------------------- | ---------------- |
| 7.1.0.4.9        | 6.5.1                   | 8.9.1        | 7.1.7               | 7.4.6                        | 2.2.1            |

> Altibase 7.1 패치 버전별 히스토리는 [Version_Histories](https://github.com/ALTIBASE/Documents/blob/master/PatchNotes/Altibase_7_1_Version_Histories.md)에서 확인할 수 있다.

### 호환성

#### Database binary version

데이터베이스 바이너리 버전은 변경되지 않았다.

> 데이터베이스 바이너리 버전은 데이터베이스 이미지 파일과 로그파일의
> 호환성을 나타낸다. 이 버전이 다른 경우의 패치(업그레이드 포함)는
> 데이터베이스를 재구성해야 한다.

#### Meta Version

메타 버전은 변경되지 않았다.

> 패치를 롤백하려는 경우,
> [메타다운그레이드](https://github.com/ALTIBASE/Documents/blob/master/Manuals/Altibase_7.1/kor/Installation.md#%EB%A9%94%ED%83%80-%EB%8B%A4%EC%9A%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EB%93%9Cmeta-downgrade)를
> 참고한다.

#### CM protocol Version

통신 프로토콜 버전은 변경되지 않았다.

#### Replication protocol Version

Replication 프로토콜 버전은 변경되지 않았다.

#### Sharding Version

샤딩 버전은 변경 되지 않았다.

> 알티베이스 샤딩 프로토콜 및 메타는 상위, 하위 호환성을 보장하지
> 않는다. 즉, 샤딩 버전이 다른 경우, 재구성해야 한다.

### 프로퍼티

추가/변경/삭제된 프로퍼티 없음

### 성능 뷰

추가/변경/삭제된 성능뷰 없음