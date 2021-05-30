## 14. 메모리와 mmap

#### 1. 동적 메모리 생성하기
* heap 영역에 생성 - malloc 함수
> malloc/free 관련 동적 메모리 생성 함수는 C 언어 과목에서 다룹니다.
>
> 메모리 조작 함수, strcmp/strcpy/memset 등도 C언어 과목에서 다룹니다.

#### 2. 파일 처리 성능 개선 기법 - 메모리에 파일 매핑

```
#include <sys/mman.h>
void *mmap(void *start, size_t length, int prot, int flags, int fd, off_t offset
```
* [start + offset] ~ [start + offset + length] 만큼의 물리 메모리 공간을 mapping 할 것을 요청
* 보통 start : NULL 또는 0 사용, offset: mapping 되기 원하는 물리 메모리 주소로 지정
* prot: 보호모드 설정
    * PROT_READ(읽기 가능) / PROT_WRITE(쓰기 가능) / PROT_EXEC(실행 가능) / PROT_NONE(접근 불가)
* flags: 메모리 주소 공간 설정
    * MAP_SHARED(다른 프로세스와 공유 가능) / MAP_PRIVATE(프로세스 내에서만 사용 가능) / MAP_FIXED(지정된 주소로 공간 지정)
* fd: device file 에 대한 file descriptor

#### 3. mmap 동작 방식으로 이해하는 실제 메모리 동작
> 운영체제, 가상 메모리 이해를 기반으로 실제 활용 총정리
> 
> 컴퓨터 공학 이해 없이는 mmap 동작을 이해하기 어려움
> 

1. mmap 실행 시, 가상 메모리 주소에 file 주소 매핑(가상 메모리 이해)
2. 해당 메모리 접근 시 (요구 페이징, lazy allocation)
    * 페이지 폴트 인터럽트 발생
    * OS 에서 file data를 복사해서 물리 메모리 페이지에 넣어줌
3. 메모리 read 시 : 해당 물리 페이지 데이터 읽으면 됨
4. 메모리 write 시 : 해당 물리 페이지 데이터 수정 후 페이지 상태 flag 중 dirty bit 를 1로 수정
5. 파일 close 시 물리 페이지 데이터가 file 에 업데이트 됨 (성능 개선)

#### 4. 파일, 메모리 가상메모리
* 장점
    * read(), write() 시 반복적인 파일 접근을 방지하여 성능 개선
    * mapping 된 영역은 파일 처리를 위한 lseek() 을 사용하지 않고 간단한 포인터 조작으로 탐색 가능
* 단점
    * mmap 은 페이지 사이즈 단위로 매핑
        * 페이지 사이즈 단위의 정부새가 아닌 경우 한 페이지 정도의 공간 추가 할당 및 남은 공간을 0으로 채워주게 됨
    
#### 5. 파일 처리 성능 개선 기법 - 메모리에 파일 매핑
```
int munmap(void *addr, size_t length)
```
* *addr 에 mapping 된 물리 메모리 주소를 해제한다
* length: mapping 된 메모리 크기 (mmap 에서 지정했던 동일 값을 넣음)

```
int msync(void *start, size_t length, int flags);
```
* start: mmap()를 통해 리턴 받은 메모리 맵의 시작 주소
* length: 동기화를 할 길이 시작 주소로 부터 길이를 지정하면 된다
* flags
    * MS_ASYNC: 비동기 방식, 동기화(memory -> file) 하라는 명령만 내리고 결과에 관계 없이 다음 코드 실행(따라서, 동기화가 완료안된 상태로 다음 코드 실행 가능)
    * MS_SYNC: 동기 방식, 동기화(memory -> file)가 될때까지 블럭 상태로 대기
    * MS_INVALIDATE: 현재 메모리 맵을 무효화 하고 파일의 데이터로 갱신 즉 File -> Memory
    
#### 5. inode 파일 시스템
1. inode 메타 데이터 - stat 함수
```
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>

int stat (const char *path, struct stat * buf);
int fstat(int filedes. struct stat *buf);
```
> fopen/read/write 등 기본 파일 시스템콜은 C 언어 과목에서 다룸

2. inode 메타 데이터 - stat 구조체
```
struct stat {
    dev_t       st_dev;     /* ID of device containing file */
    ino_t       st_ino;     /* inode number */
    mode_t      st_mode;    /* 파일 종류 및 접근 권한 */
    nlink_t     st_nlink;   /* hardlink 된 횟수 */
    uid_t       st_uid;     /* 파일 소유자 */
    gid_t       st_gid;     /* group Id of owner */
    dev_t       st_rdev;    /* device ID (if special file) */
    off_t       st_size;    /* 파일 크기(bytes) */
    blksize_t   st_blksize; /* blocksize for file system I/O */
    tlkcnt_t    st_blocks;  /* number of 512B block allocated */
    time_t      st_atime;   /* time of last access */
    time_t      st_mtime;   /* time of last modification */
    time_t      st_ctime;   /* time of last status change */
}
```

3. Standard Stream(표준 입출력) 과 파일 시스템콜
* command 로 실행되는 프로세스는 세가지 스트림을 가지고 있음
    * 표준 입력 스트림 (Standard Input Stream)     - stdin
    * 표준 출력 스트림 (Standard Output Stream)    - stdout
    * 오류 출력 스트림 (Standard Error Stream)     - stderr
* 모든 스트림은 일반적인 plain text 로 console 에 출력하도록 되어 있음
