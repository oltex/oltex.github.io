---
title: "깃 저장소 이름 규칙(Git Repository Naming Convention)"
categories:
  - git
tags:
  - tag
---
## 개요
GitHub는 스타일 가이드 표준을 유지 하지만 저장소 이름 자체에 대한 표준을 명시적으로 나열하지 않습니다. 

다른 기사, 가이드, 리포지토리 및 스택 오버플로를 조사한 결과 몇 가지 규칙이 있고  
일부 조직에서는 리포지토리 이름 규칙을 공식화하는 조치를 취했습니다.

한 가지 예는 GitHub 문서 명명 저장소에 대한 브리티시 컬럼비아 정책 프레임워크입니다. 

이를 종합하여 일관적이고, 색인이 간편한 Git 저장소 명명 규칙을 정합니다.  
프로젝트를 생성하거나 기존에 작성된 프로젝트를 참고하고자 할 떄,  
일관되지 않은 규칙으로 작성된 저장소 이름으로 인해 어려움을 겪는 것을 방지하고자 함에 목적이 있습니다.

## 규칙
- Git 저장소 이름에는 전부 소문자를 사용합니다.
- Git 저장소 이름에 사용되는 Keyword 간의 구분은 '하이픈(-)'을 사용합니다.
- Git 저장소 이름에 사용되는 Keyword 순서는 Project Name-Purpose를 사용합니다.
- GitHub는 저장소 아래 사용 language를 표기해 주기 때문에 적지 않았습니다.
- 웹 사이트의 경우 특별히 명명한 프로젝트 명이 없을 경우 도메인(Domaion) 자체가 프로젝트 명이 될 수 있습니다. 
  - http://domain.com ➔ domain.com
  - http://sub.domain.com ➔ sub.domain.com

## 참고 자료
<details>
<summary>내용</summary>
<div markdown="1">
### GitHub 문서 명명 저장소에 대한 브리티시 컬럼비아 정책 프레임워크
GitHub repo의 이름을 지정하는 것은 매우 간단해 보입니다. 다음과 같은 것을 원할 것입니다.
- 설명
- 가독성
- 일관성
- 문맥
- 미래 친화적
- 확장 가능
- 재사용 가능
- 간략한(짧은/간단한)

리포지토리에 이름을 지정하는 잘못된 방법은 없지만 일부 이름은 다른 이름보다 낫습니다.  
다음은 GitHub 리포지토리의 멋진 이름을 선택하기 위한 몇 가지 고려 사항입니다.

#### 규약 준수
특정 프로젝트에 대해 설정된 명명 규칙에 따라 코드 언어 또는 커뮤니티를 시작하는 것이 좋습니다.
그러나 종종 Git 프로젝트는 많은 언어가 사용되는 웹사이트를 위한 것입니다.
단순함을 위해 도메인 이후에 웹사이트 저장소를 모델링하는 것이 합리적입니다.
```
http://domain.com ➔ domain.com.git
http://sub.domain.com ➔ sub.domain.com.git
```
다른 프로젝트의 경우 소문자 및 대시 패턴을 유지하겠습니다.
```
star-wars.git
the-empire-strikes-back.git
return-of-the-jedi.git
```

#### CamelCase는 어떻습니까?
CamelCase의 경우 문제는 단어에 대한 해석이 다른 경우가 많다는 것입니다(예: checkinService 대 checkInService).  
또한, 이름이 비슷한 repo가 많을 경우, 자신이 관심 있는 repo를 만든 사람이 대소문자를 구분하여 사용했는지 지속적으로 확인해야 하는 경우 자동 완성 기능이 어렵습니다.  
또한 대문자를 피하는 것이 가장 좋습니다. 아무도 소리치는 것을 좋아하지 않습니다.

#### 조직 이름 피하기
일단 설정되면 리포지토리 이름을 변경하는 것은 간단한 작업이지만 다운스트림 링크가 끊어지는 것과 같은 의도하지 않은 결과가 발생할 수 있습니다.  
따라서 장기간에 걸쳐 안정적일 수 있는 이름에 대해 생각하십시오.  
부처, 부서, 기관, 부서, 지부 및 팀 이름은 변경될 수 있으므로 repo 이름의 일부로 포함하지 않는 것이 가장 좋습니다.  
이름에 "BC"를 넣는 것은 "브리티시 컬럼비아 주"의 컨텍스트를 제공하는 데 적합하지만 모든 리포지토리를 그런 식으로 시작하지 맙시다.  
정렬 및 검색을 어렵게 만듭니다.

### Modus Create 토론
#### 규칙
응답자에게 저장소 이름에서 선호하는 다음 구분 기호를 선택하도록 요청했습니다.
- Hyphens (-) e.g. my-repo
- Underscores/Snake Case (\_) e.g. my_repo
- None e.g. myrepo
- Camel Case e.g. myRepo
- Pascal Case e.g. MyRepo

다음은 이름 자체에 어떤 정보가 포함되어야 하는지에 대한 규칙이었습니다. 
즉, 구분 기호로 구분해야 하는 정보는 무엇입니까? 
연구를 기반으로 세 가지 옵션을 제시하고 응답자에게 1(낮은)에서 3(높은)까지 순위를 매기도록 요청했습니다.
```
[product/project name]-[purpose]-[framework/language] e.g. myproject-api-rails
[product/project name]-[purpose] e.g. myproject-rest-api
[language/framework]-[product/project] e.g. python-security-scripts
```
#### 결과
총 68명이 설문에 응답했습니다.

Separator type|Respondents who used this formet
---|---
Hyphens (-) e.g. my-repo|81.7%(49)
Underscores/Snake Case (\_) e.g. my_repo|5%(3)
None e.g. myrepo|0%(0)
Camel Case e.g. myRepo|6.7%(4)
Pascal Case e.g. MyRepo|5%(3)
Other|1.7%(1)

전체 결과는 하이픈이 단연 가장 많이 사용되는 구분 기호임을 보여줍니다.

다음으로 제안된 명명 규칙에 대한 결과를 검토했습니다.

Option|1|2|3
---|---|---|---
[product/project name]-[purpose]-[framework/language] e.g. myproject-api-rails|15|30|15|
[product/project name]-[purpose] e.g. myproject-rest-api|36|15|9|
[language/framework]-[product/project] e.g. python-security-scripts|16|27|17|

#### 종합
결과를 종합하면 하이픈이 81.7%가 하이픈을 선호하는 가장 인기 있는 구분 기호 규칙임을 알 수 있습니다.  

옵션 3은 옵션 1보다 약간 더 인기가 많았으므로 일관되게 사용하기만 하면 이 옵션 중 하나를 프로젝트 팀에서 수용할 수 있습니다.
요약하자면, 개인이 Python 도구 또는 JavaScript 도구를 찾고 있을 수 있으므로 언어 정의가 유용한 오픈 소스 프로젝트에 규칙 3이 더 적합할 수 있습니다.  
반면에 규칙 1은 여러 제품이 존재하고 마이크로서비스와 같은 하위 구성요소로 구성된 프로젝트 팀이나 부서에 더 적합할 수 있습니다.
</div>
</details>
