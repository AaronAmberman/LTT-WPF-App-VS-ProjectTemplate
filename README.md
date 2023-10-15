# LTT-WPF-App-VS-ProjectTemplate
A Visual Studio (VS) Project Template file for my [LTT-WPF-Application](https://github.com/AaronAmberman/LTT-WPF-Application) template.

# Installing and Removing
I didn't make a VSIX installer and had my VS export the template so it was already where it needed to be when I wanted to use it. 

I believe **you** can just copy (or delete if you wanted to remove) the ZIP file in 2 or 3 places...

1. {Documents}\Visual Studio {Number}\My Exported Templates
2. {Documents}\Visual Studio {Number}\Templates\ProjectTemplates
3. {User}\AppData\Roaming\Microsoft\VisualStudio {Number}\ProjectTemplatesCache (this entry only needs to be done for deleting and not adding)

This will add or remove the project template from VS for you. If VS was running you will need to restart it for the changes to take affect (I believe, not 100% sure).

# What Is It?
It is a project template for VS that will generate a complex WPF application base that is setup for MVVM development with a ServiceLocator pattern tied in. It has a built in logger, using my [SimpleRollingLogger](https://github.com/AaronAmberman/SimpleRollingLogger) API (NuGet package [here](https://www.nuget.org/packages/SimpleRollingLogger/)), has a built in translation mechanism using my [WPF.Translation](https://github.com/AaronAmberman/WPF.Translations) API (NuGet package [here](https://www.nuget.org/packages/WPF.Translations/)) and has built in theming (light and dark mode). The theme work comes from my [WPF.Themes](https://github.com/AaronAmberman/WPF.Themes) repo, there is no NuGet package for that.

# What Do You Get?
There is a lot in this project template so let's go over what is included...

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/48e840f2-1d44-4f3b-a9d0-97ec01a061f4)

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/37b15ca2-8140-4ab7-9642-72d73eea6c27)

We will just got from top to bottom as there is a bit to go over and all important.

## Settings
There is settings file that has 3 settings inside of it.
- Language -> allows the user to change the language/culture currently set for the application.
- LogPath -> allow the user to specify a custom log path at runtime.
- Theme -> allows the user change the theme at runtime (light or dark)

## Converters
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

This is nice because now as the user resizes the window it will stay 25% at all times, unless the user breaks the dynamic sizing by manually sizing the column.

## Services
There is a simple service that helps manage theming.
- Themingservice

## Theming
Theming in WPF is a complex topic and you should look into it a bit if you want to better understand how it works. A topical description would be, a StaticResource can't change at runtime unless the ControlTemplate is reapplied (meaning you will have to new up the control again that uses that resource to see the change). A DynamicResource can be changed at runtime and the rendering engine will accommodate the change and update. There is way more to it than those 2 simple sentences but it boils down to using DynamicResources ***properly*** to achieve theming.

Theming is broken down into 2 sub folders, dark and light. Each folder contains a collection of XAML resource dictionaries that contain the appropriate resources. Lets break each one down.

- AppTheme.xaml = This file contains colors, brushes, control templates, styles and other resource specific to the application. This can including overridden styles or templates.
- Brushes = This file contains the collection of brushes that the overridden templates use. Notice the ```options:Freeze="True"``` as it is very important. Look it up if you want to know more!
- Colors = This file contains the collection of colors that the overridden templates use.
- CustomControlsDarkThem/CustomControlLightTheme = This file contains overridden styles for my custom controls in [WPF.AA.CustomControls](https://github.com/AaronAmberman/WPF.AA.CustomControls), NuGet package [here](https://www.nuget.org/packages/WPF.AA.CustomControls/).
- DarkTheme/LightTheme = This file contains the overridden styles for the WPF built in controls that I wanted retemplate (because I commonly use them). This work comes from my [WPF.Themes](https://github.com/AaronAmberman/WPF.Themes) repo. There are certain controls not retemplated, see the repo for details.

Theme and ThemeDictionary help in managing theming for the application. Please check them out if you are curious.

It is suggested that you write styles custom for your application in AppTheme.xaml as that it is what its intended purpose is. However, if you know what you are doing enough then feel free to move things where you see fit. Hopefully the organized structure to everything makes it easy for you to do so. If you would like to add a 3rd theme then just copy either the Dark of Light directory. The only thing you should need to change is the Colors.xaml file to change how it looks. You SHOULD NOT change your ControlTemplate for the controls between themes! ControlTemplates from my experience need to remain the same between themes but their color scheme can be changed with DynamicResources. If changing the ControlTemplate then you will need to new up the control(s) again. This becomes difficult to manage so it is suggested to leave the templates the same and just change colors.

## Translations
Translations are managed with my [WPF.Translations](https://github.com/AaronAmberman/WPF.Translations) API, NuGet [here](https://www.nuget.org/packages/WPF.Translations/). The API can be utilized slightly differently if you desire but I skipped the usage of the Translator and just used the Translation directly. Either way works, see the repo for details.

### How Do Translations Work?
You have control over that and can redo what I did. What I did though was this...

Setup a nomenclature where the translations file would be called Translations.XX[XXX].xaml. So something like Translations.en.xaml or Translations.es.xaml; you could even use something like Translations.en-GB.xaml. So then in settings we set what the dynamic portion of the file name is, so for Spanish it would just be es (as my application has it set because I have no variation to the culture like es-MX). So it just boils down to what the filename is and what the culture is saved as. It is currently set like this...

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/69063b5c-8b00-4144-9f1e-236a821ee433)

Notice how the culture matches what you see in the Solution Explorer (in VS) for the translation files. This is how the matching occurs. There is no magic in the API for this, I set it in the application...

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/23d517c0-f377-40d6-8a5a-5f43ba8db5a5)

***All translation files are marked as Resource in properties so that I can use pack:///application:,,, strings to reference them.***

![image](https://github.com/AaronAmberman/LTT-WPF-App-VS-ProjectTemplate/assets/23512394/e86cbed4-52ac-46d3-90ff-d4bb95b02f9e)

You can change this too if you see fit.
