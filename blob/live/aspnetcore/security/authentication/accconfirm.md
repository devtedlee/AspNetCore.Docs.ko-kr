---
title: "계정 확인 및 ASP.NET 코어에서 암호 복구"
author: rick-anderson
description: "전자 메일 확인 및 암호 재설정으로 ASP.NET Core 응용 프로그램을 구축 하는 방법을 보여 줍니다."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: b004a8e7680b203416552e5a7a2809799e657759
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="11923-103">계정 확인 및 ASP.NET 코어에서 암호 복구</span><span class="sxs-lookup"><span data-stu-id="11923-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="11923-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="11923-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="11923-105">이 자습서에서는 전자 메일 확인 및 암호 재설정으로 ASP.NET Core 응용 프로그램을 구축 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="11923-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="11923-106">새 ASP.NET Core 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="11923-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="11923-108">이 단계는 Windows에서 Visual Studio에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="11923-109">CLI 지침은 다음 섹션을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="11923-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="11923-110">이 자습서는 Visual Studio 2017 미리 보기 2 이상이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="11923-111">Visual Studio에서 새 웹 응용 프로그램 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="11923-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="11923-112">선택 **ASP.NET Core 2.0**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="11923-113">다음 그림 표시 **.NET Core** 선택 하면 선택할 수 있지만 **.NET Framework**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="11923-114">선택 **인증 변경** 로 설정 하 고 **개별 사용자 계정**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="11923-115">기본값을 그대로 두고 **저장 된 사용자 계정 앱에서**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-115">Keep the default **Store user accounts in-app**.</span></span>

![선택 하는 "개별 사용자 계정 radio"를 표시 하는 새 프로젝트 대화 상자](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="11923-118">이 자습서는 Visual Studio 2017 이상 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="11923-119">Visual Studio에서 새 웹 응용 프로그램 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="11923-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="11923-120">선택 **인증 변경** 로 설정 하 고 **개별 사용자 계정**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![선택 하는 "개별 사용자 계정 radio"를 표시 하는 새 프로젝트 대화 상자](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="11923-122">MacOS 및 Linux 용.NET core CLI 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="11923-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="11923-123">CLI 또는 SQLite를 사용 하는 경우 명령 창에서 다음을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="11923-124">`--auth Individual`개별 사용자 계정 템플릿을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="11923-125">Windows에서 추가 된 `-uld` 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="11923-126">`-uld` 옵션은 SQLite DB 보다는 LocalDB 연결 문자열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="11923-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="11923-127">실행 `new mvc --help` 도움말을 보려면이 명령에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="11923-128">새 사용자 등록을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-128">Test new user registration</span></span>

<span data-ttu-id="11923-129">응용 프로그램을 실행, 선택는 **등록** 링크를 선택한 사용자를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="11923-130">Entity Framework Core 마이그레이션을 실행 하는 지침을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="11923-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="11923-131">전자 메일에만 유효성 검사와는 시점에서 [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="11923-132">등록을 제출 하면 앱에 로그인 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="11923-133">자습서의 뒷부분에 나오는 변경 합니다이 있으므로 새 사용자가 전자 메일의 유효성을 검사 될 때까지 로그인 할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="11923-134">Id 데이터베이스 보기</span><span class="sxs-lookup"><span data-stu-id="11923-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="11923-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="11923-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="11923-136">**보기** 메뉴 선택 **SQL Server 개체 탐색기** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="11923-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="11923-137">로 이동 **(localdb) (SQL Server 13) MSSQLLocalDB**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="11923-138">마우스 오른쪽 단추로 클릭 **dbo입니다. AspNetUsers** > **데이터를 볼**:</span><span class="sxs-lookup"><span data-stu-id="11923-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 개체 탐색기의 AspNetUsers 테이블에 대 한 상황에 맞는 메뉴](accconfirm/_static/ssox.png)

