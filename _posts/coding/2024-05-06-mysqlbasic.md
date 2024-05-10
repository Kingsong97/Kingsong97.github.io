---
layout: post
title: SQL) MySQL install
date: 2024-05-06 12:59 +0900
description: 
image: https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/6ef23fcf-9f44-4b77-bd22-684197eaee75
category: MySQL
tags: MySQL
published: true
sitemap: true
---


# MySQL 설치 및 초기 설정: Windows, Mac, Linux 플랫폼별 가이드
MySQL은 가장 인기 있는 오픈 소스 관계형 데이터베이스 관리 시스템 중 하나로, 웹 애플리케이션은 물론 다양한 데이터 저장 요구사항에 폭넓게 사용됩니다. MySQL을 사용하기 위해서는 우선적으로 해당 운영 체제에 맞는 설치 및 초기 설정 과정을 이해해야 합니다. 본 글에서는 Windows, macOS, 그리고 Linux 환경에서 MySQL을 설치하고 기본 설정하는 방법을 상세히 안내합니다.

## Windows에서의 MySQL 설치
Windows 환경에서 MySQL을 설치하기 위해선, MySQL 공식 웹사이트에서 Windows용 MySQL Installer를 다운로드 받습니다. 설치 프로그램은 일반적으로 자동으로 구성 요소를 다운로드하고 설치하는 'Full' 설치를 제안합니다. 설치 중에는 다양한 구성 요소를 선택할 수 있는데, 이 때 MySQL Server, MySQL Workbench, MySQL Shell 등 필요한 컴포넌트를 선택합니다.

설치가 완료되면, 서버 설정을 위한 초기 구성 마법사가 실행됩니다. 이 단계에서는 네트워크 설정, 서버의 용도(개발/프로덕션), 기본 인증 및 보안 설정 등을 구성할 수 있습니다. 마지막으로, root 사용자의 비밀번호를 설정하고 추가 사용자 계정을 생성할 수 있습니다.

## macOS에서의 MySQL 설치
macOS에서 MySQL을 설치하는 과정은 약간 다릅니다. 먼저 MySQL 공식 사이트에서 macOS용 DMG 파일을 다운로드 받습니다. 다운로드한 파일을 열어 MySQL 설치 패키지를 실행시키고, 설치 안내에 따라 진행합니다.

설치 후, 시스템 환경 설정에서 MySQL을 시작하거나 멈출 수 있는 MySQL 환경설정 패널을 추가할 수 있습니다. 터미널을 통해 mysql -u root -p 명령을 입력하여 MySQL 서버에 접속할 수 있으며, 처음 접속 시 설정한 root 비밀번호를 입력합니다.

## Linux에서의 MySQL 설치
Linux에서는 대부분의 배포판 패키지 관리자를 통해 MySQL을 설치할 수 있습니다. 예를 들어, Ubuntu의 경우 sudo apt-get update 후 sudo apt-get install mysql-server 명령을 사용합니다. 설치 과정 중에 MySQL 서버의 root 비밀번호를 설정하게 됩니다.

설치가 완료된 후, sudo mysql_secure_installation 스크립트를 실행하여 보안 관련 추가 설정을 할 수 있습니다. 이 스크립트는 익명 사용자 삭제, 원격 root 로그인 비활성화, 테스트 데이터베이스 제거 등을 포함한 기본적인 보안 조치를 적용할 것을 제안합니다.

## 초기 설정 및 테스트
모든 플랫폼에서 설치가 완료된 후, 간단한 명령을 통해 설치가 제대로 되었는지를 확인할 수 있습니다. 예를 들어, mysql -u root -p 명령을 통해 설치된 MySQL 서버에 접속을 시도해 볼 수 있습니다. 성공적으로 로그인이 되면, 설치가 정상적으로 완료된 것입니다.

MySQL의 설치와 기본적인 설정 방법을 이해하는 것은 데이터베이스를 사용하는 모든 개발자에게 필수적인 기술입니다. 각 운영 체제에 맞춰 적절히 설치하고 기본 설정을 마치면, MySQL을 사용하여 데이터 관리 및 애플리케이션 개발을 시작할 수 있습니다.



