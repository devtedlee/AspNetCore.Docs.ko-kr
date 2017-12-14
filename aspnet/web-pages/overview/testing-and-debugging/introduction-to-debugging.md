---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: "소개 디버깅 ASP.NET 웹 페이지 (Razor) 사이트 | Microsoft Docs"
author: tfitzmac
description: "디버깅 하는 것은 프로세스 오류 찾기 및 수정 하면 코드 페이지에서입니다. 이 장에서 몇 가지 도구와 기술을 디버그 하는 데 사용할 수 및 analyz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: 2bc1f096540d17095ef760eed67b458fcd4e1372
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>소개 디버깅 ASP.NET 웹 페이지 (Razor) 사이트
====================
으로 [Tom FitzMacken](https://github.com/tfitzmac)

> 이 문서는 ASP.NET 웹 페이지 (Razor) 웹 사이트의 페이지를 디버깅 하는 다양 한 방법을 설명 합니다. 디버깅 하는 것은 프로세스 오류 찾기 및 수정 하면 코드 페이지에서입니다.
> 
> **학습 내용:** 
> 
> - 분석 하 고 페이지를 디버깅 하는 데 도움이 되는 정보를 표시 하는 방법.
> - Visual Studio에서 도구 디버깅을 사용 하 합니다.
>   
> 
> 다음은 문서에 도입 된 ASP.NET 기능입니다.
> 
> - `ServerInfo` 도우미입니다.
> - `ObjectInfo`도우미입니다.
>   
> 
> ## <a name="software-versions"></a>소프트웨어 버전
> 
> 
> - ASP.NET 웹 페이지 (Razor) 3
> - Visual Studio 2013
>   
> 
> 이 자습서는 ASP.NET 웹 페이지 2 에서도 작동합니다. WebMatrix 3을 사용할 수 있지만 통합 된 디버거 지원 되지 않습니다.


오류 및 코드의 문제를 해결 하는 중요 한 부분은 되 게 처음에 하기 위해서입니다. 에 오류가 발생할 수 있는 코드의 섹션을 배치 하 여 할 수 있습니다 `try/catch` 블록입니다. 자세한 내용은 섹션을 참조에서 오류 처리에 [ASP.NET 웹 프로그래밍 구문을 사용 하 여 Razor 소개](https://go.microsoft.com/fwlink/?LinkId=202890)합니다.

`ServerInfo` 도우미는 페이지를 호스팅하는 웹 서버 환경에 대 한 정보의 개요를 제공 하는 진단 도구입니다. 또한, 브라우저 페이지를 요청할 때 전송 되는 HTTP 요청 정보를 표시 합니다. `ServerInfo` 현재 사용자 id는 요청한 브라우저의 종류를 표시 하는 도우미 등입니다. 이러한 종류의 정보는 일반적인 문제를 해결 하는 데 유용 합니다.

1. 라는 새 웹 페이지 생성 *ServerInfo.cshtml*합니다.
2. 페이지의 끝 바로 앞에 닫는 `</body>` 태그, 추가 `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    추가할 수는 `ServerInfo` 페이지의 코드입니다. 하지만 끝에 추가 됩니다 출력 별도로 유지 하면 다른 페이지 콘텐츠를 보다 쉽게 읽을 수 있게 해줍니다.

    > [!NOTE] 
    > 
    > **중요 한** 프로덕션 서버에 웹 페이지를 이동 하기 전에 웹 페이지에서 모든 진단 코드를 제거 해야 합니다. 이에 적용 됩니다는 `ServerInfo` 으로 도우미는 다른 진단 기술이이 문서에서 코드 페이지를 추가 합니다. 이러한 종류의 정보를 악의적인 의도로 사람들에 게 유용한 될 수 있으므로 서버 및 비슷한 세부 정보에 서버 이름, 사용자 이름, 경로 대 한 정보를 참조 하 여 웹 사이트 방문자를 되기를 원하지 않습니다.
3. 페이지를 저장 하 고 브라우저에서 실행 합니다.

    ![디버깅-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` 도우미 페이지에 4 개의 테이블의 정보를 표시 합니다.

    - 서버 구성입니다. 이 섹션에서는 호스팅 웹 서버 컴퓨터 이름, 버전을 실행 하는 ASP.NET, 도메인 이름 및 서버 시간을 포함 하는 방법에 대 한 정보를 제공 합니다.
    - ASP.NET 서버 변수입니다. 이 섹션의 많은 HTTP 프로토콜 세부 정보 (호출된 HTTP 변수)에 대 한 세부 정보를 제공 하 고 있는 값이 각 웹 페이지 요청에 포함 됩니다.
    - HTTP 런타임 정보입니다. 이 섹션에서는의 웹 페이지에서 실행 되는 Microsoft.NET Framework, 경로, 캐시, 등에 대 한 세부 정보 버전에 대해서는 자세히 설명 합니다. (에서 살펴본 것 처럼 [ASP.NET 웹 프로그래밍 구문을 사용 하 여 Razor 소개](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET 웹 페이지 Razor 구문을 기반으로 하는 광범위 한 소프트웨어는 Microsoft의 ASP.NET 웹 서버 기술, 기반을 사용 하 여 개발 라이브러리는.NET Framework를 호출 합니다.)
    - 환경 변수입니다. 이 섹션에서는 웹 서버의 모든 로컬 환경 변수 및 값의 목록을 제공 합니다.

    모든 서버 및 요청 정보에 대 한 전체 설명은이 문서의 범위를 벗어납니다. 하지만 함을 확인할 수 있습니다는 `ServerInfo` 도우미 많은 진단 정보를 반환 합니다. 값에 대 한 자세한 내용은 하는 `ServerInfo` 반환 참조 [인식 환경 변수](https://technet.microsoft.com/en-us/library/dd560744(WS.10).aspx) Microsoft TechNet 웹 사이트 및 [IIS 서버 변수](https://msdn.microsoft.com/en-us/library/ms524602(VS.90).aspx) MSDN 웹 사이트입니다.

## <a name="embedding-output-expressions-to-display-page-values"></a>포함 출력 식 페이지 값을 표시 하려면

다른 코드에서 관찰할 방법은 출력 식의 페이지에 포함 하려면입니다. 다음과 같이 추가 하 여 변수 값을 출력할 직접 수 아시다시피, `@myVariable` 또는 `@(subTotal * 12)` 페이지에 있습니다. 코드에서 디버깅을 위해 중요 한 지점에 해당 출력 식을 배치할 수 있습니다. 이 페이지에 실행 될 때 키 변수나 계산의 결과 값을 볼 수 있습니다. 완료 하면 식 제거 하거나 주석 처리를 하 수 있습니다, 디버깅 합니다. 이 프로시저는 페이지를 디버깅 하는 데 포함된 식을 사용 하는 일반적인 방법을 보여 줍니다.

1. 라는 새로운 WebMatrix 페이지 생성 *OutputExpression.cshtml*합니다.
2. 페이지를 콘텐츠를 다음으로 바꿉니다.

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    이 예제에서는 사용는 `switch` 문을의 값을 확인 하는 `weekday` 변수와 것이 어떤 요일에 따라 서로 다른 출력 메시지는 다음 표시 합니다. 예제에서는 `if` 블록 첫 번째 코드 블록 내에서 현재 평일 값을 1 일을 추가 하 여 요일을 차례로 임의로 변경 합니다. 이해를 돕기 위해 도입 하는 오류입니다.
3. 페이지를 저장 하 고 브라우저에서 실행 합니다.

    잘못 된 요일에 대 한 페이지에는 메시지가 표시 됩니다. 모든 요일 실제로 이면 하루 나중에 대 한 메시지를 표시 됩니다. (코드는 의도적으로 잘못 된 날짜 값을 설정) 이므로 메시지 해제는 이유를 알고이 경우에 있지만, 실제로 종종 어렵습니다 작업 코드에 잘못 된 것 이며 알아야 합니다. 상황의 주요 개체 및 변수 값에 같은 찾아야 할를 디버깅 하려면 `weekday`합니다.
4. 삽입 하 여 출력 식을 추가 하 여 `@weekday` 코드에서 주석으로 표시 된 두 위치에 표시 된 것 처럼 합니다. 이러한 출력 식 코드 실행에서 해당 지점에서 변수의 값이 표시 됩니다.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 저장 하 고 브라우저에서 페이지를 실행 합니다.

    페이지 실제 요일을 먼저 표시 한 다음 1 일, 및에서 결과 메시지 추가에서 발생 하는 업데이트 된 요일은 `switch` 문. 두 변수 식에서 출력 (`@weekday`)에 HTML을 추가 하지 않은 때문에 날짜 사이 공백이 없어야 `<p>` 태그를 출력; 식이란 테스트에 대 한 합니다.

    ![디버깅-2](introduction-to-debugging/_static/image2.jpg)

    이제 오류를 볼 수 있습니다. 처음 표시 하면는 `weekday` 올바른 날짜 표시는 코드에서 변수를 합니다. 표시 하면이 두 번째 시간 후의 `if` 코드에서 블록을 하루 하나으로 해제 되어 있습니다. 따라서 평일 변수의 첫 번째 및 두 번째 모양 간에 발생 하는 알 수 있습니다. 실제 버그를 인 경우 이러한 종류의 접근 방식에서 문제가 발생 하는 코드의 위치를 파악할 도움이 될 합니다.
6. 두 개의 출력 식 추가, 제거 및 해당 주의 일을 변경 하는 코드를 제거 하 여 페이지에서 코드를 수정 합니다. 다음 예제와 같이 코드의 나머지, 전체 블록:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. 브라우저에서 페이지를 실행 합니다. 이 시간 실제 요일에 대해 표시 하는 올바른 메시지가 표시 됩니다.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>ObjectInfo 도우미를 사용 하 여 개체 값을 표시 하려면

`ObjectInfo` 도우미 유형과 전달한 각 개체의 값을 표시 합니다. (했던 것 처럼 위 예제의 출력 식을 사용 하 여) 코드에서 변수 및 개체의 값을 보는 데 사용할 수 있습니다 및 데이터 형식 개체에 대 한 정보를 볼 수 있습니다.

1. 명명 된 파일을 열고 *OutputExpression.cshtml* 앞에서 만든 합니다.
2. 다음 코드 블록을 페이지의 모든 코드를 바꿉니다.

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 저장 하 고 브라우저에서 페이지를 실행 합니다.

    ![디버깅-4](introduction-to-debugging/_static/image3.jpg)

    이 예제는 `ObjectInfo` 도우미에는 두 개의 항목이 표시 됩니다.

    - 형식입니다. 첫 번째 형식은 `DayOfWeek`합니다. 형식이 두 번째 변수의 `String`합니다.
    - 값입니다. 이 경우 이미 페이지에서의 인사말 변수 값을 표시 하기 때문에 값이 다시 표시 변수를 전달 하는 경우 `ObjectInfo`합니다.

    더 복잡 한 개체에 대 한는 `ObjectInfo` 형식 및 값의 모든 개체의 속성을 표시할 수는 기본적으로, 자세한 내용은 &#8212;도우미를 표시할 수 있습니다.

## <a name="using-debugging-tools-in-visual-studio"></a>Visual Studio에서 디버깅 도구를 사용 하 여

보다 포괄적인 디버깅 환경을 사용 하 여 Visual Studio 2013 또는 무료 [Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)합니다. Visual Studio와 함께 줄을 검사 하려는 코드에 중단점을 설정할 수 있습니다.

![설정 된 중단점](introduction-to-debugging/_static/image1.png)

웹 사이트를 테스트할 때 코드 실행이 중단점에서 중단 합니다.

![중단점에 도달](introduction-to-debugging/_static/image2.png)

변수 및 단계별로 코드에서 줄의 현재 값을 검사할 수 있습니다.

![값 표시](introduction-to-debugging/_static/image3.png)

ASP.NET Razor 페이지를 디버깅 하려면 Visual Studio에서 통합 된 디버거를 사용 하는 방법에 대 한 정보를 참조 하십시오. [프로그래밍 ASP.NET 웹 페이지 (Razor)를 사용 하 여 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)합니다.

## <a name="additional-resources"></a>추가 리소스

- [Visual Studio를 사용 하 여 ASP.NET 웹 페이지 (Razor) 프로그래밍](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS 서버 변수](https://msdn.microsoft.com/en-us/library/ms524602(VS.90).aspx) (MSDN)
- [환경 변수를 인식](https://technet.microsoft.com/en-us/library/dd560744(WS.10).aspx) (TechNet)