## Инструкция по сборке просмотрщика PDF Okular из набора приложений KDE под Windows 10 x64.

### Установка окуржения Craft. 

<https://community.kde.org/Get_Involved/development/Windows>

### 1. Установка Python3

<https://www.python.org/downloads/windows/>

Скачать установщик Python3. Во время установки поставить галочку в setup master `Также установить pip`.

### 2. Установка и настройка Powershell

Необходимо установить PowerShell не менее версии 5.0 .

Проверить какая версия установлена можно введя команду: `$PSVersionTable.PSVersion`

Установка с помощью пакетного менеджера winget:
Открыть cmd.exe и ввести команду.

```powershell
    winget install --id Microsoft.Powershell --source winget
```

Если winget не установлен, то его можно установить из Microsoft Store, ввести в поиске `winget` и в результатах поиска выбрать "Установщик программ".


### 3. Установка Visual Studio

! Поддерживается на данный момент только Visual Studio 2019.

Установщик можно скачать по ссылке: <https://learn.microsoft.com/en-us/visualstudio/releases/2019/history>

Необходимо установить со следующими компонентами:  

- Desktop Development with C++
- C++ ATL
- Windows SDK

![Components](https://community.kde.org/images.community/thumb/e/ee/Kdeconnect_win01.jpeg/800px-Kdeconnect_win01.jpeg?20190130212626)


Если Visual Studio уже установлена более новой версии, то можно установщиком доустановить VisualStudio2019.BuildTools.

Установка с помощью пакетного менеджера winget:

Открыть PowerShell <u>от Администратора</u> и ввести команду:

```powershell
    winget install --id Microsoft.VisualStudio.2019.BuildTools --exact --override "--includeRecommended --includeRecommendedForThisScenario --add Microsoft.VisualStudio.Workload.VCTools --add Microsoft.VisualStudio.Component.VC.ATL --add Microsoft.VisualStudio.Component.Windows10SDK.19041"
```

### 4. Включить development mode в Windows

В меню -> поиск ввести: "for developers", открыть настройки и включить режим разработчика.

![For developers](./img/for_developers.png)

![For developers](./img/devmod.png)

### 5. Установка Craft


Открыть PowerShell **от Администратора** и ввести команду:


```powershell
    Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Далее запустить установку Craft командой:

```powershell
    iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/KDE/craft/master/setup/install_craft.ps1'))
```

На все вопросы отвечаем по умолчанию.

Установка должна завершиться без ошибок. Если установщик не находит Visual Studio значит какой-то из его компонентов не установлен и надо его доустановить и запустить установочный скрипт снова.

Вывод после успешной установки должен содержать:  

```powershell
    [Environment]
    PATH=

    Craft                : C:\CraftRoot
    Version              : master
    ABI                  : windows-c1-msvc2019_x86_64
    Download directory   : C:\CraftRoot\download

```

Закрыть PowerShell **от Администратора**.

Если установлен сторонний Антивирус, то необходимо исключить директорию `C:\CraftRoot\` из области проверки.


### 6. Сборка Okular с помощью Craft

Открыть из меню консоль `Craft CraftRoot`

![Craft CraftRoot](./img/menu_craft_craftroot.png)

В консоли запустить скрипт инициализации:

```powershell
    C:\CraftRoot\craft\craftenv.ps1

```

Скрипт должен выполниться без ошибок, он укажет craft переменные окружения:

![Console CraftRoot](./img/console_craftroot.png)

Craft работает как пакетный менеджер, даже напоминает portage в Gentoo. Он может искать нужный пакет и инструкции для сборки.


```powershell
    craft --search okular

```

Запуск сборки стоковой upstream версии:

```powershell
    craft kde/applications/okular

```


Пакетный менеджер скачает все необходимые исходники, со всеми зависимостями, инструкции для сборки и соберет и установит пакет в `C:\CraftRoot\bin`.

![Console CraftRoot](./img/installed_bin.png)

Запуск тестов установленной версии:


```powershell
    craft --test kde/applications/okular

```

Посмотреть все установленные craft пакеты:


```powershell
    craft --print-installed

```

Изменить исходники и собрать модифицированную версию:


```powershell
    craft --compile kde/applications/okular

```

Упаковать собранный пакет в инсталятор:

```powershell
    craft nsis

```

```powershell
    craft --package kde/applications/okular

```

Пакетный менеджер упакует все необходимые бинарники в .7z архив и в инсталятор .exe, а также рассчитает хэши для них в `C:\CraftRoot\tmp`.


Деинсталировать установленный пакет:



```powershell
    craft --unmerge kde/applications/okular

```



