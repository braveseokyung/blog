---
title : '유용한 명령어 및 툴 정리'
date : 2025-02-26T23:17:51+09:00
draft : False
authors : ["braveseokyung"]
description : 자주 쓰는 명령어와 툴 모음
toc:
    auto: false
---

<!--more-->

## 리눅스
### 용량 및 개수 확인
* 디스크 전체 용량 확인
    ```bash
    df -h
    ```

* 특정 폴더 용량 확인
    ```bash
    du -sh [폴더명]
    ```
* 특정 확장자의 파일 수 확인
    ```bash
    ls -l *.txt | wc -l
    ```
### 폴더 관리

* 폴더 이동
    ```bash
    mv [source] [target_folder]
    ```
* 폴더 삭제
    ```bash
    rm -r [폴더이름]
    ```
### 폴더 압축 관련
* zip 파일 압축
    ```bash
    zip -r [압축파일이름] [압축할폴더이름]
    ```
* 특정 폴더 제외하고 압축
    ```bash
    zip -r [압축파일이름] [압축할폴더이름] -x "project/node_modules/*"
    ```
* 특정 확장자 제외하고 압축
    ```bash
    zip -r [압축파일이름] [압축할폴더이름] -x "*.ipynb" "*.pth"
    ```
* zip 파일 압축 해제
    ```bash
    unzip [압축파일이름]
    ```
* tar 파일 압축

    ```bash
    tar -czvf [압축파일이름].tar.gz [압축할폴더이름]
    ```
* tar 파일 압축 해제
    ```bash
    tar -xvf [압축해제할파일]
    ```
### 프로세스 관리
* 모든 프로세스 출력
    ```bash
    ps aux 
    ```
* 프로세스 필터링
    ```bash
    ps aux | grep python
    ```
### 터미널 로그
* 모든 로그를 파일로 저장
    ```bash
    [파일실행] | tee output.txt
    ```

## gpu 관련
* gpu 확인
    ```bash
    nvidia-smi
    ```
* 실시간 확인
    ```bash
    gpustat -i
    ```
* 1초마다 출력해볼 수 있게
    ```bash
    watch -n 1 "nvidia-smi"
    ```

## tmux
* 새로운 세션 시작
    ```bash
    tmux new -s [세션이름]
    ```
* 세션에 접속
    ```bash
    tmux attach -t [세션이름]
    ```
* 특정 세션 종료
    ```bash
    tmux kill-session -t [세션이름]
    ```
* 세션 목록 보기
    ```bash
    tmux ls
    ```

+추가) tmux에서 스크롤하기
`Ctrl+b` 누르고 `[`

## Python
### Python Debugger
* 디버거 사용하기
    ```python
    import pdb
    pdb.set_trace()
    ```
* 디버거 지나서 실행하기 : `continue` 또는 `step` 입력

* 디버거 종료하기 : `quit()` 또는  `Ctrl+D`

### 파이썬 실행 시간 측정
```python
    import time

    start = time.time()
    # 실행할 코드
    end = time.time()

    print(f"Execution Time: {end - start:.4f} sec")
```
## Git
* main을 풀 받아야 하지만 지금 수정사항을 바로 커밋할 수는 없을 때
    ```bash
    git stash
    ```
    해서 main으로 옮겼다가 내 브랜치로 돌아와서
    ```bash
    git stash pop
    ```