<span data-ttu-id="11923-140">참고는 `EmailConfirmed` 필드는 `False`합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="11923-141">응용 프로그램에서 확인 전자 메일을 보낼 때 다음 단계에서이 전자 메일에 다시 사용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="11923-142">선택한 행을 마우스 오른쪽 단추로 클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="11923-143">전자 메일을 지금 별칭 삭제는 간편한 방법이 다음 단계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="11923-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="11923-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="11923-145">참조 [SQLite ASP.NET Core MVC 프로젝트에서 작업](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite DB를 확인 하는 방법에 대 한 지침은 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="11923-146">Ssl을 사용 하 여 SSL에 대 한 IIS Express를 설치합니다</span><span class="sxs-lookup"><span data-stu-id="11923-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="11923-147">참조 [SSL 적용](xref:security/enforcing-ssl)합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="11923-148">전자 메일 확인이 필요</span><span class="sxs-lookup"><span data-stu-id="11923-148">Require email confirmation</span></span>

<span data-ttu-id="11923-149">다른 사용자에 가장 하지 않는 것을 확인 하려면 새 사용자 등록의 전자 메일을 확인 하는 것이 좋습니다 (즉, 이러한 하지 않은 등록 된 다른 사용자의 전자 메일).</span><span class="sxs-lookup"><span data-stu-id="11923-149">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="11923-150">토론 포럼을 가지 며 하지 못하도록 하려는 경우를 가정해 볼 "yli@example.com로 등록" 에서"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="11923-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="11923-151">전자 메일 확인 하지 않고 "nolivetto@contoso.com" 응용 프로그램에서 원치 않는 전자 메일을 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="11923-152">사용자는 실수로으로 등록 된다고 가정 합니다 "ylo@example.com" 맞춤법 오류를 발견 하지 않은 "yli"의 수 앱이 올바른 전자 메일에 없는 때문에 암호 복구를 사용 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="11923-153">전자 메일 확인 bot에서만 제한 된 보호를 제공 하 고 여러 작업 전자 메일 별칭을 등록 하는 데 사용할 수 있는 결정된 된 스팸에서 보호를 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="11923-154">일반적으로 새 사용자는 확인 된 전자 메일을 갖기 전에 웹 사이트에 데이터를 게시 하는 것을 방지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="11923-155">업데이트 `ConfigureServices` 확인 된 전자 메일을 요구 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="11923-158">이전 줄에는 해당 전자 메일이 확인 될 때까지 로그인에서 등록 된 사용자 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="11923-159">그러나 해당 줄에서 등록 한 후에 기록 되 고 새 사용자를 방해 되지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-159">However, that line does not prevent new users from being logged in after they register.</span></span> <span data-ttu-id="11923-160">기본 코드는 등록 한 후 사용자를 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="11923-161">로그 한 후에 다시 등록할 때 까지는 로그인 할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="11923-162">따라서 새로 등록 된 사용자는 이번에 변경 하는 자습서의 뒷부분에 나오는 **하지** 에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="11923-163">전자 메일 공급자를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-163">Configure email provider</span></span>

<span data-ttu-id="11923-164">이 자습서에서는 SendGrid 전자 메일을 보내는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="11923-165">SendGrid 계정 및 전자 메일을 보내는 키 필요.</span><span class="sxs-lookup"><span data-stu-id="11923-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="11923-166">다른 전자 메일 공급자를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-166">You can use other email providers.</span></span> <span data-ttu-id="11923-167">ASP.NET Core 2.x 포함 `System.Net.Mail`, 응용 프로그램에서 전자 메일을 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="11923-168">전자 메일을 보내는 SendGrid 또는 다른 전자 메일 서비스를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-168">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="11923-169">[옵션 패턴](xref:fundamentals/configuration/options) 사용자 계정 및 키 설정에 액세스 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-169">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="11923-170">자세한 내용은 참조 [구성](xref:fundamentals/configuration/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-170">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="11923-171">전자 메일 보안 키를 인출 하는 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="11923-171">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="11923-172">이 샘플은 `AuthMessageSenderOptions` 클래스에 만들어집니다는 *Services/AuthMessageSenderOptions.cs* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-172">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="11923-173">설정의 `SendGridUser` 및 `SendGridKey` 와 [암호 관리자 도구](../app-secrets.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-173">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="11923-174">예:</span><span class="sxs-lookup"><span data-stu-id="11923-174">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="11923-175">암호 관리자 windows에서의 사용자 키/값 쌍을 저장 한 *secrets.json* %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId > 디렉터리에 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-175">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="11923-176">콘텐츠는 *secrets.json* 파일 암호화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-176">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="11923-177">*secrets.json* 파일은 다음과 같습니다 (의 `SendGridKey` 값 제거 되었습니다.)</span><span class="sxs-lookup"><span data-stu-id="11923-177">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="11923-178">AuthMessageSenderOptions를 사용 하는 시작 구성</span><span class="sxs-lookup"><span data-stu-id="11923-178">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="11923-179">추가 `AuthMessageSenderOptions` 끝날 때 서비스 컨테이너에는 `ConfigureServices` 에서 메서드는 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="11923-179">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="11923-182">AuthMessageSender 클래스 구성</span><span class="sxs-lookup"><span data-stu-id="11923-182">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="11923-183">이 자습서에서는 통해 전자 메일 알림을 추가 하는 방법을 보여 줍니다. [SendGrid](https://sendgrid.com/), 하지만 SMTP 및 다른 메커니즘을 사용 하 여 메일을 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-183">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="11923-184">설치는 `SendGrid` NuGet 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-184">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="11923-185">패키지 관리자 콘솔에서 다음을 입력 다음 명령을:</span><span class="sxs-lookup"><span data-stu-id="11923-185">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="11923-186">참조 [SendGrid를 무료로 시작](https://sendgrid.com/free/) 무료 SendGrid 계정을 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-186">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="11923-187">SendGrid를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-187">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="11923-189">에 코드를 추가 *Services/EmailSender.cs* SendGrid를 구성 하는 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-189">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="11923-191">에 코드를 추가 *Services/MessageServices.cs* SendGrid를 구성 하는 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-191">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="11923-192">계정 확인 및 암호 복구 사용</span><span class="sxs-lookup"><span data-stu-id="11923-192">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="11923-193">서식 파일에는 계정 확인 및 암호 복구를 위한 코드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-193">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="11923-194">찾을 `[HttpPost] Register` 에서 메서드는 *AccountController.cs* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-194">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-195">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-195">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="11923-196">새로 등록 된 사용자가 있는 다음 줄을 주석 처리에 자동으로 로그온에서 함:</span><span class="sxs-lookup"><span data-stu-id="11923-196">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="11923-197">전체 메서드 강조 표시 하는 변경 된 줄으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-197">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="11923-198">참고: 앞의 코드 구현 하는 경우 실패 합니다 `IEmailSender` 일반 텍스트 전자 메일을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="11923-198">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="11923-199">참조 [이 문제](https://github.com/aspnet/Home/issues/2152) 자세한 내용 및 문제를 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-199">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="11923-201">계정 확인을 사용할 수 있도록 코드 주석 처리를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-201">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="11923-202">참고: 다음 줄을 주석 처리에 자동으로 로그온에서 새로 등록 된 사용자를 또한 방지 하는 것:</span><span class="sxs-lookup"><span data-stu-id="11923-202">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="11923-203">코드의 주석 처리 하 여 암호 복구 사용는 `ForgotPassword` 작업에는 *Controllers/AccountController.cs* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="11923-204">Form 요소에 주석 처리 제거 *Views/Account/ForgotPassword.cshtml*합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="11923-205">제거 하려는 경우도 `<p> For more information on how to enable reset password ... </p>` 이 문서에 대 한 링크를 포함 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="11923-206">등록 전자 메일을 확인 하 고 암호를 다시 설정</span><span class="sxs-lookup"><span data-stu-id="11923-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="11923-207">웹 응용 프로그램을 실행 하 고 계정 확인 및 암호 복구 흐름을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="11923-208">응용 프로그램을 실행 하 고 새 사용자 등록</span><span class="sxs-lookup"><span data-stu-id="11923-208">Run the app and register a new user</span></span>

 ![웹 응용 프로그램 계정 등록 보기](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="11923-210">계정 확인 링크에 대 한 전자 메일을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="11923-211">참조 [전자 메일을 디버그](#debug) 전자 메일을 얻지 못한 경우.</span><span class="sxs-lookup"><span data-stu-id="11923-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="11923-212">사용자의 전자 메일 확인에 대 한 링크를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="11923-213">전자 메일 및 암호를 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-213">Log in with your email and password.</span></span>
* <span data-ttu-id="11923-214">로그 오프 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="11923-215">관리 페이지 보기</span><span class="sxs-lookup"><span data-stu-id="11923-215">View the manage page</span></span>

<span data-ttu-id="11923-216">브라우저에서 자신의 사용자 이름을 선택: ![사용자 이름으로 브라우저 창](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="11923-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="11923-217">사용자 이름을 보려면 탐색 모음을 확장 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-217">You might need to expand the navbar to see user name.</span></span>

![탐색 모음](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11923-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11923-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="11923-220">관리 페이지에 표시 됩니다는 **프로필** 탭이 선택 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="11923-221">**전자 메일** 확인 전자 메일을 나타내는 확인란을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="11923-221">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![관리 페이지](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11923-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11923-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="11923-224">이 자습서의 뒷부분에 나오는이 페이지에 대 한 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="11923-224">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="11923-225">![관리 페이지](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="11923-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="11923-226">테스트 암호 재설정</span><span class="sxs-lookup"><span data-stu-id="11923-226">Test password reset</span></span>

* <span data-ttu-id="11923-227">로그인 되어 있는 경우 선택 **로그 아웃**합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-227">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="11923-228">선택 된 **로그인** 연결 하 고 선택의 **암호를 잊으셨나요?** 링크 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="11923-229">계정 등록에 사용한 전자 메일을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="11923-230">암호를 다시 설정에 대 한 링크가 포함 된 메일이 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-230">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="11923-231">전자 메일을 확인 하 고 암호를 다시 설정에 대 한 링크를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-231">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="11923-232">암호가 성공적으로 다시 설정, 전자 메일 및 새 암호 로그인 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-232">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="11923-233">전자 메일을 디버그</span><span class="sxs-lookup"><span data-stu-id="11923-233">Debug email</span></span>

<span data-ttu-id="11923-234">전자 메일 작업을 가져올 수 없습니다 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="11923-234">If you can't get email working:</span></span>

* <span data-ttu-id="11923-235">검토는 [전자 메일 작업](https://sendgrid.com/docs/User_Guide/email_activity.html) 페이지.</span><span class="sxs-lookup"><span data-stu-id="11923-235">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="11923-236">스팸 폴더를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-236">Check your spam folder.</span></span>
* <span data-ttu-id="11923-237">다른 전자 메일 별칭에는 다른 전자 메일 공급자 (Microsoft, Yahoo, Gmail 등)를 시도 하십시오.</span><span class="sxs-lookup"><span data-stu-id="11923-237">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="11923-238">만들기는 [전자 메일을 보내는 콘솔 응용 프로그램](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-238">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="11923-239">다른 전자 메일 주소로 보내 보십시오.</span><span class="sxs-lookup"><span data-stu-id="11923-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="11923-240">**참고:** 보안 모범 사례는 테스트 및 개발에서 생산 암호를 사용 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-240">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="11923-241">Azure에 응용 프로그램을 게시 하는 경우에 Azure 웹 앱 포털에서 응용 프로그램 설정으로 SendGrid 암호를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="11923-242">구성 시스템에는 환경 변수에서 키를 읽을 설정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-242">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="11923-243">등록 시 로그인을 방지</span><span class="sxs-lookup"><span data-stu-id="11923-243">Prevent login at registration</span></span>

<span data-ttu-id="11923-244">현재 템플릿과 함께 사용자 등록 양식을 완료 되는 로그인 (인증).</span><span class="sxs-lookup"><span data-stu-id="11923-244">With the current templates, once a user completes the registration form, they are logged in (authenticated).</span></span> <span data-ttu-id="11923-245">일반적으로 자신의 전자 메일 기록 하기 전에 확인 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-245">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="11923-246">아래 섹션에 것을 요구 하도록 코드 수정 기록 하기 전에 새로운 사용자가 확인 된 전자 메일입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-246">In the section below, we will modify the code to require new users have a confirmed email before they are logged in.</span></span> <span data-ttu-id="11923-247">업데이트는 `[HttpPost] Login` 작업에는 *AccountController.cs* 다음 강조 표시 된 변경 내용과 파일을 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-247">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="11923-248">**참고:** 보안 모범 사례는 테스트 및 개발에서 생산 암호를 사용 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-248">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="11923-249">Azure에 응용 프로그램을 게시 하는 경우에 Azure 웹 앱 포털에서 응용 프로그램 설정으로 SendGrid 암호를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-249">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="11923-250">구성 시스템에는 환경 변수에서 키를 읽을 설정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-250">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="11923-251">공유 및 로컬 로그인 계정을 결합합니다</span><span class="sxs-lookup"><span data-stu-id="11923-251">Combine social and local login accounts</span></span>

<span data-ttu-id="11923-252">참고:이 부분 ASP.NET Core에만 적용 1.x 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-252">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="11923-253">ASP.NET에 대 한 핵심 2.x, 참조 [이](https://github.com/aspnet/Docs/issues/3753) 문제입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-253">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="11923-254">이 섹션을 완료 하려면 외부 인증 공급자를 먼저 활성화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-254">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="11923-255">참조 [Facebook, Google 및 다른 외부 공급자를 사용 하 여 사용 하도록 설정 하면 인증](social/index.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-255">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="11923-256">사용자 전자 메일 링크를 클릭 하 여 로컬 및 소셜 계정을 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-256">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="11923-257">그러나 다음 순서로 "RickAndMSFT@gmail.com"; 로컬 로그인으로 처음 만들어질를 만들어 계정을 소셜 로그인으로 먼저, 다음 로컬 로그인을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-257">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![웹 응용 프로그램: RickAndMSFT@gmail.com 인증 된 사용자](accconfirm/_static/rick.png)

<span data-ttu-id="11923-259">클릭는 **관리** 링크 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-259">Click on the **Manage** link.</span></span> <span data-ttu-id="11923-260">이 계정에 연결 된 참고 0 외부 (소셜 로그인)</span><span class="sxs-lookup"><span data-stu-id="11923-260">Note the 0 external (social logins) associated with this account.</span></span>

![보기를 관리](accconfirm/_static/manage.png)

<span data-ttu-id="11923-262">다른 로그인 서비스에 대 한 링크를 클릭 하 고 응용 프로그램 요청을 수락 합니다.</span><span class="sxs-lookup"><span data-stu-id="11923-262">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="11923-263">아래 이미지 Facebook 외부 인증 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="11923-263">In the image below, Facebook is the external authentication provider:</span></span>

![Facebook을 나열 하 여 외부 로그인 보기를 관리 합니다.](accconfirm/_static/fb.png)

<span data-ttu-id="11923-265">두 개의 계정은 결합 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-265">The two accounts have been combined.</span></span> <span data-ttu-id="11923-266">두 계정으로 로그온 할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="11923-266">You will be able to log on with either account.</span></span> <span data-ttu-id="11923-267">경우에 대비 다운 되는 인증 서비스에서 소셜의 로그 또는 쓰지만 대개은 स ॅ स 소셜 계정으로 로컬 계정을 추가 하려면 사용자가 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="11923-267">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>