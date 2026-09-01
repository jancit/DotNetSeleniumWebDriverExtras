# DotNetSeleniumWebDriverExtras

Extra utilities for use with the Selenium .NET language bindings — a maintained fork of
[DotNetSeleniumTools/DotNetSeleniumExtras](https://github.com/DotNetSeleniumTools/DotNetSeleniumExtras),
rebuilt against modern Selenium.WebDriver.

## Why this fork exists

The original `DotNetSeleniumExtras` packages (`DotNetSeleniumExtras.WaitHelpers` and
`DotNetSeleniumExtras.PageObjects`) preserve the `ExpectedConditions` and `PageFactory` implementations that
were removed from the Selenium project itself. That repository has been unmaintained since 2018 and
explicitly isn't accepting issues or pull requests, so its packages are still compiled against Selenium's
old `WebDriver`/`WebDriver.Support` assemblies.

A later Selenium 4.x release renamed and strong-signed those core assemblies to
`Selenium.WebDriver`/`Selenium.Support`. That's a binary-breaking change for anything still referencing the
old assembly names — including the original `DotNetSeleniumExtras` packages, which can no longer be used
alongside current Selenium.WebDriver versions.

This fork is the same Apache-2.0 source, retargeted and recompiled against current Selenium.WebDriver. No
API or namespace changes — see [Migrating from DotNetSeleniumExtras](#migrating-from-dotnetseleniumextras)
below.

## Packages

| Package | Provides | Install |
|---|---|---|
| [DotNetSeleniumWebDriverExtras.WaitHelpers](https://www.nuget.org/packages/DotNetSeleniumWebDriverExtras.WaitHelpers) | `ExpectedConditions` for use with `WebDriverWait` | `dotnet add package DotNetSeleniumWebDriverExtras.WaitHelpers` |
| [DotNetSeleniumWebDriverExtras.PageObjects](https://www.nuget.org/packages/DotNetSeleniumWebDriverExtras.PageObjects) | `PageFactory` and `[FindsBy]` page-object support | `dotnet add package DotNetSeleniumWebDriverExtras.PageObjects` |

Both target `net462` and `netstandard2.0`.

## Usage

### WaitHelpers

```csharp
using SeleniumExtras.WaitHelpers;

var wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
IWebElement element = wait.Until(ExpectedConditions.ElementIsVisible(By.Id("myElement")));
```

### PageObjects

```csharp
using SeleniumExtras.PageObjects;

public class LoginPage
{
    [FindsBy(How = How.Id, Using = "username")]
    private IWebElement UsernameField { get; set; }

    public LoginPage(IWebDriver driver) => PageFactory.InitElements(driver, this);
}
```

## Migrating from DotNetSeleniumExtras

Uninstall the old package, install the new one — no source changes required. Assembly names, root
namespaces, and public APIs (`SeleniumExtras.WaitHelpers.ExpectedConditions`,
`SeleniumExtras.PageObjects.PageFactory`, etc.) are unchanged, so existing `using` statements keep working.

```bash
dotnet remove package DotNetSeleniumExtras.WaitHelpers
dotnet add package DotNetSeleniumWebDriverExtras.WaitHelpers
```

## Versioning

`<selenium-major>.<selenium-minor>.<fork-patch>` — the major/minor version tracks the Selenium.WebDriver
line each build targets; the patch number is this fork's own and increments independently for fixes that
don't correspond to a Selenium bump.

## License

Apache License 2.0 — see [LICENSE](LICENSE). Copyright 2018, Software Freedom Conservancy (original code);
fork maintained by [jancit](https://github.com/jancit).
