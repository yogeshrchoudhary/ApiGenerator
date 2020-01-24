# MSBuild
* Use MSBuild to generate code files from T4 templates
From the `.csproj` directory execute the following,
```shell
msbuild /t:TransformAll
```

* Include the following in the csproj file manually
Note, the `<Import Project=...>` references a `.targets` file insite the `tools` directory.
```xml
  <PropertyGroup>
    <TransformOutOfDateOnly>false</TransformOutOfDateOnly>
    <OverwriteReadOnlyOutputFiles>true</OverwriteReadOnlyOutputFiles>
    <TransformOnBuild>true</TransformOnBuild>
  </PropertyGroup>

  <Import Project="..\tools\Microsoft.TextTemplating.targets" Condition="" />
  <Target Name="PreBuild" BeforeTargets="BeforeBuild">
    <CallTarget Targets="TransformDuringBuild" />
  </Target>
```

* __Issue__ `dotnet` CLI doesn't recognize some of the assemblies used for generating T4. Hence build fails.

  __Possible mitigation__ Use conditional `target` in the `.csproj` to omit the `Import` and the `BeforeBuild`