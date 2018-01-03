---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: "JavaScript 코드 (C#)를 사용 하 여 컨트롤을 동적으로 채우는 | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX 컨트롤 도구 키트에서 DynamicPopulate 제어 웹 서비스 (또는 페이지 메서드)를 호출 및 t 대상 컨트롤에 결과 값을 채우는 중..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3b23e16f2e4dd26f50eb3e07f35d078dd19a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="7dd01-103">JavaScript 코드 (C#)를 사용 하 여 컨트롤을 동적으로 채울</span><span class="sxs-lookup"><span data-stu-id="7dd01-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="7dd01-104">으로 [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7dd01-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7dd01-105">[코드를 다운로드](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) 또는 [PDF 다운로드](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7dd01-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="7dd01-106">ASP.NET AJAX 컨트롤 도구 키트에서 DynamicPopulate 컨트롤은 웹 서비스 (또는 페이지 메서드)를 호출 하 고 없이 페이지 새로 고침 페이지에서 대상 컨트롤에 결과 값을 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7dd01-107">사용자 지정 클라이언트 쪽 JavaScript 코드를 사용 하 여 모집단을 트리거할 수 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="7dd01-108">개요</span><span class="sxs-lookup"><span data-stu-id="7dd01-108">Overview</span></span>

<span data-ttu-id="7dd01-109">`DynamicPopulate` 제어는 ASP.NET AJAX 컨트롤 도구 키트에서 웹 서비스 (또는 페이지 메서드)를 호출 하 고 대상 컨트롤 페이지 새로 고침 없이 페이지에 결과 값을 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7dd01-110">사용자 지정 클라이언트 쪽 JavaScript 코드를 사용 하 여 모집단을 트리거할 수 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7dd01-111">단계</span><span class="sxs-lookup"><span data-stu-id="7dd01-111">Steps</span></span>

<span data-ttu-id="7dd01-112">ASP.NET 웹 서비스 호출 될 메서드를 구현 하는 우선, 해야는 `DynamicPopulateExtender` 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="7dd01-113">웹 서비스 메서드를 구현 `getDate()` 인수 이라는 형식 문자열 중 하나 필요로 하 `contextKey`, 이후는 `DynamicPopulate` 컨트롤 각 웹 서비스 호출에 컨텍스트 정보의 한 부분을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="7dd01-114">코드는 다음과 같습니다 (파일 `DynamicPopulate.cs.asmx`) 세 가지 형식 중 하나에 현재 날짜를 검색 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="7dd01-115">다음 단계에서는 새 ASP.NET 사이트를 만들고 ASP.NET AJAX ScriptManager 컨트롤으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="7dd01-116">레이블 컨트롤을 추가 합니다 (예를 들어 이름이 같은 HTML 컨트롤을 사용 하 여 또는 `<asp:Label />` 웹 컨트롤) 웹 서비스 호출의 결과 나중에 표시 됩니다입니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="7dd01-117">다음으로, 포함 된 `DynamicPopulateExtender` 제어 하 고 웹 서비스 정보, 대상 컨트롤 이지만 사용자 지정 JavaScript를 사용 하 여 나중에 수행 하는 모집단을 트리거하는 컨트롤의 이름이 아니라 제공!</span><span class="sxs-lookup"><span data-stu-id="7dd01-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="7dd01-118">JavaScript 부분입니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-118">Now to the JavaScript part.</span></span> <span data-ttu-id="7dd01-119">`$find()` ASP.NET AJAX 라이브러리에 정의 된 함수 등의 ASP.NET AJAX 컨트롤 도구 키트 서버 측 개체에 대 한 참조를 반환 `DynamicPopulateExtender`합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="7dd01-120">현재 파일에 `$find("dpe")` 것에 대 한 참조를 반환 `DynamicPopulateExtender` 페이지에서 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="7dd01-121">라는 메서드가 노출 `populate()` 동적 채우기 프로세스를 트리거합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="7dd01-122">`populate()` 메서드는 하나의 인수가 필요:에 대 한 인수 역할을 수행 하는 상황에 맞는 키는 `getDate()` 웹 메서드.</span><span class="sxs-lookup"><span data-stu-id="7dd01-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="7dd01-123">예를 들어 `$find("dpe").populate("format1")` 월-일-연도 형식의 현재 날짜를 사용 하 여 레이블의 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="7dd01-124">이 샘플을 좀 더 유연 하 게 하기 위해 사용자가 다양 한 날짜 형식을 이제 선택 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="7dd01-125">그 중 한 각 라디오 단추가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="7dd01-126">한 번 라디오 단추에서 사용자가 클릭 JavaScript 코드 동적으로 채우는 선택한 날짜 형식을 사용 하 여 레이블을 합니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="7dd01-127">다음은 이러한 라디오 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="7dd01-128">라디오 단추를 JavaScript 식의 컨텍스트 내에 유의 `this.value` 정확히 동일한 정보에는 현재 단추의 값을 참조 하는 `getDate()` 메서드와 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7dd01-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="7dd01-129">[![지정 된 형식으로 서버에서 날짜를 검색 하는 단추를 클릭](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7dd01-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="7dd01-130">지정 된 형식으로 서버에서 날짜를 검색 하는 단추를 클릭 ([전체 크기 이미지를 보려면 클릭](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7dd01-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7dd01-131">[이전](dynamically-populating-a-control-cs.md)
[다음](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7dd01-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>