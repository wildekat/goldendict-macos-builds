# goldendict-macos-builds

Неофициальные сборки раннего доступа программы GoldenDict для macOS

Собрано на базе Qt 5.14.2 и QtWebKit 5.212.0 Alpha 4  
Работоспособность протестирована на Mojave 10.14.6 и Monterey 12.6  

> [!IMPORTANT]
> Настоящие сборки предоставляются как есть, без каких-либо гарантий, исключительно в ознакомительных целях.

## Описание

Сборки предоставляются в двух версиях: `master` и `mac-adapted`.  

Версия `master` собирается из исходного кода [официального репозитория GoldenDict](https://github.com/goldendict/goldendict) без изменений и дополнений, как есть.  

Версия `mac-adapted` собирается на основе исходного кода официального репозитория GoldenDict, с внесением косметических изменений и дополнений для лучшей адаптации к внешнему виду macOS. С историей изменений можно ознакомиться в [ответвлении кода GoldenDict](https://github.com/yozhic/goldendict/tree/mac-adapted), где они производятся. Вкратце, они включают: новые иконки для панелей инструментов, дополнительные стили для окон программы и словарных статей.  

Отличия между двумя версиями проиллюстрированы ниже.  

## Настройки

Для работоспособности таких функций, как Всплывающее окно и Глобальные горячие клавиши, необходимо предоставить GoldenDict разрешения на управление Mac через функции универсального доступа. Для этого открываем меню Apple :green_apple: → Системные настройки → Защита и безопасность → вкладка Конфиденциальность. Разблокируем возможность редактирования щелчком по иконке замочка :lock: внизу окна. В левой панели выбираем Универсальный доступ, в правой жмём кнопку Добавить :heavy_plus_sign:, находим и выделяем `GoldenDict.app`, жмём Открыть. В списке приложений отмечаем флажок :ballot_box_with_check: слева от `GoldenDict.app`[^1].  

[^1]: Источник: [Apple Support: Разрешение доступа к компьютеру Mac программам Универсального доступа](https://support.apple.com/ru-ru/guide/mac-help/mh43185/10.14/mac/10.14)  

В силу особенностей работы macOS, процедуру предоставления разрешений необходимо повторять после каждого обновления GoldenDict. Для этого в указанном диалоге выделяем строку `GoldenDict.app` и жмём кнопку Удалить :heavy_minus_sign:. Затем жмём кнопку Добавить :heavy_plus_sign: и далее, как описано выше.  

Дополнительные стили сборки `mac-adapted` включаются в настройках программы, на вкладке `Интерфейс`, в выпадающем списке `Стиль интерфейса`. Стили синхронизированы с системными настройками оформления macOS, т.е. при включенном системном оформлении `Light` следует использовать стиль `macOS Light`, а при системном `Dark` — один из двух тёмных стилей, `macOS Dark` или `macOS Dark Deep`.  

## Известные проблемы

1. Если в других приложениях не работает копирование текста по сочетанию клавиш <kbd>Cmd</kbd>+<kbd>C</kbd>, необходимо предоставить GoldenDict разрешение на Универсальный доступ. Описание этой процедуры см. выше. Можно также переназначить это сочетание в настройках GoldenDict → Горячие клавиши.

2. Функция Всплывающее окно работает на последних версиях macOS не так хорошо, как на Windows. Рекомендуется воздержаться от её использования и отключить её в настройках.  

## Отличия

Сравнение версий: окно программы, системный стиль `Light`  

![COMPARE LIGHT](https://github.com/yozhic/goldendict-macos-builds/blob/main/screenshots/COMPARE_LIGHT.png)  

Сравнение версий: окно программы, системный стиль `Dark`  

![COMPARE DARK](https://github.com/yozhic/goldendict-macos-builds/blob/main/screenshots/COMPARE_DARK.png)  

Сравнение версий: окно программы, системный стиль `Dark`, стиль окна `macOS Dark Deep`  

![COMPARE DARK DEEP](https://github.com/yozhic/goldendict-macos-builds/blob/main/screenshots/COMPARE_DARK_DEEP.png)  

Сравнение версий: диалог настроек, системный стиль `Light`  

![COMPARE PREFS](https://github.com/yozhic/goldendict-macos-builds/blob/main/screenshots/COMPARE_PREFS.png)  

## Скачать

Готовые к использованию сборки, упакованные в контейнер `dmg`, размещаются в разделе [Releases](https://github.com/yozhic/goldendict-macos-builds/releases).  

## Самостоятельная сборка

1. Скачиваем установщик Qt. Последний в свободном доступе: [5.14.2](https://download.qt.io/archive/qt/5.14/5.14.2/).  

2. Устанавливаем Qt. В процессе понадобится подключиться к личному кабинету Qt (если не существует, создаём). Путь для установки, например: `/Users/yozhic/Qt`. В окне выбора компонентов отмечаем `Qt 5.14.2` → `macOS` (`Developer` отмечен по умолчанию).  

3. Скачиваем QtWebKit 5.212.0 Alpha 4, скомпилированные бинарники для macOS: [qtwebkit-MacOS-MacOS_10_13-Clang-MacOS-MacOS_10_13-X86_64.7z](https://github.com/qtwebkit/qtwebkit/releases). Распаковываем архив, копируем всю структуру папок в `~/Qt/5.14.2/clang_64`.  

4. Клонируем головной репозиторий `goldendict`:  

   ```sh
   cd ~/git && git clone https://github.com/goldendict/goldendict
   ```
   
5. Запускаем создание проекта и сборку:  

   ```sh
   cd ~/git/goldendict && ~/Qt/5.14.2/clang_64/bin/qmake && make
   ```

6. Упаковываем собранный `GoldenDict.app` необходимыми библиотеками:  

   ```sh
   ~/Qt/5.14.2/clang_64/bin/macdeployqt GoldenDict.app
   ```

   Можно также сразу создать `dmg`:  
   
   ```sh
   ~/Qt/5.14.2/clang_64/bin/macdeployqt GoldenDict.app -dmg
   ```
