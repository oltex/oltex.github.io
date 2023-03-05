---
title: "깃 치트 시트(Git Cheat Sheet) by GitHub Education"

categories:
  - git
tags:
  - tag
---

Git is the free and open source distributed version control system that's responsible for everything GitHub
related that happens locally on your computer. This cheat sheet features the most important and commonly
used Git commands for easy reference.

## INSTALLATION & GUIS
With platform specific installers for Git, GitHub also provides the
ease of staying up-to-date with the latest releases of the command
line tool while providing a graphical user interface for day-to-day
interaction, review, and repository synchronization.

## SET UP
모든 로컬 저장소에서 사용되는 사용자 정보 구성

|버전 기록을 검토할 때 크레딧으로 식별할 수 있는 이름 설정|
|---|
|git config --global user.name “[firstname lastname]”|

|각 기록 마커와 연결될 이메일 주소 설정|
|---|
|git config --global user.email “[valid-email]”|

|쉽게 검토할 수 있도록 Git에 대한 자동 명령줄 색상 설정|
|---|
|git config --global color.ui auto|

## SETUP & INIT
사용자 정보 구성, 리포지토리 초기화 및 복제

|기존 디렉토리를 Git 저장소로 초기화|
|---|
|git init|

|retrieve an entire repository from a hosted location via URL|
|---|
|git clone [url]|

## STAGE & SNAPSHOT
스냅샷 및 Git 준비 영역 작업

|다음 커밋을 위해 준비된 작업 디렉토리의 수정된 파일 표시|
|---|
|git status|

|다음 커밋(단계)에 지금 보이는 대로 파일을 추가|
|---|
|git add [file]|

|작업 디렉토리의 변경 사항을 유지하면서 파일을 언스테이징|
|---|
|git reset [file]|

|변경되었지만 스테이징되지 않은 차이점|
|---|
|git diff|

|diff of what is staged but not yet commited|
|---|
|git diff --staged|

|commit your staged content as a new commit snapshot|
|---|
|git commit -m “[descriptive message]”|

## BRANCH & MERGE
Isolating work in branches, changing context, and integrating changes

|list your branches. a * will appear next to the currently active branch|
|---|
|git branch|

|create a new branch at the current commit|
|---|
|git branch [branch-name]|

|switch to another branch and check it out into your working directory|
|---|
|git checkout|

|merge the specified branch’s history into the current one|
|---|
|git merge [branch]|

|show all commits in the current branch’s history|
|---|
|git log|

||
|---|
||

||
|---|
||

||
|---|
||

||
|---|
||

||
|---|
||
