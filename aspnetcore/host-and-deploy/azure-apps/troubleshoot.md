---
title: "Azure 앱 서비스에서 ASP.NET Core 문제 해결"
author: guardrex
description: "ASP.NET Core Azure App Service 배포에 대한 문제 진단 방법을 알아봅니다."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 144af8e93bb935d07fd064d5f45b40faea4a2664
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a><span data-ttu-id="3f926-103">Azure 앱 서비스에서 ASP.NET Core 문제 해결</span><span class="sxs-lookup"><span data-stu-id="3f926-103">Troubleshoot ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="3f926-104">[Luke Latham](https://github.com/guardrex)으로</span><span class="sxs-lookup"><span data-stu-id="3f926-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3f926-105">이 문서에서는 설명 ASP.NET Core를 진단 하는 방법에 Azure 앱 서비스의 진단 도구를 사용 하 여 응용 프로그램 시작 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue using Azure App Service's diagnostic tools.</span></span> <span data-ttu-id="3f926-106">추가 문제 해결 권장 사항을 참조 하세요. [Azure 앱 서비스 진단 개요](/azure/app-service/app-service-diagnostics) 및 [하는 방법: Azure 앱 서비스 앱 모니터링](/azure/app-service/web-sites-monitor) Azure 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="3f926-106">For additional troubleshooting advice, see [Azure App Service diagnostics overview](/azure/app-service/app-service-diagnostics) and [How to: Monitor Apps in Azure App Service](/azure/app-service/web-sites-monitor) in the Azure documentation.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="3f926-107">응용 프로그램 시작 오류</span><span class="sxs-lookup"><span data-stu-id="3f926-107">App startup errors</span></span>

<span data-ttu-id="3f926-108">**502.5 프로세스 오류**</span><span class="sxs-lookup"><span data-stu-id="3f926-108">**502.5 Process Failure**</span></span>  
<span data-ttu-id="3f926-109">작업자 프로세스가 실패합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-109">The worker process fails.</span></span> <span data-ttu-id="3f926-110">응용 프로그램을 시작 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-110">The app doesn't start.</span></span>

<span data-ttu-id="3f926-111">[ASP.NET Core 모듈](xref:fundamentals/servers/aspnet-core-module) 하지만 작업자 프로세스를 시작 하는 데 시작 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-111">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="3f926-112">응용 프로그램 이벤트 로그를 자주 검사 이러한 유형의 문제를 해결 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-112">Examining the Application Event Log often helps troubleshoot this type of problem.</span></span> <span data-ttu-id="3f926-113">에 설명 되어 로그에 액세스 하는 [응용 프로그램 이벤트 로그](#application-event-log) 섹션.</span><span class="sxs-lookup"><span data-stu-id="3f926-113">Accessing the log is explained in the [Application Event Log](#application-event-log) section.</span></span>

<span data-ttu-id="3f926-114">*502.5 프로세스 오류* 잘못 구성 된 응용 프로그램 작업자 프로세스가 실패 하면 오류 페이지가 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-114">The *502.5 Process Failure* error page is returned when a misconfigured app causes the worker process to fail:</span></span>

![브라우저 창에 502.5 프로세스 오류 페이지를 표시 합니다.](troubleshoot/_static/process-failure-page.png)

<span data-ttu-id="3f926-116">**500 내부 서버 오류**</span><span class="sxs-lookup"><span data-stu-id="3f926-116">**500 Internal Server Error**</span></span>  
<span data-ttu-id="3f926-117">응용 프로그램 시작 하지만 오류는 서버에서 요청을 수행할 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-117">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="3f926-118">시작 하는 동안 또는 응답을 만드는 동안 응용 프로그램의 코드 내에서이 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-118">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="3f926-119">응답 없는 콘텐츠를 포함할 수 또는 응답으로 나타날 수 있습니다는 *500 내부 서버 오류* 브라우저에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-119">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="3f926-120">응용 프로그램 이벤트 로그는 일반적으로 응용 프로그램이 정상적으로 시작 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-120">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="3f926-121">서버의 관점에서는 사실입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-121">From the server's perspective, that's correct.</span></span> <span data-ttu-id="3f926-122">응용 프로그램 시작 않았습니다 하지만 유효한 응답을 생성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-122">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="3f926-123">[Kudu 콘솔의 실행](#run-the-app-in-the-kudu-console) 또는 [ASP.NET Core 모듈 stdout 로그 사용](#aspnet-core-module-stdout-log) 문제를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-123">[Run the app in the Kudu console](#run-the-app-in-the-kudu-console) or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="3f926-124">응용 프로그램 시작 오류 문제 해결</span><span class="sxs-lookup"><span data-stu-id="3f926-124">Troubleshoot app startup errors</span></span>

### <a name="application-event-log"></a><span data-ttu-id="3f926-125">응용 프로그램 이벤트 로그</span><span class="sxs-lookup"><span data-stu-id="3f926-125">Application Event Log</span></span>

<span data-ttu-id="3f926-126">응용 프로그램 이벤트 로그에 액세스 하려면 사용 된 **진단 및 문제 해결** 블레이드는 Azure 포털에서:</span><span class="sxs-lookup"><span data-stu-id="3f926-126">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal :</span></span>

1. <span data-ttu-id="3f926-127">Azure 포털에서에서 응용 프로그램의 블레이드를 여세요는 **응용 프로그램 서비스** 블레이드입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-127">In the Azure portal, open the app's blade in the **App Services** blade.</span></span>
1. <span data-ttu-id="3f926-128">선택 된 **진단 및 문제 해결** 블레이드입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-128">Select the **Diagnose and solve problems** blade.</span></span>
1. <span data-ttu-id="3f926-129">아래 **문제 범주를 선택**을 선택는 **웹 응용 프로그램 아래로** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-129">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="3f926-130">아래 **제안 솔루션**에 대 한 창을 열고 **응용 프로그램 이벤트 로그 열기**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-130">Under **Suggested Solutions**, open the pane for **Open Application Event Logs**.</span></span> <span data-ttu-id="3f926-131">선택 된 **응용 프로그램 이벤트 로그 열기** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-131">Select the **Open Application Event Logs** button.</span></span>
1. <span data-ttu-id="3f926-132">오류 검사 하 여 최신에서 제공 되는 *IIS AspNetCoreModule* 에 **소스** 열입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-132">Examine the latest error provided by the *IIS AspNetCoreModule* in the **Source** column.</span></span>

<span data-ttu-id="3f926-133">사용 하는 대신은 **진단 및 문제 해결** 직접 사용 하 여 응용 프로그램 이벤트 로그 파일을 검사 하려면 블레이드는 [Kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="3f926-133">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="3f926-134">선택 된 **고급 도구** 블레이드는 **개발 도구** 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-134">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3f926-135">선택 된 **이동&rarr;**  단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-135">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3f926-136">Kudu 콘솔이 새 브라우저 탭 또는 창으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-136">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3f926-137">페이지의 위쪽 탐색 모음을 사용 하 여 엽니다 **디버그 콘솔** 선택 **CMD**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-137">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3f926-138">열기는 **LogFiles** 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-138">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="3f926-139">연필 아이콘을 옆에 선택 된 *eventlog.xml* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-139">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="3f926-140">로그를 검토 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-140">Examine the log.</span></span> <span data-ttu-id="3f926-141">가장 최근의 이벤트를 보려면 로그의 아래쪽으로 스크롤하십시오.</span><span class="sxs-lookup"><span data-stu-id="3f926-141">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="3f926-142">Kudu 콘솔에서 앱 실행</span><span class="sxs-lookup"><span data-stu-id="3f926-142">Run the app in the Kudu console</span></span>

<span data-ttu-id="3f926-143">많은 시작 오류 응용 프로그램 이벤트 로그에서 유용한 정보를 생성 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-143">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="3f926-144">앱을 실행할 수는 [Kudu](https://github.com/projectkudu/kudu/wiki) 오류를 검색 하도록 원격 실행 콘솔:</span><span class="sxs-lookup"><span data-stu-id="3f926-144">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="3f926-145">선택 된 **고급 도구** 블레이드는 **개발 도구** 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-145">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3f926-146">선택 된 **이동&rarr;**  단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-146">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3f926-147">Kudu 콘솔이 새 브라우저 탭 또는 창으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-147">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3f926-148">페이지의 위쪽 탐색 모음을 사용 하 여 엽니다 **디버그 콘솔** 선택 **CMD**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-148">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3f926-149">폴더 경로를 열어 **사이트** > **wwwroot**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-149">Open the folders to the path **site** > **wwwroot**.</span></span>
1. <span data-ttu-id="3f926-150">콘솔에서 사용 하 여 응용 프로그램의 어셈블리를 실행 하 여 앱을 실행 *dotnet.exe*합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-150">In the console, run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="3f926-151">다음 명령에서에 대 한 응용 프로그램의 어셈블리의 이름을 바꾸어야 `<assembly_name>`:</span><span class="sxs-lookup"><span data-stu-id="3f926-151">In the following command, substitute the name of the app's assembly for `<assembly_name>`:</span></span>
   ```console
   dotnet .\<assembly_name>.dll
   ```
1. <span data-ttu-id="3f926-152">오류를 보여 주는 응용 프로그램에서 콘솔 출력 Kudu 콘솔에 파이프 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-152">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="3f926-153">ASP.NET Core 모듈 stdout 로그</span><span class="sxs-lookup"><span data-stu-id="3f926-153">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="3f926-154">ASP.NET Core 모듈 stdout 로그는 종종 응용 프로그램 이벤트 로그에서 찾을 수 없음 유용한 오류 메시지를 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-154">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="3f926-155">사용 하도록 설정 하 고 stdout 로그를 확인 하십시오.</span><span class="sxs-lookup"><span data-stu-id="3f926-155">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="3f926-156">로 이동는 **진단 및 문제 해결** 블레이드 Azure 포털의.</span><span class="sxs-lookup"><span data-stu-id="3f926-156">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="3f926-157">아래 **문제 범주를 선택**을 선택는 **웹 응용 프로그램 아래로** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-157">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="3f926-158">아래 **제안 솔루션** > **Stdout 로그 리디렉션을 사용 하도록 설정**, 단추를 선택 **Web.Config를 편집할 Kudu 콘솔을 열고**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-158">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="3f926-159">Kudu는에서 **진단 콘솔**, 폴더 경로를 열어 **사이트** > **wwwroot**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-159">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="3f926-160">표시 아래로 스크롤하여는 *web.config* 목록 맨 아래에 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-160">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="3f926-161">옆의 연필 아이콘을 클릭는 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-161">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="3f926-162">설정 **stdoutLogEnabled** 를 `true` 변경 하 고는 **가 stdoutLogFile** 경로를: `\\?\%home%\LogFiles\stdout`합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-162">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="3f926-163">선택 **저장** 업데이트 된 저장할 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-163">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="3f926-164">응용 프로그램에 요청을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-164">Make a request to the app.</span></span>
1. <span data-ttu-id="3f926-165">Azure 포털에 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-165">Return to the Azure portal.</span></span> <span data-ttu-id="3f926-166">선택 된 **고급 도구** 블레이드는 **개발 도구** 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-166">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3f926-167">선택 된 **이동&rarr;**  단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-167">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3f926-168">Kudu 콘솔이 새 브라우저 탭 또는 창으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-168">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3f926-169">페이지의 위쪽 탐색 모음을 사용 하 여 엽니다 **디버그 콘솔** 선택 **CMD**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-169">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3f926-170">선택 된 **LogFiles** 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-170">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="3f926-171">검사는 **Modified** 최신 수정 날짜를 사용 하 여 로그인의 stdout 편집 하려면 연필 아이콘을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-171">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="3f926-172">로그 파일을 열 때 오류가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-172">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="3f926-173">**기억해 야 합니다.**</span><span class="sxs-lookup"><span data-stu-id="3f926-173">**Important!**</span></span> <span data-ttu-id="3f926-174">Stdout 문제 해결이 완료 될 때의 로깅이 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="3f926-174">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="3f926-175">Kudu는에서 **진단 콘솔**, 경로에 return **사이트** > **wwwroot** 표시 하기 위해는 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-175">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="3f926-176">열기는 **web.config** 연필 아이콘을 선택 하 여 다시 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-176">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="3f926-177">설정 **stdoutLogEnabled** 를 `false`합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-177">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="3f926-178">선택 **저장** 파일을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-178">Select **Save** to save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="3f926-179">Stdout 로그를 사용 하지 않도록 설정 하지 않으면 응용 프로그램 또는 서버 오류가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-179">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3f926-180">로그 파일 크기에 제한이 없음을 또는 로그 파일을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-180">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="3f926-181">ASP.NET Core 응용 프로그램의 일상적인 로깅에 대 한 로그 파일 크기를 제한 하 고 로그를 회전 하는 로깅 라이브러리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-181">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3f926-182">자세한 내용은 참조 [제 3 자 로깅 공급자](xref:fundamentals/logging/index#third-party-logging-providers)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-182">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="3f926-183">일반적인 시작 오류</span><span class="sxs-lookup"><span data-stu-id="3f926-183">Common startup errors</span></span> 

<span data-ttu-id="3f926-184">참조는 [ASP.NET Core 오류 통칭](xref:host-and-deploy/azure-iis-errors-reference)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-184">See the [ASP.NET Core common errors reference](xref:host-and-deploy/azure-iis-errors-reference).</span></span> <span data-ttu-id="3f926-185">대부분의 응용 프로그램 시작을 방해 하는 일반적인 문제는 참조 항목에서 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-185">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="process-dump-for-a-slow-or-hanging-app"></a><span data-ttu-id="3f926-186">느린 되거나 응용 프로그램에 대 한 프로세스 덤프</span><span class="sxs-lookup"><span data-stu-id="3f926-186">Process dump for a slow or hanging app</span></span>

<span data-ttu-id="3f926-187">앱 느리게 응답 또는 요청에 응답 하지 때 참조 [Azure 앱 서비스의 느린 웹 앱 성능 문제 해결](/azure/app-service/app-service-web-troubleshoot-performance-degradation) 지침을 디버깅 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-187">When an app responds slowly or hangs on a request, see [Troubleshoot slow web app performance issues in Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) for debugging guidance.</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="3f926-188">원격 디버깅</span><span class="sxs-lookup"><span data-stu-id="3f926-188">Remote debugging</span></span>

<span data-ttu-id="3f926-189">참조 [원격 디버깅 문제 해결 Visual Studio를 사용 하 여 Azure 앱 서비스의 웹 앱의 웹 앱 섹션](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) Azure 설명서에서.</span><span class="sxs-lookup"><span data-stu-id="3f926-189">See [Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) in the Azure documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="3f926-190">응용 프로그램 정보</span><span class="sxs-lookup"><span data-stu-id="3f926-190">Application Insights</span></span>

<span data-ttu-id="3f926-191">[Application Insights](https://azure.microsoft.com/services/application-insights/) 오류 로깅 및 보고 기능을 포함 하 여 Azure 응용 프로그램 서비스에 호스팅된 앱에서 원격 분석을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-191">[Application Insights](https://azure.microsoft.com/services/application-insights/) provides telemetry from apps hosted in the Azure App Service, including error logging and reporting features.</span></span> <span data-ttu-id="3f926-192">Application Insights에서 응용 프로그램의 로깅 기능을 사용할 수 있게 하는 경우 응용 프로그램에서 시작 된 이후에 발생 하는 오류만 보고할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-192">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="3f926-193">자세한 내용은 참조 [ASP.NET Core 용 Application Insights](/azure/application-insights/app-insights-asp-net-core)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-193">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="monitoring-blades"></a><span data-ttu-id="3f926-194">블레이드를 모니터링합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-194">Monitoring blades</span></span>

<span data-ttu-id="3f926-195">모니터링 블레이드 문제 해결 환경을 항목의 앞부분에서 설명 하는 방법에 대 한 대안을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-195">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="3f926-196">500 시리즈 오류를 진단 하 이러한 블레이드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-196">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="3f926-197">ASP.NET Core 확장이 설치 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-197">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="3f926-198">확장이 설치 되어 있지 않으면를 수동으로 설치할:</span><span class="sxs-lookup"><span data-stu-id="3f926-198">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="3f926-199">에 **개발 도구** 블레이드 섹션은 **확장** 블레이드입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-199">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="3f926-200">**ASP.NET Core 확장** 는 목록에 표시 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-200">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="3f926-201">확장이 설치 되어 있지 않으면, 선택는 **추가** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-201">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="3f926-202">선택 된 **ASP.NET Core 확장** 목록에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-202">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="3f926-203">선택 **확인** 약관을 수락 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-203">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="3f926-204">선택 **확인** 에 **확장 추가** 블레이드입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-204">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="3f926-205">정보 팝업 메시지는 확장은 성공적으로 설치를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-205">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="3f926-206">Stdout 로깅을 활성화 하지 않으면 다음이 단계를 따르십시오.</span><span class="sxs-lookup"><span data-stu-id="3f926-206">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="3f926-207">Azure 포털에서 선택 된 **고급 도구** 블레이드는 **개발 도구** 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-207">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="3f926-208">선택 된 **이동&rarr;**  단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-208">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="3f926-209">Kudu 콘솔이 새 브라우저 탭 또는 창으로 열립니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-209">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="3f926-210">페이지의 위쪽 탐색 모음을 사용 하 여 엽니다 **디버그 콘솔** 선택 **CMD**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-210">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="3f926-211">폴더 경로를 열어 **사이트** > **wwwroot** 고 표시 아래로 스크롤하여는 *web.config* 목록 맨 아래에 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-211">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="3f926-212">옆의 연필 아이콘을 클릭는 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-212">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="3f926-213">설정 **stdoutLogEnabled** 를 `true` 변경 하 고는 **가 stdoutLogFile** 경로를: `\\?\%home%\LogFiles\stdout`합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-213">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="3f926-214">선택 **저장** 업데이트 된 저장할 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-214">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="3f926-215">진단 로깅을 활성화 하려면 계속 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-215">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="3f926-216">Azure 포털에서 선택 된 **진단 로그** 블레이드입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-216">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="3f926-217">선택 된 **에** 에 대 한 전환 **응용 프로그램 로깅 (파일 시스템)** 및 **자세한 오류 메시지**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-217">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="3f926-218">선택 된 **저장** 블레이드 맨 위에 있는 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-218">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="3f926-219">실패 요청 이벤트 버퍼링 (FREB) 로깅 라고도 하는 실패 한 요청 추적을 포함 시키려면 선택 된 **에** 에 대 한 전환 **실패 한 요청 추적**합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-219">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span> 
1. <span data-ttu-id="3f926-220">선택 된 **로그 스트림** 블레이드에서 아래에 즉시 표시 됩니다는 **진단 로그** 블레이드는 포털에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-220">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="3f926-221">응용 프로그램에 요청을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-221">Make a request to the app.</span></span>
1. <span data-ttu-id="3f926-222">로그 스트림 데이터 내에서 오류 원인을 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-222">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="3f926-223">**기억해 야 합니다.**</span><span class="sxs-lookup"><span data-stu-id="3f926-223">**Important!**</span></span> <span data-ttu-id="3f926-224">Stdout 문제 해결이 완료 될 때의 로깅이 사용 하지 않도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-224">Be sure to disable stdout logging when troubleshooting is complete.</span></span> <span data-ttu-id="3f926-225">지침 참조는 [ASP.NET Core 모듈 stdout 로그](#aspnet-core-module-stdout-log) 섹션.</span><span class="sxs-lookup"><span data-stu-id="3f926-225">See the instructions in the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) section.</span></span>

<span data-ttu-id="3f926-226">실패 한 요청 추적 로그 (FREB 로그)를 보려면:</span><span class="sxs-lookup"><span data-stu-id="3f926-226">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="3f926-227">로 이동는 **진단 및 문제 해결** 블레이드 Azure 포털의.</span><span class="sxs-lookup"><span data-stu-id="3f926-227">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="3f926-228">선택 **실패 한 요청 추적 로그** 에서 **지원 도구** 세로 막대의 영역입니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-228">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="3f926-229">참조 [실패 한 요청 추적 항목 Azure 앱 서비스에서에서 웹 앱에 대 한 진단 로깅 사용 하도록 설정의 섹션](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) 및 [Azure에서 웹 앱에 대 한 응용 프로그램 성능 Faq: 실패 한 요청 추적을 끄려면 어떻게?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-229">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="3f926-230">자세한 내용은 참조 [Azure 앱 서비스의 웹 앱에 대 한 진단 로깅 사용](/azure/app-service/web-sites-enable-diagnostic-log)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-230">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="3f926-231">Stdout 로그를 사용 하지 않도록 설정 하지 않으면 응용 프로그램 또는 서버 오류가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-231">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3f926-232">로그 파일 크기에 제한이 없음을 또는 로그 파일을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-232">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="3f926-233">ASP.NET Core 응용 프로그램의 일상적인 로깅에 대 한 로그 파일 크기를 제한 하 고 로그를 회전 하는 로깅 라이브러리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-233">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3f926-234">자세한 내용은 참조 [제 3 자 로깅 공급자](xref:fundamentals/logging/index#third-party-logging-providers)합니다.</span><span class="sxs-lookup"><span data-stu-id="3f926-234">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f926-235">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="3f926-235">Additional resources</span></span>

* [<span data-ttu-id="3f926-236">ASP.NET Core의 오류 처리 소개</span><span class="sxs-lookup"><span data-stu-id="3f926-236">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)
* [<span data-ttu-id="3f926-237">ASP.NET Core를 사용하는 Azure App Service 및 IIS에 대한 일반적인 오류 참조</span><span class="sxs-lookup"><span data-stu-id="3f926-237">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="3f926-238">Visual Studio를 사용 하 여 Azure 앱 서비스의 웹 응용 프로그램 문제 해결</span><span class="sxs-lookup"><span data-stu-id="3f926-238">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="3f926-239">"502 잘못 된 게이트웨이" 및 "503 서비스를 사용할 수 없음" Azure 웹 앱에서 HTTP 오류 문제 해결</span><span class="sxs-lookup"><span data-stu-id="3f926-239">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="3f926-240">Azure 앱 서비스의 느린 웹 앱 성능 문제 해결</span><span class="sxs-lookup"><span data-stu-id="3f926-240">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="3f926-241">Azure에서 웹 앱에 대 한 응용 프로그램 성능 Faq</span><span class="sxs-lookup"><span data-stu-id="3f926-241">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="3f926-242">Azure 금요일: Azure 앱 서비스의 진단 하 고 문제 해결 경험 (12 분 분량의 비디오)</span><span class="sxs-lookup"><span data-stu-id="3f926-242">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)