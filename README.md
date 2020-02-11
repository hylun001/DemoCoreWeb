[Strongly Typed Client API Generators](https://github.com/zijianhuang/webapiclientgen) generate strongly typed client API in C# codes and TypeScript codes. This repository contains code examples explained in the following CodeProject.com articles:

* [Generate C# Client API for ASP.NET Core Web API](https://www.codeproject.com/Articles/1243908/Generate-Csharp-Client-API-for-ASP-NET-Core-Web-AP)


And this repository contains the following demo applications:
1. CoreWebApi, ASP.NET Core Web API providing data to other test suites.
1. CoreMvc, ASP.NET Core MVC.
1. CoreNG, .NET Core + Angular 2+.
1. axios, JEST test suite using the generated TypeScript codes for Axios which is recommended by React and Vue.js.
1. vueTS, JEST test suite with Vue TypeScript and the generated TypeScript codes.


**Remarks:** 

* .NET Core 2.x had dependency on Newtonsoft.JSON, while .NET Core 3.0 had been decoupled from Neewtonsoft.JSON and the default serializer is working well in most scenarios except for Tuple, 2D array and anonymous object etc. If you would support these data types or would keep 100% compitability with the serialization of NewtonSoft.JSON, you should explicitly include package `Microsoft.AspNetCore.Mvc.NewtonsoftJson` and add add `AddNewtonsoftJson()` in `Startup.cs`.
* This SLN also include Swagger demos, so you may compare WebApiClientGen with Swagger toolchain in the landscape of .NET Core.


## WebApiClientGen vs Swagger

Swagger here means Swashbuckle.AspNetCore on ASP.NET Core Web API plus NSwagStudio for generating client codes. 

### C# Clients
**Swagger does not support:**
1. User defined struct.
2. Object
3. dynamic

**Swagger does not give exact data type mapping for the following types:**
1. Decimal ==> double
2. Nullable\<T\> ==> T
3. float ==>double
4. uint, short, byte ==> int
5. ulong ==> long
6. char==> string
7. Tuple ==> Generated user defined type with similar structure to Tuple
8. int[,] ==> ICollection\<object\>
9. int[][] ==> ICollection\<int\>
10. KeyValuePair ==> Generated user defined type with similar structure to KeyValuePair


**Swagger generates verbose, larger and complex codes:**

When generating async functions only, codes generated by WebApiClientGen is 97KB, along with debug build 166KB and release build 117KB, while Swagger gives 489KB-495KB, along with debug build 340KB-343KB and release build 263KB-283KB.

**Swagger yield verbose GeneratedCodeAttribute**

According to https://docs.microsoft.com/en-us/dotnet/api/system.codedom.compiler.generatedcodeattribute?view=netcore-3.1 , the GeneratedCodeAttribute class can be used by code analysis tools to identify computer-generated code, and to provide an analysis based on the tool and the version of the tool that generated the code.

It is a good practice to put generated codes into a dedicated assembly with generated codes only. Thus an application programmer may simply exclude the assembly from code analysis tools. Therefore GeneratedCodeAttribute is not necessary in the generated codes.



### How WebApiClientGen is more superior to Swagger?

For generating C# clients, WebApiClientGen supports more .NET built-in data types and give exact data type mappings. Exact type mappings make client programming much easier for high quality since the integration tests should pick up data out of range easily because of proper data constraints.

Smaller codes and smaller binary are allways welcome.

The manual steps of generating client codes is less and faster.


### How Swagger is more superior to WebApiClientGen?

Swagger here means the Open API standard and respective toolchains.

Swagger is an open standard and platform neutural, being supported by major software vendors and developed by hundreds of developers around the world. Microsoft Docs has a dedicated section for Swagger at https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-3.1, and Microsoft has been using Swagger for her own Web API products.

### Can WebApiClientGen and Swagger coexist?

Swagger here means Swashbuckle.AspNetCore on ASP.NET Core Web API plus NSwagStudio for generating client codes.

Yes.

These two products are greately overlapping in the .NET landscapes, while Swagger covers wider and deeper spectrum. 

If you are developing ASP.NET (Core) Web API and expect all clients are coded in C# and TypeScript only, WebApiClientGen gives you more advantages.

When you need to support clients coded in languages other than C# and TypeScript, you may introduce Swagger into your Web API and generate the Open API definition files either in JSON or YAML.

#### Perfect SDLC with WebApiClientGen and Swagger

Whenever you as a backend developer has just updated the Web API, you run WebApiClientGen with a batch file and generate C# client codes and TypeScript client codes for other client developers. And the Swagger endpoint of the Web API gives the Open API definition files, so client developers working on other languages may generate client API codes in other languages.

So you get the best of WebApiClientGen and Swagger.

