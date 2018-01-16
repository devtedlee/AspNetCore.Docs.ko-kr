---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "ASP.NET Web API 2에서에서의 라우팅 특성 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: ad44ee525601f308498967159e964aa41a2ce00c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="326fd-102">ASP.NET Web API 2에서에서 특성의 라우팅</span><span class="sxs-lookup"><span data-stu-id="326fd-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="326fd-103">으로 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="326fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="326fd-104">*라우팅* 웹 API 작업에 대 한 URI를 일치 하는 방법이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="326fd-105">Web API 2는 새로운 형식을 지 원하는 라우팅 이라고 *특성 라우팅을*합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="326fd-106">이름에서 알 수 있듯이 특성을 사용 하 여 경로 정의 하 특성 라우팅을 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="326fd-107">특성 라우팅을 제어할 수 더 많은 Uri 통해 웹 API에에서 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="326fd-108">예를 들어 계층의 리소스에 설명 하는 Uri를 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="326fd-109">이전 스타일의 라우팅 규칙 기반 라우팅, 프로파일러가 완전히 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="326fd-110">실제로 같은 프로젝트의 두 가지 방법을 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="326fd-111">특성 라우팅을 위한 다양 한 옵션이이 항목와 특성 라우팅을 사용 하도록 설정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="326fd-112">특성 라우팅을 사용 하는 종단 간 자습서를 참조 하십시오. [Web API 2에 특성 라우팅을 사용 하 여 REST API 만들기](create-a-rest-api-with-attribute-routing.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="326fd-113">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="326fd-113">Prerequisites</span></span>

<span data-ttu-id="326fd-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional 또는 Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="326fd-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="326fd-115">또는, NuGet 패키지 관리자를 사용 하 여 필요한 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="326fd-116">**도구** 선택 Visual Studio에서 메뉴 **라이브러리 패키지 관리자**을 선택한 후 **패키지 관리자 콘솔**합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="326fd-117">패키지 관리자 콘솔 창에서 다음 명령을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="326fd-118">라우팅 특성 이유?</span><span class="sxs-lookup"><span data-stu-id="326fd-118">Why Attribute Routing?</span></span>

