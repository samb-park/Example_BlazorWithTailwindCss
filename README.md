# TailwindCssInBlazor
1. Create a directory "Styles" on root

2. Create an app.css on root and write below code.

		  @tailwind base;
		  @tailwind components;
		  @tailwind utilities;

3. Create a package.json on root and write below code.

		{
			"scripts": {
			"buildcss:dev": "postcss ./Styles/app.css -o ./wwwroot/css/site.css"
			},	  
		 {
			"scripts": {
			"buildcss:dev": "postcss ./Styles/app.css -o ./wwwroot/css/site.css"
			},	  
			"devDependencies": {
			"cross-env": "^7.0.3",
			"autoprefixer": "^10.3.1",
			"postcss": "^8.3.5",
			"postcss-cli": "^8.3.1",
			"tailwindcss": "^2.2.4"
		   }

 }


		}

4. Create a postcss.config.js on root and write below code.
		
		module.exports = {
		  plugins: {
			tailwindcss: {},
			autoprefixer: {},
		  }
		}

5. Create a tailwind.config.js on root and write below code.

	
		module.exports = {
		purge: [],
		darkMode: false, // or 'media' or 'class'
		theme: {
			extend: {},
		},
		variants: {},
		plugins: [],
		}

6. Add below code on *.csproj

	<Project Sdk="Microsoft.NET.Sdk.Web">

			<PropertyGroup>
					<TargetFramework>net5.0</TargetFramework>
					<NpmLastInstall>node_modules/.last-install</NpmLastInstall> <!--This code -->
			</PropertyGroup>
	<!--From this code -->
			<Target Name="CheckForNpm" BeforeTargets="NpmInstall">
					<Exec Command="npm -v" ContinueOnError="true">
							<Output TaskParameter="ExitCode" PropertyName="ErrorCode" />
					</Exec>
					<Error Condition="'$(ErrorCode)' != '0'" Text="You must install NPM to build this project" />
			</Target>

			<Target Name="NpmInstall" BeforeTargets="BuildCSS" Inputs="package.json" Outputs="$(NpmLastInstall)">
					<Exec Command="npm install" />
					<Touch Files="$(NpmLastInstall)" AlwaysCreate="true" />
			</Target>

			<Target Name="BuildCSS" BeforeTargets="Compile">
					<Exec Command="npm run buildcss:dev" Condition=" '$(Configuration)' == 'Debug' " />
					<Exec Command="npm run buildcss:release" Condition=" '$(Configuration)' == 'Release' " />
			</Target>
	<!--To this code -->
	
	</Project>

7. Build and check if site.css is created properly. It must be about 4MB.

8. Remove the below code in _Host.cshtml or index.html if you don't use bootstrap.
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css"/>
