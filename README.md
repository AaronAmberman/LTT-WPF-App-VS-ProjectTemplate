# LTT-WPF-App-VS-ProjectTemplate
A Visual Studio (VS) Project Template file for my [LTT-WPF-Application](https://github.com/AaronAmberman/LTT-WPF-Application) template.

## Installing and Removing
I didn't make a VSIX installer and had my VS export the template so it was already where it needed to be when I wanted to use it. 

I believe **you** can just copy (or delete if you wanted to remove) the ZIP file in 3 places...

1. {Documents}\Visual Studio {Number}\My Exported Templates
2. {Documents}\Visual Studio {Number}\Templates\ProjectTemplates
3. {User}\AppData\Roaming\Microsoft\VisualStudio {Number}\ProjectTemplatesCache

This will add or remove the project template from VS for you. If VS was running you will need to restart it for the changes to take affect (I believe, not 100% sure).

## What Is It
It is a project template for VS that will generate a complex WPF application base that is setup for MVVM development with a ServiceLocator pattern tied in. It has a built in logger, using my [SimpleRollingLogger](https://github.com/AaronAmberman/SimpleRollingLogger) API (NuGet package [here](https://www.nuget.org/packages/SimpleRollingLogger/)), has a built in translation mechanism using my [WPF.Translation](https://github.com/AaronAmberman/WPF.Translations) API (NuGet package [here](https://www.nuget.org/packages/WPF.Translations/)) and has built in theming (light and dark mode). The theme work comes from my [WPF.Themes](https://github.com/AaronAmberman/WPF.Themes) repo, there is no NuGet package for that.

## What Do You Get
There is a lot in this project template so let's go over what is included...

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/48e840f2-1d44-4f3b-a9d0-97ec01a061f4)

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/37b15ca2-8140-4ab7-9642-72d73eea6c27)

We will just got from top to bottom as there is a bit to go over and all important.

### Settings
There is settings file that has 3 settings inside of it.
- Language -> allows the user to change the language/culture currently set for the application.
- LogPath -> allow the user to specify a custom log path at runtime.
- Theme -> allows the user change the theme at runtime (light or dark)

### Converters
Most of the converters were built in a simplistic manner. Meaning they were built to go to the same data type; int to int, bool to bool, etc. If you need expanded functionality then feel free to go in and modify the guts of the converters. The ***ArithmeticConverter*** is a special case and does not fit into the statement above.
- ArithmeticConverter
- EqualsConverter
- EqualsMultiConverter
- NotEqualsConverter
- NotEqualsMultiConverter

The ***ArithmeticConverter*** is unique in that it can assist in acheving dynamic sizing via binding. Here are two simple examples...

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/10b27a9b-ad8c-49d1-a875-77057644b691)
Here the Border will be 80% of the width of the parent containing Grid.

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/60cd9a1f-e3eb-44a3-85e3-e4d81d00f0a7)
Here the GridViewColumn will be a quarter (25%) of the width of the referenced ListView (*decksListView*).