<span data-ttu-id="326fd-119">사용 되는 Web API의 첫 번째 릴리스에서 *규칙 기반* 라우팅입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="326fd-120">라우팅의 해당 형식에 하나를 정의 하거나 기본적으로 더 많은 경로 템플릿 문자열을 매개 변수화 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="326fd-121">프레임 워크는 요청을 받으면 경로 템플릿에 대 한 URI와 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="326fd-122">(규칙 기반 라우팅에 대 한 자세한 내용은 참조 [ASP.NET Web API에서 라우팅](routing-in-aspnet-web-api.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="326fd-123">규칙 기반 라우팅의 이점은 한 곳에 정의 된 템플릿 및 라우팅 규칙은 모든 컨트롤러 일관 되 게 적용 됩니다입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="326fd-124">안타깝게도, 라우팅 규칙 기반 하는 것 RESTful Api에 공통 되는 특정 URI 패턴을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="326fd-125">예를 들어 리소스 자식 리소스를 주로 포함: 고객이 주문을 갖고, 영화 작업자에는, 책 저자 등과 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="326fd-126">이러한 관계를 반영 하는 Uri를 만들 수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="326fd-127">이러한 유형의 URI 규칙 기반 라우팅을 사용 하 여 만드는 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="326fd-128">완료할 수 있지만 많은 컨트롤러 또는 리소스 종류를 설정한 경우에 결과 크기가 조정 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="326fd-129">특성 라우팅을 사용 하 여이 URI에 대 한 경로 정의 하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="326fd-130">컨트롤러 동작에는 특성을 추가 하기만 하면:</span><span class="sxs-lookup"><span data-stu-id="326fd-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="326fd-131">다음은 라우팅 사용 하면 쉽게 특성 다른 몇 가지 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="326fd-132">**API 버전 관리**</span><span class="sxs-lookup"><span data-stu-id="326fd-132">**API versioning**</span></span>

<span data-ttu-id="326fd-133">이 예제에서는 "/ api/v 1/제품"과 다른 제어 라우트된 "/ api/v2/제품" 것입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="326fd-134">**오버 로드 된 URI 세그먼트**</span><span class="sxs-lookup"><span data-stu-id="326fd-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="326fd-135">이 예제에서는 "1" 주문 번호 이지만 컬렉션에 "보류 중" 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="326fd-136">**여러 매개 변수 형식**</span><span class="sxs-lookup"><span data-stu-id="326fd-136">**Mulitple parameter types**</span></span>

<span data-ttu-id="326fd-137">이 예에서는 주문 번호를 "1"은 하지만 "16/06/2013" 날짜를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="326fd-138">특성 라우팅을 사용 하도록 설정</span><span class="sxs-lookup"><span data-stu-id="326fd-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="326fd-139">특성 라우팅을 사용 하려면 호출 **MapHttpAttributeRoutes** 구성 중입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="326fd-140">에 정의 되어이 확장 메서드는 **System.Web.Http.HttpConfigurationExtensions** 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="326fd-141">특성 라우팅을와 결합할 수 있습니다 [규칙 기반](routing-in-aspnet-web-api.md) 라우팅입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="326fd-142">호출 규칙 기반 경로 정의 하려면는 **MapHttpRoute** 메서드.</span><span class="sxs-lookup"><span data-stu-id="326fd-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="326fd-143">웹 API를 구성 하는 방법에 대 한 자세한 내용은 참조 [구성 ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="326fd-144">참고: Web API 1에서 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="326fd-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="326fd-145">Web API 2 하기 전에 웹 API 프로젝트 템플릿은 다음과 같은 코드 생성:</span><span class="sxs-lookup"><span data-stu-id="326fd-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="326fd-146">특성 라우팅을 사용 하는 경우이 코드는 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="326fd-147">특성 라우팅을 사용 하려면 기존 웹 API 프로젝트를 업그레이드 하는 경우에 다음이 구성 코드를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="326fd-148">자세한 내용은 참조 [ASP.NET 호스팅와 Web API 구성](../advanced/configuring-aspnet-web-api.md#webhost)합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="326fd-149">경로 특성 추가</span><span class="sxs-lookup"><span data-stu-id="326fd-149">Adding Route Attributes</span></span>

<span data-ttu-id="326fd-150">특성을 사용 하 여 정의 하는 경로 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="326fd-151">문자열 &quot;고객 / {customerId} orders&quot; 경로 대 한 URI 템플릿입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="326fd-152">Web API 요청 URI를 템플릿에 일치 시 키 려 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="326fd-153">이 예제에서는 "customers" 및 "orders"가 리터럴 세그먼트가 고 "{customerId}"는 가변 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="326fd-154">이 템플릿에 다음 Uri 일치:</span><span class="sxs-lookup"><span data-stu-id="326fd-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="326fd-155">사용 하 여 일치 프로세스를 제한할 수 있습니다 [제약 조건을](#constraints)이 항목의 뒷부분에 설명 된 대로 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="326fd-156">다음에 유의 &quot;{customerId}&quot; 경로 템플릿에 매개 변수 이름과 일치는 *customerId* 메서드의 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="326fd-157">Web API 컨트롤러 작업을 호출을 경로 매개 변수에 바인딩할 하려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="326fd-158">예를 들어, URI가 `http://example.com/customers/1/orders`, 웹 API 하려고 값 "1"에 바인딩하는 *customerId* 동작에 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="326fd-159">URI 템플릿에서 여러 매개 변수를 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="326fd-160">경로 특성이 없는 모든 컨트롤러 메서드 규칙 기반 라우팅을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="326fd-161">이런 방식으로 두 가지 유형의 라우팅 동일한 프로젝트에 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="326fd-162">HTTP 메서드</span><span class="sxs-lookup"><span data-stu-id="326fd-162">HTTP Methods</span></span>

<span data-ttu-id="326fd-163">또한 web API HTTP 메서드 (GET, POST 등)는 요청에 따라 작업을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="326fd-164">기본적으로 웹 API 컨트롤러 메서드 이름의 시작 되 면 대/소문자 구분 일치 항목을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="326fd-165">예를 들어 라는 컨트롤러 메서드 `PutCustomers` HTTP PUT 요청을 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="326fd-166">재정의할 수 있습니다이 규칙이 준수 mathod 데코레이팅하여 다음과 같은 특성:</span><span class="sxs-lookup"><span data-stu-id="326fd-166">You can override this convention by decorating the mathod with any the following attributes:</span></span>

- <span data-ttu-id="326fd-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="326fd-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="326fd-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="326fd-168">**[HttpGet]**</span></span>
- <span data-ttu-id="326fd-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="326fd-169">**[HttpHead]**</span></span>
- <span data-ttu-id="326fd-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="326fd-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="326fd-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="326fd-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="326fd-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="326fd-172">**[HttpPost]**</span></span>
- <span data-ttu-id="326fd-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="326fd-173">**[HttpPut]**</span></span>

<span data-ttu-id="326fd-174">다음 예제에서는 HTTP POST 요청에 CreateBook 메서드를 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="326fd-175">사용 하 여 다른 모든 HTTP 메서드를 사용할 수 없는 메서드를 포함 하는 **AcceptVerbs** 목록이 며 HTTP 메서드는 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="326fd-176">경로 접두사</span><span class="sxs-lookup"><span data-stu-id="326fd-176">Route Prefixes</span></span>

<span data-ttu-id="326fd-177">대개 동일한 접두사로 시작 하는 컨트롤러의 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="326fd-178">예:</span><span class="sxs-lookup"><span data-stu-id="326fd-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="326fd-179">사용 하 여 전체 컨트롤러에 대 한 공통 접두사를 설정할 수 있습니다는 **[RoutePrefix]** 특성:</span><span class="sxs-lookup"><span data-stu-id="326fd-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="326fd-180">메서드 특성에 대해 물결표 (~)를 사용 하 여 경로 접두사를 재정의 하려면:</span><span class="sxs-lookup"><span data-stu-id="326fd-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="326fd-181">경로 접두사 매개 변수를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="326fd-182">경로 제약 조건</span><span class="sxs-lookup"><span data-stu-id="326fd-182">Route Constraints</span></span>

<span data-ttu-id="326fd-183">경로 제약 조건이 경로 템플릿 매개 변수를 일치 시키는 방법을 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="326fd-184">일반 구문은 &quot;{매개 변수: 제약 조건}&quot;합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="326fd-185">예:</span><span class="sxs-lookup"><span data-stu-id="326fd-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="326fd-186">여기서는 첫 번째 경로 경우에 선택할는 &quot;id&quot; URI의 세그먼트는 정수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="326fd-187">그렇지 않으면 두 번째 경로 선택 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="326fd-188">다음 표에서 지원 되는 제약 조건을 나열 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="326fd-189">제약 조건</span><span class="sxs-lookup"><span data-stu-id="326fd-189">Constraint</span></span> | <span data-ttu-id="326fd-190">설명</span><span class="sxs-lookup"><span data-stu-id="326fd-190">Description</span></span> | <span data-ttu-id="326fd-191">예제</span><span class="sxs-lookup"><span data-stu-id="326fd-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="326fd-192">알파</span><span class="sxs-lookup"><span data-stu-id="326fd-192">alpha</span></span> | <span data-ttu-id="326fd-193">일치 하는 항목 대문자 또는 소문자로 라틴어 알파벳 (a ~ z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="326fd-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="326fd-194">{x: 알파}</span><span class="sxs-lookup"><span data-stu-id="326fd-194">{x:alpha}</span></span> |
| <span data-ttu-id="326fd-195">bool</span><span class="sxs-lookup"><span data-stu-id="326fd-195">bool</span></span> | <span data-ttu-id="326fd-196">부울 값을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-196">Matches a Boolean value.</span></span> | <span data-ttu-id="326fd-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="326fd-197">{x:bool}</span></span> |
| <span data-ttu-id="326fd-198">datetime</span><span class="sxs-lookup"><span data-stu-id="326fd-198">datetime</span></span> | <span data-ttu-id="326fd-199">일치는 **DateTime** 값입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="326fd-200">{x: datetime을 (를)</span><span class="sxs-lookup"><span data-stu-id="326fd-200">{x:datetime}</span></span> |
| <span data-ttu-id="326fd-201">decimal</span><span class="sxs-lookup"><span data-stu-id="326fd-201">decimal</span></span> | <span data-ttu-id="326fd-202">10 진수 값을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-202">Matches a decimal value.</span></span> | <span data-ttu-id="326fd-203">{x: 소수}</span><span class="sxs-lookup"><span data-stu-id="326fd-203">{x:decimal}</span></span> |
| <span data-ttu-id="326fd-204">double</span><span class="sxs-lookup"><span data-stu-id="326fd-204">double</span></span> | <span data-ttu-id="326fd-205">64 비트 부동 소수점 값을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="326fd-206">{x: double}</span><span class="sxs-lookup"><span data-stu-id="326fd-206">{x:double}</span></span> |
| <span data-ttu-id="326fd-207">float</span><span class="sxs-lookup"><span data-stu-id="326fd-207">float</span></span> | <span data-ttu-id="326fd-208">32 비트 부동 소수점 값을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="326fd-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="326fd-209">{x:float}</span></span> |
| <span data-ttu-id="326fd-210">guid</span><span class="sxs-lookup"><span data-stu-id="326fd-210">guid</span></span> | <span data-ttu-id="326fd-211">GUID 값과 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-211">Matches a GUID value.</span></span> | <span data-ttu-id="326fd-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="326fd-212">{x:guid}</span></span> |
| <span data-ttu-id="326fd-213">int</span><span class="sxs-lookup"><span data-stu-id="326fd-213">int</span></span> | <span data-ttu-id="326fd-214">32 비트 정수 값과 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="326fd-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="326fd-215">{x:int}</span></span> |
| <span data-ttu-id="326fd-216">길이</span><span class="sxs-lookup"><span data-stu-id="326fd-216">length</span></span> | <span data-ttu-id="326fd-217">지정 된 길이로 또는 길이의 지정된 된 범위 내 문자열과 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="326fd-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="326fd-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="326fd-219">long</span><span class="sxs-lookup"><span data-stu-id="326fd-219">long</span></span> | <span data-ttu-id="326fd-220">64 비트 정수 값과 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="326fd-221">{x: long}</span><span class="sxs-lookup"><span data-stu-id="326fd-221">{x:long}</span></span> |
| <span data-ttu-id="326fd-222">max</span><span class="sxs-lookup"><span data-stu-id="326fd-222">max</span></span> | <span data-ttu-id="326fd-223">최대 값을 가진 정수를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="326fd-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="326fd-224">{x:max(10)}</span></span> |
| <span data-ttu-id="326fd-225">maxlength</span><span class="sxs-lookup"><span data-stu-id="326fd-225">maxlength</span></span> | <span data-ttu-id="326fd-226">최대 길이가 문자열과 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="326fd-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="326fd-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="326fd-228">분</span><span class="sxs-lookup"><span data-stu-id="326fd-228">min</span></span> | <span data-ttu-id="326fd-229">최소 값이 포함 된 정수를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="326fd-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="326fd-230">{x:min(10)}</span></span> |
| <span data-ttu-id="326fd-231">minlength</span><span class="sxs-lookup"><span data-stu-id="326fd-231">minlength</span></span> | <span data-ttu-id="326fd-232">최소 길이가 문자열과 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="326fd-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="326fd-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="326fd-234">range</span><span class="sxs-lookup"><span data-stu-id="326fd-234">range</span></span> | <span data-ttu-id="326fd-235">정수 값의 범위 내에서 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="326fd-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="326fd-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="326fd-237">정규식</span><span class="sxs-lookup"><span data-stu-id="326fd-237">regex</span></span> | <span data-ttu-id="326fd-238">정규식과 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-238">Matches a regular expression.</span></span> | <span data-ttu-id="326fd-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="326fd-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="326fd-240">공지 여 제약 조건 중 일부는 같은 &quot;min&quot;, 괄호 안에 인수를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="326fd-241">콜론으로 구분 된 매개 변수에 여러 개의 제약 조건을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="326fd-242">사용자 지정 경로 제약 조건</span><span class="sxs-lookup"><span data-stu-id="326fd-242">Custom Route Constraints</span></span>

<span data-ttu-id="326fd-243">구현 하 여 사용자 지정 경로 제약 조건을 만들 수 있습니다는 **IHttpRouteConstraint** 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="326fd-244">예를 들어 다음 제약 조건을 0이 아닌 정수 값을 매개 변수를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="326fd-245">다음 코드에는 제약 조건을 등록 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="326fd-246">이제 사용자 경로에 제약 조건을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="326fd-247">전체를 바꿀 수도 있습니다 **DefaultInlineConstraintResolver** 클래스를 구현 하 여는 **IInlineConstraintResolver** 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="326fd-248">이렇게 바뀝니다 모든 기본 제공 제약 조건을 하지 않는 한 구현 **IInlineConstraintResolver** 구체적으로 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="326fd-249">선택적 URI 매개 변수 및 기본값</span><span class="sxs-lookup"><span data-stu-id="326fd-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="326fd-250">경로 매개 변수를 매개 변수를 추가 하 여 URI 매개 변수를 선택적 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="326fd-251">경로 매개 변수는 선택 사항, 메서드 매개 변수에 대 한 기본값을 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="326fd-252">이 예제에서는 `/api/books/locale/1033` 및 `/api/books/locale` 동일한 리소스를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="326fd-253">또는 다음과 같이 경로 템플릿 안에 기본값을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="326fd-254">앞의 예제와 거의 동일 하지만 기본값 적용 될 때 동작의 약간의 차이만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="326fd-255">정확한 값이 매개 변수를 가집니다 ("{lcid?}") 첫 번째 예제에서 1033의 기본값 메서드 매개 변수에 직접 할당 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="326fd-256">두 번째 예제에서 ("{lcid 1033 =}"), "1033" 기본값인 모델 바인딩 프로세스를 진행 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="326fd-257">기본 모델 바인더 숫자 값 1033 "1033"로 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="326fd-258">그러나 다른 작업을 수행할 수 있는 사용자 지정 모델 바인더에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="326fd-259">(대부분의 경우에서 파이프라인에서 사용자 지정 모델 바인더를 없는 경우 두 형식의 수는 동일 합니다.)</span><span class="sxs-lookup"><span data-stu-id="326fd-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="326fd-260">경로 이름</span><span class="sxs-lookup"><span data-stu-id="326fd-260">Route Names</span></span>

<span data-ttu-id="326fd-261">Web API에서 모든 경로는 이름이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-261">In Web API, every route has a name.</span></span> <span data-ttu-id="326fd-262">경로 이름은 HTTP 응답의 링크를 포함할 수 있도록 링크를 생성 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="326fd-263">경로 이름을 지정 하려면는 **이름** 특성에는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="326fd-264">다음 예제에서는 경로 이름을 설정 하는 방법 및 링크를 생성 하는 경우 경로 이름을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="326fd-265">경로 순서</span><span class="sxs-lookup"><span data-stu-id="326fd-265">Route Order</span></span>

<span data-ttu-id="326fd-266">프레임 워크 경로 사용 하 여 URI를 일치 시 키 려, 경로 특정 순서로 평가 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="326fd-267">순서를 지정 하려면 설정는 **RouteOrder** 경로 특성에는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="326fd-268">값이 낮을수록 먼저 계산 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-268">Lower values are evaluated first.</span></span> <span data-ttu-id="326fd-269">기본 순서 값은 0입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-269">The default order value is zero.</span></span>

<span data-ttu-id="326fd-270">총 순서를 결정 하는 방법을 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="326fd-271">비교는 **RouteOrder** 경로 특성의 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="326fd-272">경로 템플릿에 각 URI 세그먼트를 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="326fd-273">각 세그먼트에 대해 다음과 같이 정렬 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="326fd-274">리터럴 세그먼트입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-274">Literal segments.</span></span>
    2. <span data-ttu-id="326fd-275">제약 조건으로 경로 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="326fd-276">제약 조건 없이 경로 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="326fd-277">제약 조건으로 와일드 카드 매개 변수 세그먼트입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="326fd-278">제약 조건 없이 와일드 카드 매개 변수 세그먼트입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="326fd-279">동률의 경우 경로 대/소문자 비구분 서 수 문자열 비교로 정렬 됩니다 ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx))의 경로 템플릿입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="326fd-280">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-280">Here is an example.</span></span> <span data-ttu-id="326fd-281">다음 컨트롤러 정의 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="326fd-282">이러한 경로 다음과 같이 정렬 됩니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="326fd-283">주문/세부 정보</span><span class="sxs-lookup"><span data-stu-id="326fd-283">orders/details</span></span>
2. <span data-ttu-id="326fd-284">주문 / {id}</span><span class="sxs-lookup"><span data-stu-id="326fd-284">orders/{id}</span></span>
3. <span data-ttu-id="326fd-285">주문 / {customerName}</span><span class="sxs-lookup"><span data-stu-id="326fd-285">orders/{customerName}</span></span>
4. <span data-ttu-id="326fd-286">주문 / {\*날짜}</span><span class="sxs-lookup"><span data-stu-id="326fd-286">orders/{\*date}</span></span>
5. <span data-ttu-id="326fd-287">주문 / 보류 중</span><span class="sxs-lookup"><span data-stu-id="326fd-287">orders/pending</span></span>

<span data-ttu-id="326fd-288">"Details"는 리터럴 세그먼트 및 "{id}" 앞에 표시 되 있지만 "보류 중" 표시 마지막는 **RouteOrder** 속성은 1입니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="326fd-289">(이 예에서는 고객이 없는 "details" 라고 가정 "보류 중" 또는 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="326fd-290">일반적으로 모호한 경로 방지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="326fd-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="326fd-291">이 예제에 대 한 더 나은 경로 템플릿 `GetByCustomer` 는 "고객이 / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="326fd-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>