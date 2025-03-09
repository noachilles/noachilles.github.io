---
title: (Git Hub Blog) Failed Action error, Action 실패할 때
date: 2025-03-09 16:00:00 +0900
categories: [Git, Blog]
tags: [Github, Blog, 깃허브, 블로그]
---

## 깃허브 블로그 액션 실패 원인과 해결 방안

2025년 2월, 깃허브 블로그에 올린 글을 다시 보러 들어갔는데 Update가 이루어지지 않았다.
분명히 github push도 성공했는데 왜일까? 하고 Action 기록을 살펴보니 Workflow file을 실행하는 과정에서 오류가 발생했다.  
![Image1.Github Actions Error](https://github.com/user-attachments/assets/6c979b80-2fbb-431c-b583-ddbad0c96186)

아래와 같은 안내문을 함께 보여줘서 링크를 타고 들어가봤다.  
`` This request has been automatically failed because it uses a deprecated version of `actions/upload-artifact: v3`. Learn more: https://github.blog/changelog/2024-04-16-deprecation-notice-v3-of-the-artifact-actions/ ``

그랬더니 알 수 있었던 사실, version 업데이트로 인한 문제였다.

### Github Actions

이 블로그는 Ruby 언어 기반의 Jekyll 프레임 워크로 만들어져 있는 정적 웹사이트(백엔드를 필요로 하지 않고, 간단한 기능이 구현된 웹사이트)이다. 작성된 코드를 하나의 웹 프로젝트 형태로 build 한다.  
여기까지는 알고 있었던 사실이지만 build 이후 deploy 과정에서 Github Actions을 사용한다.

**Github Actions**는 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD(연속 통합 및 지속적인 업데이트) 플랫폼이라고 한다.

블로그 작성 이후 YAML 형식으로 만들어진 Workflow 파일을 Github Actions를 통해 배포하는 방식으로 블로그 배포가 이루어지는 것으로 보인다.

<span style="color:#f08080">위의 문제는 결국 블로그 포스트를 배포하기 위한 Workflow 파일에서 version 오류 때문에 발생한 것이다. </span>

### 해결 방안

![Image2. Workflows file root](https://github.com/user-attachments/assets/87786d9c-604c-4020-bb75-f0b6847fcad1)  
우선 블로그 전체 폴더 중 `.github/workflows/pages-deploy.yml` 경로로 들어간다.

이후 아래의 항목들을 찾아 version을 의미하는 @ 뒤의 숫자를 아래와 동일하게 변경한다.  
`actions/upload-pages-artifact@v3`  
`actions/deploy-pages@v4`

이후 yml 파일을 먼저 저장하고, post를 수정하거나 업로드하면 정상적으로 작동되는 것을 확인할 수 있다!
